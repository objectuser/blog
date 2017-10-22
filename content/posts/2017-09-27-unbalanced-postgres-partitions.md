---
title: "Unbalanced Postgres Partitions"
date: 2017-09-27T13:00:00-06:00
---

I recently needed to partition a large table in Postgres. The table is
approaching 600M rows. It had a sufficient number of rows that I was not
able to create an index without running out of temporary storage on my
cloud-hosted server.

Most of my reading on Postgres partitioning talked about partitioning by
date. However, few of my queries are of the "recent rows" variety, so
that didn't seem like an optimal approach.

The records are associated with an "account" and so I thought I might
partition by account. But looking at the data, the resulting partitions
would have very different sizes. The records from one account make up
about 20% of the rows. There are a few other large accounts that also
share a significant percentage of the total.

But I couldn't think of a better approach. So I tried it.

I was able to create indexes on the largest partition, so that removed a
blocker for me. Analyzing my queries indicated that they would perform
well. I needed to move forward so I accepted the results.

Now I have very unbalanced partitions. It's not optimal, but it works.
As I have time I plan to come back to the problem to see if something
else makes sense.

My take away is that, despite my preconceptions, having unbalanced
partitions is better than no partitions.
