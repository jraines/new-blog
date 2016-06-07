---
title: Postgres Tricks
date: 2016-06-07 18:36 UTC
tags: postgres
---

#Postgres Tricks

##Upsert

To insert or update a given record, you need a constraint that will let the query know that the record already exists:

```sql
insert into items (user_id, image_uid)
values (2, 101)
on conflict on constraint unique_image_uid do update
set updated_at = now()
returning *
```

Also note the `returning` part, which tells the query to return all the fields of the record whether it was created or updated.

##Adding to an array without creating duplicates

First, create a function that can do this:

```sql
CREATE OR REPLACE FUNCTION array_append_distinct(anyarray, anyarray)
RETURNS anyarray AS $$
  SELECT ARRAY(SELECT unnest($1) union SELECT unnest($2))
$$ LANGUAGE sql;
```

Then, you can use it like:

```sql
update items set tags = array_append_distinct(tags, ("cool","stuff") where id = 2;
```

If the item was already tagged with "cool", it won't now be "cool, cool, ..."

You may need to cast the second arg to the same type as whatever `tags` is, like `text[]`
