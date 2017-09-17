---
title: "Postgres Partial Indexes"
date: 2017-09-17T09:48:47-06:00
---

I have a large Postgres database that backs some data services used by a
few other apps. This database is not large by Postgres standards, having
about 600M rows.

I did some reading on Postgres indexes and decided that a series of partial
indexes would likely provide the best performance for our variety of queries.
However, I was confused that while my analysis revealed that the indexes I
created would serve those queries well, Postgres never seemed to use them.

I'm not a database expert and certainly not a Postgres expert, so I had done
(what I thought was) quite a bit of reading on the subject, but without a
satisfactory explaination.

This is when I [found something I had
overlooked](https://www.postgresql.org/docs/9.6/static/indexes-partial.html)
in the Postgres documentation:

> As a result, parameterized query clauses do not work with a partial index.

That explained everything I was seeing.

Maybe this will help someone.
