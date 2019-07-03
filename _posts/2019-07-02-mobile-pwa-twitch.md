---
layout: post
title: A mobile web app implementation of Twitch
categories:
  - development
published: true
tags:
  - draft
---

With all of the new capabilities for the web being promoted by Google at [I/O 2019](https://youtu.be/GSiUzuB-PoI), I thought it would be worthwhile to see if it is possible to increase development velocity by targeting a PWA design instead of a native app.

The goal for this experiment was to build a mobile proof-of-concept for something like [Twitch](https://www.twitch.tv/).

### Hypothesis:

> *It is possible to multiplex generated graphics with an audio stream, record the resulting object, and finally share it as a file to other native apps.*

## Graphics

With [WebGL 2](https://www.khronos.org/registry/webgl/specs/latest/2.0/), creating full screen imagery is markedly more performant than the DOM manipulation hackery of the past, and better lines up with existing graphics concepts.

Dive directly into [WebGL 2 fundamentals](https://webgl2fundamentals.org/), or use 2D or 3D wrappers like [pixi.js](https://pixijs.io/) or [three.js](https://threejs.org/) depending on the game requirements. For this PoC I was aiming for some simple video billboards with text overlays so I opted for pixi.js.

All of these options render to a [\<canvas\>](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) element for insertion into the DOM for display.

## Mix in an audio stream

The Web Audio API can be used to construct a node-based filter graph that can terminate into the canvas element or MediaStream Recording API.

```javascript
// bind source to default destination (speakers)
sourceNode.connect(audioCtx.destination);

// bind source to canvas destination
let canvasDest = audioCtx.createMediaStreamDestination();
sourceNode.connect(canvasDest);
```

## Record everything

The [MediaStream Recording API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream_Recording_API) provides a mechanism to capture the canvas contents to a media file.

It is possible to test the API for codec capabilities, but there isn't a clear way to interrogate it for a list of format support.

```javascript
// connect to canvas of Pixi instance
const canvasStream = pixiApp.view.captureStream();

// attach the WebAudio track
canvasStream.addTrack(audioTrack);
```

## Share to other (native) apps

The final piece of magics comes with Web Share API Level 2, which now allows the sharing of files in addition to text. This is very recently supported in Chrome for Android 75 and newer.

*Note that in the very short term, Chrome for Android 75 always tries to share multiple files even if only one file is provided. This is already fixed in Canary, and hopefully won't take long to make it to Stable.*

Convert the Blob(s) into a File object, and then pass them to the navigator.share() method.

The Web Share API requires a [secure context](https://heycam.github.io/webidl/#dfn-available-only-in-secure-contexts), and will 

**silently** fail without one. Like the Web Audio API, it also must be [triggered by a user activation](https://html.spec.whatwg.org/multipage/interaction.html#activation).

```javascript
function shareRecording() {
  const webShareAvailable = {
    links: 'share' in navigator,
    files: 'canShare' in navigator,
  };

  if (webShareAvailable.files) {
    const filesArray = [shareFile];
    const shareData = { files: filesArray };
    if (navigator.canShare(shareData)) {
      shareData.title = 'Titles Go Here';
      shareData.text = 'And a bunch of text too!';
      navigator.share(shareData)
        .then(() => console.log('Share was successful.'))
        .catch((error) => console.log('Sharing failed', error.name));
    } else {
      // File sharing not supported
    }
  }
}
```

## Put it all together

See https://lexicast.glitch.me for a working* demo.

## Experimental results

* Recording a 1080x1080 pixel canvas is noticeably choppy on a Pixel 2 running Android 9. Using VP8 instead of VP9 seems a little better.
* Most popular video-based share targets (e.g., Instagram, Twitter, TikTok) don't currently support the codecs that the MediaStream Recorder API is capable of generating.

## Next steps

It might be possible to work around the codec incompatibility issues in the PWA space using WebAssembly.

In the meantime, it seems more likely that a native app will be better suited to this use case.
