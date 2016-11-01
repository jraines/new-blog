---
title: FILTER clauses in Postgres 9.4+
date: 2016-10-31 23:30 UTC
tags: chain, postgres
---

#FILTER clauses

In Postgres 9.4, aggregate functions can take FILTER clauses that replace the functionality previously provided by 

```sql
sum(case foo when bar then 1 else 0 end)
```

like so:

```sql
count(1) filter (where foo is bar)`
```

