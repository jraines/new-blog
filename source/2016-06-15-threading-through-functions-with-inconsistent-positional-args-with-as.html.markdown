---
title: Threading through functions with inconsistent positional args with as->
date: 2016-06-15 04:31 UTC
tags: chain, clojure
---

##as->

Threading macro which gives an alias to the thing you're passing through a series of functions.

Copied straight from [the docs](https://clojuredocs.org/clojure.core/as-%3E)

```clojure
;; when you want to use arbitrary positioning of your argument in a thread macro
(as-> {:a 1 :b 2} m
  (update m :a + 10)
  (reduce (fn [s [_ v]] (+ s v)) 0 m))

;; when you'd like an if statement in your thread
(as-> {:a 1 :b 2} m
  (update m :a + 10)
  (if update-b
    (update m :b + 10)
    m))
```

