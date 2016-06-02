---
title: Getting a CSV of a query too big for Heroku Dataclips
date: 2016-04-15 20:27 UTC
tags: chain
---

1. Copy the query into a .sql file locally.
2. Remove all newlines, because psql's `\copy` command does not accept multiline arguments.
3. Surround the query with parens and the `\copy` command, like so: `\copy (select ...) TO dump.csv CSV DELIMITER ',' HEADERS`
4. Make sure to use `as` aliases for any column names that are the same across tables, because this method will not be as smart as Dataclips about distinguishing the column headers for those.
5. pipe it to psql: `cat jeans.sql | heroku pg:psql dbcolor -a yourapp`


