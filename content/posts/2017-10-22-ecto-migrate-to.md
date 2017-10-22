---
title: "Ecto Migrate To"
date: 2017-10-22T15:00:00-06:00
---

[Ecto](https://hexdocs.pm/ecto/Ecto.html) has a [nice
feature](https://hexdocs.pm/ecto/Mix.Tasks.Ecto.Migrate.html#content)
that allows a target migration to be specified.

The example is:

```
mix ecto.migrate --to 20080906120000
```

The ID above is the prefix of the migration file name.

I used this when I had a set of migrations to run, but needed to do
intermediate work as each one was applied. This made it really easy.
