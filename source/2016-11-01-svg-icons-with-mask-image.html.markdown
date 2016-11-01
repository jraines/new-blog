---
title: SVG icons with mask-image
date: 2016-11-01 01:23 UTC
tags:
---

#CSS tricks

This example features:

- Responsive square with vw unit height/widget
- Looping animation
- Radial gradient background
- SVG icon with `mask-image`


<div id="mapicon-wrapper">
  <div id="mapicon">

  </div>
</div>

```sass
#mapicon-wrapper
  border: 3px green solid
  display: inline-block
  background-image: radial-gradient(farthest-corner at 0.75vw 0.25vw, green 0%, yellow 100%)
  padding: 2vw

#mapicon
  -webkit-mask-image: url(/images/mapicon.svg)
  width: 10vw
  height: 10vw
  mask-image: url(/images/mapicon.svg)
  animation: my-animation 3s
  animation-iteration-count: infinite

@keyframes my-animation
  0%
    background: green
  50%
    background: yellow
  100%
    background: green
```
