---
title: core.async & http.kit API call caching function
date: 2016-06-06 17:01 UTC
tags: chain
---

<img src="/images/function.png" style="width:500px">

`cache-fn` takes a function -- which itself must return a channel -- and returns a version of it that will cache results based on its arguments.

It does this by returning a sort of "promise" result so that all consumers can get the value out of the result chan, not just the first one.

`promise-c` returns a channel that always returns the result value.

