---
title: Thread-first macro for native objects -- doto
date: 2016-06-15 04:39 UTC
tags: chain, clojure
---

From [the docs](https://clojuredocs.org/clojure.core/doto)


```
;; Note that even though println returns nil, doto still returns the HashMap object
user> (doto (java.util.HashMap.)
            (.put "a" 1)
            (.put "b" 2)
            (println))
#<HashMap {b=2, a=1}>
{"b" 2, "a" 1}

;; Equivalent to
user> (def m (java.util.HashMap.))
user> (.put m "a" 1)
user> (.put m "b" 2)
user> m
{"a" 1, "b" 2}
```

