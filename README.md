> ## 🚨 Status: Experimental
> `<model-viewer>` is currently in the Experimentation phase. Someone on the team thinks it’s an idea worth exploring, but it may not go any further than this. Use at your own risk.


# `<model-viewer>` 

 [![Build Status](https://api.travis-ci.org/GoogleWebComponents/model-viewer.svg?branch=master)](https://travis-ci.org/GoogleWebComponents/model-viewer)
 [![NPM](https://img.shields.io/npm/v/@google/model-viewer.svg)](https://www.npmjs.com/package/@google/model-viewer)

`<model-viewer>` is a web component that makes rendering interactive 3D
models - optionally in AR - easy to do, on as many browsers and devices as possible.
`<model-viewer>` strives to give you great defaults for rendering quality and
performance.

As new standards and APIs become available `<model-viewer>` will be improved
to take advantage of them. If possible, fallbacks and polyfills will be
supported to provide a seamless development experience.

See: [API](#api), [Demo](https://model-viewer.glitch.me),
[Examples](https://googlewebcomponents.github.io/model-viewer/examples/index.html),
[Kanban](https://github.com/GoogleWebComponents/model-viewer/projects/1) (for work prioritization and progress)



## Installing

You can load a _bundled build_ via
[unpkg.com](https://unpkg.com/@google/model-viewer/dist/model-viewer.js):

```html
<script src="https://unpkg.com/@google/model-viewer/dist/model-viewer.js"></script>
```

Alternatively, you can install the _npm package_:

```
npm install ---save @google/model-viewer
```

### Important note on bundling

Bundled builds are useful for demos or for kicking the tires. However,
the _bundled build_ includes some third party dependencies. Some of these
dependencies (like [three](https://threejs.org/)) are quite large. For
production use cases it is we recommend that you use the _npm package_ and your
own bundler (such as [Rollup](http://rollupjs.org) or
[Webpack](https://webpack.js.org/)) to eliminate potential duplicate
dependencies.

## Usage

If you are using a _bundled build_, first add a script tag to your page to load it

```html
<script src="path/to/bundled/model-viewer.js"></script>
```

Alternatively, if you are using the _npm package_ and a bundler (see
"Important note on bundling" above), you can import the module:

```javascript
import '@google/model-viewer';
```

After the library has been loaded, a new custom element will be defined. You can
use it anywhere you would write HTML. For example, using the _bundled build_ in
an HTML document might look like this:

```html
<!doctype html>
<html>
  <head>
    <title>3D Test</title>
    <script src="path/to/bundled/model-viewer.js"></script>
  </head>
  <body>
    <model-viewer src="path/to/model.gltf"></model-viewer>
  </body>
</html>
```

Alternatively, using the _npm package_ in a JavaScript module might look like
this:

```javascript
import '@google/model-viewer';

const model = document.createElement('model-viewer');
model.src = 'path/to/model.gltf';

document.body.appendChild(model);
```

You can think of `<model-viewer>` sort of like an `<img>` or `<video>` tag, but for
3D content. Just set its `src` attribute to the URL of a valid [glTF][glTF] (or
[GLB][GLB]) file and voila!

## Browser Support

`<model-viewer>` builds upon standard web platform APIs so that the performance,
capabilities and compatibility of the library get better as the web evolves.

However, not all browsers support all of these features today. Below is the
latest state of browser support for the relevant emerging features.

**📢 Check out
[POLYFILLS.md](https://github.com/PolymerLabs/model-viewer/blob/master/POLYFILLS.md) to
learn how to polyfill for maximum browser compatibility!**

---

 - ✅ No polyfill needed
 - 🚧 Requires a polyfill
 - 🚫 Not available
 - 🎌 Behind a flag, unstable

Feature                   | Chrome | Canary | Safari 12 | Firefox 63 | Firefox 62 | Edge  | IE 11
--------------------------|--------|--------|-----------|------------|------------|-------|------
Resize Observer           |     ✅ |     ✅ |        🚧 |         🚧 |         🚧 |    🚧 |   🚧
Custom Elements           |     ✅ |     ✅ |        ✅ |         🚧 |         🚧 |    🚧 |   🚧
Shadow DOM                |     ✅ |     ✅ |        ✅ |         ✅ |         🚧 |    🚧 |   🚧
Intersection Observer     |     ✅ |     ✅ |        🚧 |         ✅ |         ✅ |    ✅ |   🚧
Fullscreen API            |     🚧 |     ✅ |        🚧 |         🚧 |         🚧 |    🚧 |   🚧
WebXR Device API         |     🚫 |     🎌 |        🚫 |         🚫 |         🚫 |    🚫 |   🚫
WebXR HitTest API        |     🚫 |     🎌 |        🚫 |         🚫 |         🚫 |    🚫 |   🚫

## API

### Styles

Currently no custom CSS variables are supported, but the model viewer's containing box
can be sized via traditional `width` and `height` properties, and positioned with
the typical properties (`display`, `position`, etc.).

### Attributes

Parameters that are required for display:

* *`src`*: The URL to the 3D model. This parameter is required for
  `<model-viewer`> to display. Only [glTF][glTF]/[GLB][GLB] models
  are supported, see [Supported Formats](#supported-formats).

Optional parameters (not required for display):

* *`auto-rotate`*: Enables the auto rotation of the model.
* *`background-color`*: Sets the background color of the scene when viewed inline. Takes any
  valid CSS color string.
* *`background-image`*: Sets the background image of the scene when viewed inline. Takes a
  URL to an [equirectangular projection image](https://en.wikipedia.org/wiki/Equirectangular_projection) that's used for the skybox, as well as applied as an environment map on the model. Currently only supports traditional image formats (png, jpg), and does not yet support HDR (#65). Setting `background-image` supercedes `background-color`.
* *`controls`*: Enables controls via mouse/touch when in flat view.
* *`ios-src`*: The url to a [USDZ][USDZ] model which will be used on
  [supported iOS 12+ devices](https://www.apple.com/ios/augmented-reality/) via 
  [AR Quick Look](https://developer.apple.com/videos/play/wwdc2018/603/) on Safari.
  See [Augmented Reality](#augmented-reality).
* *`magic-leap`*: Enables the ability to view models in AR when viewing content on
  [Magic Leap's Helio](https://magicleaphelio.com/) browser, requires that `src` is
  a GLB model, and requires the inclusion of the [@magicleap/prismatic](https://www.npmjs.com/package/@magicleap/prismatic) library.
* *`poster`*: Displays an image instead of the model.  See [On
  Loading](#on-loading) for more information.
* *`preload`*: When *`poster`* is also enabled, the model will be downloaded
  before user action.
  See [On Loading](#on-loading) for more information.
* *`reveal-when-loaded`*: When *`poster`* and *`preload`* are specified, hide
  the poster and show the model once the model has been loaded.  See [On
  Loading](#on-loading) for more information.
* *`unstable-webxr`*: Enables the ability to view the model in AR via the experimental
  [WebXR Device API], currently [implemented only in Chrome Canary](https://developers.google.com/web/updates/2018/06/ar-for-the-web).
  See [Augmented Reality](#augmented-reality).

All attributes have a corresponding property in camel-case format. For example,
the `background-color` attribute can also be configured using the
`backgroundColor` property.

### Events

* *`'load'`*: Fired when a model is loaded. Can fire multiple times per
  `<model-viewer>` if the `src` attribute is changed.
* *`'preload'`*: When *`preload`* is enabled this event is fired when
  preloading is done.

### Supported Formats

A `<model-viewer>`'s attributes allows developers to specify multiple file types to
work across different platforms. For WebGL and WebXR purposes, both
[glTF][glTF] and [GLB][GLB] are supported out of the box. Additionally,
developers can specify a [USDZ][USDZ] file (using the `ios-src` attribute) that
will be used to launch Quick Look on iOS Safari as an interim solution until
Safari has support for something like the WebXR Device and Hit Test APIs.

### On Loading

Models are often large, so especially on pages with large numbers of them it
may be desirable to load them after user action. Three parameters -
*`poster`*, *`preload`*, and *`reveal-when-loaded`* - control the loading
behavior.

Four configuration options are available:

* By default, the model will load with the page and will be displayed once
  it's loaded.
* With a *`poster`* specified, the model will not load or display until the
  user takes action (for instance, by clicking on the model element).
* With both *`poster`* and *`preload`* set, the model will load with the page, but
  the poster image will be displayed until the user takes action.
* With all of *`poster`*, *`preload`*, and *`reveal-when-loaded`* set, the poster
  will be displayed until the model is loaded, at which time the poster will
  be hidden and the model displayed.

See the [loading examples](https://googlewebcomponents.github.io/model-viewer/examples/loading.html)

### Important note on data usage

iOS Quick Look only supports model files that use the [USDZ][USDZ] format. This
means that iOS users who see a live-rendered model in the browser (loaded as
[glTF][glTF]/[GLB][GLB] will have to download the same model
a _second time_ in [USDZ][USDZ] format when they launch Quick Look.

### Augmented Reality

There are currently multiple options for viewing content in augmented reality.
Different platforms enable slightly different experiences, but generally finds
a real-world surface and allows the user to place the model, to be viewed through
a camera.

The attributes `ios-src`, `magic-leap` and `unstable-webxr` enable AR features
on certain platforms -- read the [API attributes](#attributes) for each to
understand the support and caveats.

When in augmented reality, all current platforms assume that the models unit size
be in meters, such that a 1.5 unit tall model will be 1.5 meters when in AR.

See the [augmented reality examples](https://googlewebcomponents.github.io/model-viewer/examples/augmented-reality.html).

## Development

After you have cloned the repository locally, you should run:

```
npm install
```

This will install dependencies, run a build and run the tests. Build artifacts
are placed in the `lib` and `dist` folders.

The following npm scripts are available:

* `npm run clean` - Deletes all build artifacts
* `npm run build` - Builds the distributable from the `src/` directory.
* `npm run watch` - Watches the `src/` directory, rebuilding when a file changes.
* `npm run serve` - Serves a static server on port `8000` from the project root.
* `npm run dev` - Combination of `npm run watch` and `npm run serve` -- watches the `src/` directory, rebuilding when a file changes and opens a static server on port `8000`.
* `npm test` - Runs tests.

## License

Apache License Version 2.0, Copyright © 2018 Google

[USDZ]: https://graphics.pixar.com/usd/docs/Usdz-File-Format-Specification.html
[glTF]: https://github.com/KhronosGroup/glTF/tree/master/specification/2.0
[GLB]: https://github.com/KhronosGroup/glTF/tree/master/specification/2.0#glb-file-format-specification
[WebXR Device API]: https://github.com/immersive-web/webxr
