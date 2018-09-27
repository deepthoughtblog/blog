---
title: "What indexes should I create on a table?"
date: "2018-09-23"
type: "article"
---

While designing a software system that involves a database, this question inevitably
pops up in the minds of software developers. Indexes improve the speed of data retrieval operations,
and who wouldn't like their queries to be faster? Aside, database design is such an important decision in
the lifetime of the software. Your application code is most likely to change over a few years, unlike your database schema which 
is most likely to stay the same.<label for="sn-data-maturity" class="margin-toggle sidenote-number"></label>
<input type="checkbox" id="sn-data-maturity" class="margin-toggle"/>
<span class="sidenote">
  <em>"Data matures like wine, applications like fish."</em><br><br>
  A fitting example
  pointed [here](https://www.reaktor.com/blog/applications-age-like-fish-data-ages-like-wine/) is about
  [Last.fm](https://web.archive.org/web/20150401045306/http://www.last.fm/forum/21717/_/46047), who were not
  able to allow change of usernames despite ten years of feature requests.
</span> This article helps you understand the idea of database indexes from the fundamentals, so you
can decide when to use one.


## What's an index?

Let's assume you have a table in your database similar to this:

```sql
  CREATE TABLE blogpost (
    id integer,
    slug varchar,
    content varchar
  );
```

And your application fetches individual blog posts frequently, using queries like this:
```sql
  SELECT content from blogpost WHERE slug = "greatest-post-ever";
```

To find all matching entries, the system would have to scan the entire `blogpost` table,
row by row. Assuming there are many rows in the table, and only a few rows (in this case, zero or one)
that would match the query, this is clearly an inefficient method. Instead, if the system maintains an
_index_<label for="sn-index" class="margin-toggle sidenote-number"></label>
<input type="checkbox" id="sn-index" class="margin-toggle"/>
<span class="sidenote">A great analogy is the alphabetical index of frequently looked up terms that you see
at the back of most non-fiction books. You can scan the index and quickly flip to the appropriate pages, rather than read the entire book.</span>
on the column `slug`, it can use a more efficient method for locating matching rows. Databases use data structures like
[balanced trees](https://en.wikipedia.org/wiki/Balanced_tree), [B+ trees](https://en.wikipedia.org/wiki/B%2B_tree) and [hashes](https://en.wikipedia.org/wiki/Hash_table)
to implement indexes; and allow you to create [different types](https://en.wikipedia.org/wiki/Database_index#Types_of_indexes) of indexes to suit your needs.


## Which fields should be indexed?

Here are four simple rules thumb to keep in mind:

 1. Index every [primary key](https://en.wikipedia.org/wiki/Primary_key).
 2. Index every [foreign key](https://en.wikipedia.org/wiki/Foreign_key).
 3. Index every column used in a `JOIN` clause.
 4. Index every column used in a `WHERE` clause.

<span class="newthought">If you are using a modern database,</span> chances are the system will automatically create some essential indexes for you -- like those on primary
keys and foreign keys. It's very helpful to understand what kind of queries are going to be made by the application, like in our example earlier,
to figure out which additional columns should be indexed. Patterns like [multicolumn indexes](https://www.postgresql.org/docs/current/static/indexes-multicolumn.html)
are a helpful pattern when you frequently make queries involving lookups on more than one column together.

## Here be dragons.

Database indexes are indeed a great idea, and you might be tempted to _index all the things_<label for="sn-index-all" class="margin-toggle sidenote-number"></label>
<input type="checkbox" id="sn-index-all" class="margin-toggle"/>
<span class="sidenote">Some one actually [did this](https://www.citusdata.com/blog/2017/10/11/index-all-the-things-in-postgres/). For science.</span>. Not so fast! Every index you add adds some overhead to the database. Generally, write operations (`DELETE`, `UPDATE`) become more expensive,
while read operations (`SELECT`) generally just benefit. Too many indexes can exhaust the cache memory so that even read operations can suffer.

Finally, as with all great things, database indexes are a double-edged sword that should be wielded very carefully. There is no way around deeply
understanding your own database schema and how it's going to be used by the application to do this well.


## Further reading
 1. [Indexes](https://www.postgresql.org/docs/current/static/indexes.html)
 2. [Efficient Use of PostgreSQL Indexes](https://devcenter.heroku.com/articles/postgresql-indexes)
 3. [Use the index, Luke](https://use-the-index-luke.com/sql/preface)
