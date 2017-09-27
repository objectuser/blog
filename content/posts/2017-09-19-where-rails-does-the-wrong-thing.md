---
title: "Where Rails Does the Wrong Thing"
date: 2017-09-19T10:30:00-06:00
---

I don't have a lot of experience in Rails. I've used it over the last
year, but most of my time during that period has been in Elixir.

Recently, due to how Postgres partial indexes work, I wanted to replace
some parameter markers in my queries with manifest (i.e., hard coded)
values. This was only for things like processing states and the like. I
still wanted values like keys and dates to be parameterized for DB
efficiency.

A colleague pointed out that doing queries like this doesn't actually
send parameter markers to the DB:

```
MyModel.where("state = ?", "in progress")
```

Instead, Rails replaces the value in memory and the DB gets a statement
like:

```
select * from my_model where state = 'in progress'
```

If, instead, one writes:

```
MyModel.where(state: "in progress")
```

Rails will send the database a parameterized statement along with values
to be bound to the statement:

```
select * from my_model where state = ?
```

I don't know why Rails handles the two forms differently. But in most
cases, it's much better to have the second form because the DB only has
to compile that statement once for all bound values.

There is a particular problem though: the second form does not support
inequalities. So if I have a query like this:

```
MyModel.where("created_at < ?", 1.day.ago)
```

Rails will always send the database something like:


```
select * from my_model where created_at < '2017-09-09 10:30'
```

That's problematic because every time the query is submitted, the
database will see it as unique and have to go through the compilation
process again. That's a performance killer akin to CGI.

By the way, I verified this behavior by looking at the statements that
Postgres actually receives, not what Rails prints in the logs.

My conclusion is to almost always use the `where(key: value)` form of
statements and to know that inequalities will have an added performance
impact.
