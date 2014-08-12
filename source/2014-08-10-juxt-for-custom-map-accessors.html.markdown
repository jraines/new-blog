---
title: Juxt for custom map accessors
date: 2014-08-10
tags: clojure, chain
---

#Juxt for custom map accessors

Let's say we have a map like this:

```clojure
(def ppl [{:name "Jeremy"  :age 31 :job "programmer"}
          {:name "Grace"   :age 24 :job "designer"}])
```

If I wanted to grab just the name and age from it, I could write:

```clojure
(map #(vector (get % :name) (get % :age)) ppl)
```

But with `juxt` I can do:

```clojure
(map #(juxt :name :age) ppl)
```

Hat tip to [Tim Baldridge for this example](https://tbaldridge.pivotshare.com/media/function-of-the-day-juxt/11920)


