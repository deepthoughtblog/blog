---
title: "Zero-downtime Database Migration"
date: "2018-10-30"
type: "article"
---

Let's say you maintain an e-commerce application that uses an RDBMS, like PostgreSQL, to store data. Your application has a catalog of 100 products which you store in a table named `product`. Now assume you need to add a column to the table, called `discount`. 
<!--more-->

<span class="marginnote">
A relational database management system (RDBMS) is a database system based on the [relational model](https://en.wikipedia.org/wiki/Relational_model). It stores data in a structured format, using rows and columns.
</span>

Here's the query which you could use:

```sql
ALTER TABLE product ADD COLUMN "discount" INTEGER NOT NULL DEFAULT 0;
```

When you run this query against the database, it will _appear_ to be instantaneous. However, if you happen to have a hundred thousand products in your table, the addition of the new column will not be as fast. All application code that depends on writing something to that table while the query is still running would break.

Now, assume that the same table has another column `user`, which stores the name of the user who added this product. Say you
want to rename the column to `creator`, since you think that makes more sense. You can write this query to do that:

```sql
ALTER TABLE product RENAME user TO creator;
```

Now we can go ahead and update the code in all the instances to use the column name `creator` instead of `user`, right?

Not so fast. While this would work without a glitch when you're locally developing your app, in real life, the database would be accessed by more than one instances of application code. Renaming column would break for all instances of your application which have the older version of the code prior to this change &mdash; which is a common scenario during new deployments.

## "But I have done all of that, and never faced any issues!"

And you are right. But that could probably be because:

1. There weren't many rows in your table, due to which the downtime was unnoticeable.
2. There weren't any users using your application at that time.
3. There aren't many instances running your application code, and your deployment takes very less time owing to simplicity.

## Concepts

<span class="newthought">Changes in the database schema</span> take time. The amount of time depends on the operation that is to be undertaken. For example, the time required to add a new column with default depends on the number of rows in that table.

<span class="newthought">The application code</span> must be in sync with the database schema at all times. For example, renaming a column of a table needs update in the application code as well. If older versions of your application still try to access the old column name, that part of the code will break.

<span class="newthought">Table locks<label for="sn-tablelock" class="margin-toggle sidenote-number"></label></span> <input type="checkbox" id="sn-tablelock" class="margin-toggle"/><span class="sidenote">The primary purpose of table-level locks is to block reads and/or writes when changes to the underlying table structure are made during DDL commands such as `ALTER TABLE`. However, not all DDL commands need to block reads or writes, some only block each other.</span> should be avoided. Such locks render the entire table unusable for some time. Hence, any part of the application that depends on a non-read operation in this table will be unusable. Some operations, like adding indexes on a table require the table to be locked.


## Example 1: Avoiding table locks

### Adding a column with defaults

Adding a new column to a table is really fast. However, when a column is added with a default, it requires a full table lock to add the default value to existing rows.

**Fix**

1. Add the column to the table without any default.
2. Create another migration that sets the default on the column.  This time, the table will not be locked and the default will only be set for any new row that is written.
3. For all old rows, write a script that updates the rows one by one.

## Example 2: Keeping the application code in sync with the schema

### Renaming a column

Directly renaming a table column will result in the column still being accessed by the application. Changing the application code first and then renaming the column would not work either.

**Fix**

Assuming that column `user` is to be renamed to `creator`.

1. Add a new column named `creator`.
2. Update the application code to write to both columns, `creator` and `user`.
3. For all existing rows, copy the data from `user` column to `creator` column.
4. When done, drop the `user` column.


## What else can cause downtime?

1. Changing a column type
2. Adding indexes on a table
3. Removing a column from a table
4. Renaming a table

## Next steps

The above concepts will help ensure zero downtime when changing your database's schema. Ensuring these rules are followed is a bit difficult, though. There are a few libraries (like
[this](https://github.com/tbicr/django-pg-zero-downtime-migrations/blob/master/README.md)) that change the SQL such that these issues are minimized.

There is another way to ensure these practices:

Write a test that:

1. Checks for migrations that do something similar to what's outlined above
2. Raises an error if one or more such migrations exist
3. Ignores migrations that contain a special flag, like `IGNORE_THIS_MIGRATION` (useful in ORMs)

The ignore flag is important because once you have reviewed the migrations and made it zero-downtime-ready, the tests will still fail (for example, if a column is dropped in the last step, as explained in the example where we rename a column).

## Further reading

Postgres has added a new feature to ensure faster column creation with defaults. Read it [here](https://brandur.org/postgres-default).

There are some issues with building concurrent indexes. Read about them [here](https://www.postgresql.org/docs/9.1/static/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY).
