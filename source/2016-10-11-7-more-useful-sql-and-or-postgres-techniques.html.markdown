---
title: 7 More Useful SQL and/or Postgres Techniques
date: 2016-10-11 01:28 UTC
tags: postgres
---

# 7 More Useful SQL and/or Postgres Techniques

## 1) Returning structured data from relations with `json_agg`

Let's say we have an outfit and we want to return its id and a json string with data
from all of that outfit's associated items.

The following shows an example where you only want a subset of the items' columns:

```sql
select outfits.id,
       (select json_agg((select item_stub
                                      from (select items.name,
                                                   items.buy_url,
                                                   items.brand,
                                                   items.category,
                                                   items.image_url,
                                                   items.price) item_stub))
        from outfit_items
        join items on items.id = outfit_items.item_id
        where outfit_items.outfit_id = outfits.id
      ) as items
from outfits;
```


## 2) Querying an array field's length.

Assuming `tags` is a flat, one dimensional array, how many outfits have any tags?

```sql
select count(outfits)
where array_length(tags, 1) is null
```


## 3) Counting conditionally with `case`

```sql
select sum(case when mobile = true then 1 else 0 end) as mobile_count
from impressions;
```

## 4) Set-like array append (no duplicates)

Create an `array_append_distinct` function with

```sql
CREATE OR REPLACE FUNCTION array_append_distinct(anyarray, anyarray)
RETURNS anyarray AS $$
  SELECT ARRAY(SELECT unnest($1) union SELECT unnest($2))
$$ LANGUAGE sql;
```

Then use it like this:

```sql
update items set tags = array_append_distinct(tags, CAST(ARRAY['foo','bar'] as text[]))
```

## 5) Removing duplicate associations from a join table

```sql
delete from outfit_items where id in (
  select o1.id
  from outfit_items o1
  join outfit_items o2
    on o1.outfit_id = o2.outfit_id
    and o2.item_id = o1.item_id
  where o1.id != o2.id);
```


## 6) Get column names for a table

```sql
select column_name
from information_schema.columns
where table_name='items'
```

## 7) Get list of all databases and their users

In psql:  `\l`

