---
title: "4Clojure: rotating a sequence"
date: 2014-12-14 18:18 UTC
tags: clojure
---

#Rotating a sequence

The problem:

> Write a function which can rotate a sequence in either direction.

Such that:

```clojure
  (= (your-fn 2 [1 2 3 4 5]) '(3 4 5 1 2))
```

and

```clojure
  (= (your-fn 2 [1 2 3 4 5]) '(3 4 5 1 2))
```

##My solution

A recursive loop where the function that takes the loop closer to completion and the function that recurses on the collection
differ depending on we're rotating "backwards" or "forwards".

It is ugly in the case of negative values because the `into` is O(n), and that's called n times.

```clojure
(fn [n v]
  (if (= n 0)
    v
    (let [f (if (pos? n) dec inc)
          c (if (pos? n)
              (conj (vec (rest v)) (first v))
              (into [(last v)] (take (- (count v) 1) v)))]
      (recur (f n) c))))
```

##Two better solutions and why I missed them

###chouser's solution:

```clojure
  #(let [c (count %2)] (take c (drop (mod % c) (cycle %2))))
```

I was missing two key insights that are required for this more elegant
solution. One is "thinking lazy" to create an infinite seq of the passed-in
sequence repeated over and over.  Once you have you just need to put your
`(count n)` long "window" in the right place.

This is where `mod` comes in.  `(mod % c)` gives you the number of items to
skip before placing the front of the "window".  This skipping and placement of
the "window" is accomplished by the paired `take` and `drop`.  It would not
have occurred to me to `drop` from an infinite sequence, because I neglected to
"think lazy" and understand that that concept isn't realized until the `take`
is attempted.

###quant1's solution:

```clojure
(fn [n x] (let [idx (mod n (count x))]
                  (concat (drop idx x) (take idx x))))
```

I started down this path, but because the `mod` insight did not occur to me, I
gave up after being unable to `take` or `drop` negative numbers.  I messed with
`subvec` for a few mins before implementing my solution.
