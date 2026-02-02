# CSS Grid Lanes Polyfill

A polyfill for the new `display: grid-lanes` CSS feature, enabling support in browsers that do not yet implement it natively.

This implementation is based on the WebKit proposal described here:
https://webkit.org/blog/17660/introducing-css-grid-lanes/

Originally written by Simon Willison, this edition features numerous enhancements to make the original work properly.

## Supported Features

- Inline stylesheets, `script` element, imported stylesheets
- `display: grid-lanes`
- `grid-template-columns` / `grid-template-rows` for lane definition
- `gap`, `column-gap`, `row-gap`
- `item-tolerance` for placement sensitivity
- Spanning items (`grid-column: span N`)
- Explicit placement (`grid-column: N / M`)
- Responsive `auto-fill` / `auto-fit` with `minmax()`
- Both waterfall (columns) and brick (rows) layouts
- CSS math functions: `min()`, `max()`, `clamp()`, `calc()` (supports nested functions and arithmetic operations)

## Limitations

The following features are **not** supported:

- `fr` units with `grid-template-rows` in "brick" (horizontal) layouts (fr units require a known container size, but brick layout containers typically have auto height. Use fixed row heights like 100px instead.)

## Usage

### 1. Load the polyfill/ponyfill

There are multiple ways to load the grid lanes functionality:

#### As a traditional polyfill (loads globally)

```html
<script src="grid-lanes-polyfill.js"></script>
// OR
<script src="https://cdn.jsdelivr.net/gh/ninjamar/grid-lanes-polyfill@1.1.0/grid-lanes-polyfill.js"></script>
```

#### As a ponyfill (importable module)

```js
// ES Modules
import { GridLanesPolyfill } from "./grid-lanes-polyfill.js";

// Or with a bundler that supports packages
import { GridLanesPolyfill } from "grid-lanes-polyfill";
```

### 2. Initialize the polyfill

Run the polyfill after the DOM has loaded, and only when native support is missing:

```js
document.addEventListener("DOMContentLoaded", () => {
  if (!GridLanesPolyfill.supportsGridLanes()) {
    GridLanesPolyfill.init({ force: true });
  }
});
```

### 3. Add the required custom property in CSS

For every element using `display: grid-lanes`, you **must** include the following custom property:

```css
.my-grid {
  --grid-lanes-polyfill: 1;
  display: grid-lanes;
}
```

This is required because browsers strip unknown properties and values (including `display: grid-lanes`) during CSS parsing. The polyfill uses this custom property as a hook to detect and process affected elements.

> [!NOTE]
> The script parses CSS in many ways. Inline styles and root stylesheets may be parsed "as is", meaning that this custom property technically is not always needed. However, this is subject to change, so it is important to include it.

## Version

**1.2.2**

## Authors

- Simon Willison
- ninjamar
- Kaleidosium

## License

> MIT

Other files in this repository may be distributed under different licenses. Please check the license header or accompanying license file in each individual file for its specific terms.
