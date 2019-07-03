---
layout: post
title: An attempt at a mobile web implementation of Twitch in 2019
categories:
  - development
published: true
tags:
  - draft
---

With all of the [new capabilities for the web](https://youtu.be/GSiUzuB-PoI) being promoted by Google at I/O 2019, I thought it would be worthwhile to see if it is possible to increase developer velocity by targeting a PWA design instead of a native app using the other current cross-platform hype, [Flutter](https://flutter.dev).

The goal for this experiment is to build a proof-of-concept for something like Twitch, but for PWA-backed mobile games:

> Is it possible to mux the composited contents of a [\<canvas\>](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) element with an audio stream, record the resulting object, and finally share it as a file to other native apps using the newly minted [Web Share API - Level 2](https://wicg.github.io/web-share/level-2/)?

# 1. Generate some content

Step one involves generating bitmap content in a canvas (aka game graphics). With advances in WebGL and now [WebGL 2](https://www.khronos.org/registry/webgl/specs/latest/2.0/), this can be markedly more performant than the DOM manipulations of the past.

Dive directly into [WebGL 2 fundamentals](https://webgl2fundamentals.org/), or use 2D or 3D wrappers like [pixi.js](https://pixijs.io/) or [three.js](https://threejs.org/) depending on the game look. I was aiming for some simple video billboards with text overlays so I opted for pixi.js.

# 2. Mix in an audio stream

# 3. Record everything

# 4. Share it to other apps
