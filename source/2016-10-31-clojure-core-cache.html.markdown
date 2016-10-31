---
title: clojure.core.cache
date: 2016-10-31 23:37 UTC
tags: clojure
---

#clojure.core.cache

[clojure.core.cache](https://github.com/clojure/core.cache/) provides a functional in-memory cache. It provides different built-in cache types, like TTL, LRU, or others. Like other clojure data structures, it is immutable.  I've found the following pattern useful for reckoning with that immutability.

In this example, we store weather forecasts provided by an external API for a given zip code and cache the results for that zip code for half a day.


```clojure
(ns foo.weather
  (:require [org.httpkit.client :as http]
            [foo.zips :as zips]
            [clojure.core.cache :as cache]
            [clojure.data.json :as json]))

(def endpoint "https://api.darksky.net/forecast/an-api-key/")

(def half-day
  (* 1000 ;milliseconds -> seconds
     60   ;seconds -> minutes
     60   ;mins -> hours
     12))

(def forecasts-cache (atom (cache/ttl-cache-factory {} :ttl half-day)))

(defn parse-response
  [url]
  (let [resp @(http/get url)
        forecast (json/read-str (:body resp) :key-fn keyword)
        daily (get-in forecast [:daily :data])
        today (first daily)]
    today))

;fn which does the work on a cache miss
(defn forecast*
  [zip]
  (let [latlong (get zips/zips zip)
        url (str endpoint (clojure.string/join "," latlong))]
    (parse-response url)))

;fn that tries to return cached values
(defn forecast
  [zip]
  (if (cache/has? @forecasts-cache zip)
    (get (cache/hit @forecasts-cache zip) zip)
    (let [updated-cache (swap! forecasts-cache #(cache/miss % zip (forecast* zip)))]
      (get updated-cache zip))))
```
