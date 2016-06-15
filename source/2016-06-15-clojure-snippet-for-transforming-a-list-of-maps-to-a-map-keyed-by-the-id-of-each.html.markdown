---
title: Clojure snippet for transforming a list of maps to a map keyed by the :id of each
date: 2016-06-15 04:54 UTC
tags: chain, clojure
---

##Transforming a list of maps to a map keyed by the :id of each

```clojure
(into {} (map (juxt :id identity) coll))
```

