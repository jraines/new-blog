---
title: Copying a table from Heroku Postgres to local csv file
date: 2014-12-24 02:57 UTC
tags: chain, postgres, psql
---

```
echo "COPY products TO STDOUT WITH CSV HEADER " | heroku pg:psql -a app-name > products.csv
```

