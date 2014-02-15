---
layout: page
title: "What's the difference between responsive design and adaptive design?"
comments: true
sharing: true
footer: true

---


### Responsive design

Responsive design is where you can drag-resize your browser can you see stuff rearranging and diminishing (on purpose), to get a better experience for your screen size. 

It interactively "responds" to your dynamic resizing, and does so using:

* flexible and fluid grids, using percentages or `em`, not fixed `px` values
* fluid typography using `em` or `rem`
* media queries and the CSS `@media` rule 
* flexible images and videos

### Adaptive design

Same general intent as Adaptive design, but more rigid: change the _entire site_ based on a pre-determined set of supported devices and screen sizes. I.e., based on what you detect, deliver potentially different HTML, CSS, JS, images, etc. Different stylesheets would get conditionally included in the response. So there's some flexibility in what gets included, but that included content is styled in a fixed way.

### Summary & takeaway

* Responsive site responds fluidly to screen size only, it doesn't try to detect devices (i.e., make a hard difference between mobile and PC). It serves the same code to all clients.
* Adaptive site tries to detect, however it can (which might include screen size), whether you're on a certain device type (again, mobile vs PC). It conditionally serves subsets of of the  codebase tailored to specific targets.
* Responsive is much more popular and cooler, but people still do use adaptive.

