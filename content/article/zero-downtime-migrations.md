---
title: "Zero Downtime Migrations"
date: "2018-10-30"
type: "article"
---

## Introduction

Say you are maintaining an e-commerce application that has a catalog of 100 products stored in a table named `product`
in an RDBMS database, say PostgreSQL. Now, let's add a new column named `discount` to the table.
Let's use this query:

```sql
ALTER TABLE product ADD COLUMN "discount" INTEGER NOT NULL DEFAULT 0;
```

When you run this query against the database, it will appear to be instantaneous.
However, consider the case where you have a hundred thousand products in your table. The addition of the new column will
not be as fast. All application code that depends on writing something to that table would break.

Now, assume that the same table has another column `user`, which stores the name of user who added this product. Say we
want to rename the column to `creator`, since we think that this name makes more sense.
We can write a query to do that:

```sql
ALTER TABLE product RENAME user TO creator;
```

Now we can go ahead and update the code in all the instances to use the column name `creator` instead of `user`, right?

Not so correct. While this would work when you are developing your app, or when you know that there are no users using
the application, this would break for anyone who tries to access the app before the application code is updated.

To solve these problems, and more, read on.

## But I have done all of that, never faced any issues.

That is probably because:

1. There weren't many rows in your table, due to which the downtime was unnoticeable.
2. There weren't any users using your application at that time.

# Concepts

1. Changes in the database schema take time. The amount of time depends on the operation that is to be undertaken. For example, the time required to add a new column with default depends on the number of rows in that table.
2. The application code should be in sync with the database schema as much as possible. For example, renaming a column of a table needs update in the application code as well. If the application still tries to access the old column name, that part of the code will break.
3. Table locks should be avoided. Such locks render the entire table unusable for some time. Hence, any part of the application that depends on a non-read operation in this table will be unusable. Some operations, like adding indices on a table, require the table to be locked.

## Examples

### 1. Avoiding table locks

**Adding a column to a table with defaults**

Adding a new column to a table is really fast. However, when a column is added with a default, it requires a full table lock to add the default value to existing rows.

**Fix**

1. Add the column to the table without any default.
2. Create another migration that sets the default on the column.  This time, the table will not be locked and the default will only be set for any new row that is written.
3. For all old rows, write a script that updates the rows one by one.

### 2. Keeping the application code in sync with the new schema changes

**Renaming a column**

Directly renaming a table column will result in the column still being accessed by the application. Changing the application code first and then renaming the column would not work either.

**Fix**

Assuming that column `user` is to be renamed to `creator`.

1. Add a new column named `creator`.
2. Update the application code to write to both columns, `creator` and `user`.
3. For all existing rows, copy the data from `user` column to `creator` column.
4. When done, drop the `user` column.

## What else can cause downtimes?

1. Changing a column type
2. Adding indices on a table
3. Removing a column from a table
4. Renaming a table

## Next Steps

The above outlined concepts will help ensure zero downtime when changing your database's schema. Ensuring these rules are a bit hard.
There are a few libraries (like
[this](https://github.com/tbicr/django-pg-zero-downtime-migrations/blob/master/README.md)) that change the SQL such that these issues are minimized.

There is another way to ensure these practices:

Write a test that:

1. Checks for migrations that do something similar to what's outlined above
2. Raises an error if one or more such migrations exist
3. Ignores migrations that contain a special flag, like `IGNORE_THIS_MIGRATION` (useful in ORMs)

The ignore flag is important because once you have reviewed the migrations and made it zero-downtime-ready, the tests
will still fail (for example, if a column is dropped in the last step, as explained in the example where we rename a
column).

---

Postgres has added a new feature to ensure faster column creation with defaults. Read it [here](https://brandur.org/postgres-default).

There are some issues with building concurrent indexes. Read about them [here](https://www.postgresql.org/docs/9.1/static/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY).
