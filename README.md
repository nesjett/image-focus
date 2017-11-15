# image-focus
A dependency free utility for cropping images based on a focus point ~2.6kB gzipped

[![NPM](https://nodei.co/npm/image-focus.png)](https://nodei.co/npm/image-focus/)

[![npm downloads](https://img.shields.io/npm/dm/image-focus.svg)](http://npm-stat.com/charts.html?package=image-focus)

**NOTE: This project is not ready for release yet. API's are subject to change**

## Demo

[Check out the demo](https://image-focus.stackblitz.com) and then [play with editing it](https://stackblitz.com/edit/image-focus)

## Docs

[Docs generated by `typedoc`](https://third774.github.io/image-focus/)

## Usage

`Focus` Coordinates range between `-1` and `1` for both `x` and `y` axes. The [`FocusPicker`](#focuspicker) class will help enable users to select the focus point to be used with an image.

### FocusedImage

There are two ways to supply the coordinates when initializing the `FocusedImage` class

#### Data Tags

```html
<div class="focused-image-container">
  <img class="focused-image" src="https://picsum.photos/2400/1400" data-focus-x="0.34" data-focus-y="-0.21">
</div>
```

```ts
import { FocusedImage } from "image-focus"

const img = document.querySelector('.focused-image') as HTMLImageElement
const focusedImage = new FocusedImage(img)
```

#### Using `focus` Option

```html
<div class="focused-image-container">
  <img class="focused-image" src="https://picsum.photos/2400/1400">
</div>
```

```ts
import { FocusedImage } from "image-focus"

const img = document.querySelector('.focused-image') as HTMLImageElement
const focusedImage = new FocusedImage(img, {
  focus: {
    x: 0.34,
    y: -0.21
  }
})
```

### FocusPicker

Provide an `onChange` callback that will receive a `Focus` object that has `x` and `y` properties for the newly selected coordinates. Optionally supply a `focus` to initialize with, or a `retina` src to use instead of the default white ring SVG.

```ts
import {FocusedImage, FocusPicker} from "image-focus"

const imgEl = document.getElementById("focused-image") as HTMLImageElement
const focusedImage = new FocusedImage(imgEl)

const focusPickerEl = document.getElementById("focus-picker-img") as HTMLImageElement
const focusPicker = new FocusPicker(focusPickerEl, {
  onChange: focus => focusedImage.setFocus(focus),
  focus: startingFocus,
})
```

## What's going on?

The `<img/>` element is being set to `position: absolute;` and having its `top` and `left` properties adjusted based on some calculations using the image and parent containers' aspect ratios and dimensions. The `<img/>`'s parent container gets set to `position: relative;` and `overflow: hidden;` to create the effect. There are a few other inline styles that get applied, so if anything appears to be behaving unexpectedly, be sure to check that the inline styles on both the `<img/>` and its parent aren't being overridden by CSS on your page (especially from rules using `!important`).

Additionally, because the `FocusedImage` is positioned absolutely so it can shift as needed, its container needs to manage its own height and width. If you aren't seeing an image appear at all, it is likely that the parent div's `height` is fully collapsed.

## What if I'm not using npm and a build process?

That's okay! [unpkg](https://unpkg.com/) has you covered. Just add this script tag to your page and the `image-focus` module is exposed in the global namespace under `window.imageFocus`.

```html
<script src="https://unpkg.com/image-focus"></script>
```

Then in some script that loads after the above script tag:

```js
var imgEl = document.querySelector('img.focused-image');
var focusedImage = new imageFocus.FocusedImage(imgEl, {x: 0.25, y: -0.3});
```

## Attributions

This project was largely inspired by and adapted from [jquery-focuspoint](https://github.com/jonom/jquery-focuspoint) by [jonom](https://github.com/jonom) and used [typescript-library-starter](https://github.com/alexjoverm/typescript-library-starter) to scaffold the build process.
