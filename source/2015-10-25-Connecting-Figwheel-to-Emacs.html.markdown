---
title: "Connecting Figwheel to Emacs"
date: 2015-10-25 18:18 UTC
tags: clojure, chain
---

- Make sure you have the lein-fighweel plugin
- Make sure you have nrepl port set (mine is 7888)
- `lein repl :connect 7888`
- In Emacs, run `cider-connect`, choose `localhost`, and type the port (7888)
- In that repl, `(use 'figwheel-sidecar.repl-api)` then `(cljs-repl)`
- `cider-jack-in`
- Load page in browser
- `(in-ns your-ns.core)`
