# Magic Helpers

A collection of magic properties and helper functions for use with Alpine.

![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/alpine-collective/alpine-magic-helpers?label=version&style=flat-square)

## About

Adds the following magic helpers to use with Alpine JS. ***More to come!***
| Magic Helpers | Description |
| --- | --- |
| [`$component/$parent`](#component) | Natively access and update data from other components or the parent component. |
| [`$fetch`](#fetch) | Using Axios, fetch JSON from an external source.  |
| [`$interval`](#interval) | Run a function every n milliseconds. Optionally start and stop the timer. |
| [`$range`](#range) | Iterate over a range of values. |
| [`$screen`](#screen) | Detect if the current browser width is equal or greater than a given breakpoint. |
| [`$scroll`](#scroll) | Scroll the page vertically to a specific position. |
| [`$truncate`](#truncate) |  Limit a text string to a specific number of characters or words. |

🚀 If you have ideas for more magic helpers, please open a [discussion](https://github.com/alpine-collective/alpine-magic-helpers/discussions) or join us on the [AlpineJS Discord](https://discord.gg/snmCYk3)

## Installation

Include the following `<script>` tag in the `<head>` of your document (before Alpine):

```html
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/index.min.js"></script>
```

Or only use the specific methods you need:

```html
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/component.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/fetch.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/interval.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/range.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/screen.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/scroll.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alpine-collective/alpine-magic-helpers@0.5.x/dist/truncate.min.js"></script>
```

---

### Manual

If you wish to create your own bundle:

```bash
npm install alpine-magic-helpers --save
```

Then add the following to your script:

```javascript
import 'alpine-magic-helpers'
import 'alpinejs'
```

### `$component`
**Example:**

Arguably more useful, this also adds a `$parent` magic helper to access parent data
```html
<div x-data="{ color: 'blue' }">
    <p x-data x-text="$parent.color"></p>
    <!-- The text will say blue -->
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/XWdjWrr)

You may watch other components, but you must give them each an id using the 'id' attribute or `x-id` if you need more flexibility:
```html
<div x-data="{ color: 'blue' }">
    <p
        x-data
        x-text="$component('yellowSquare').color"
        :class="`text-${$parent.color}-700`">
        <!-- This text will have blue background color and the text will say yellow -->
    </p>
</div>

<div x-id="yellowSquare" x-data="{ color: 'yellow' }"></div>
```


### `$fetch`
**Example:**
```html
<div x-data="{ url: 'https://jsonplaceholder.typicode.com/todos/1' }"
    x-init="$fetch(url).then(data => console.log(data))">
    <!-- After init, data will be logged to the console -->
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/poyyXKj)

**Optionally pass in an options object**

By default, `$fetch` will return the JSON data object. However, because we are using Axios behind the scenes, you may pass in an object to customize the request [See all options](https://github.com/axios/axios).

**Example:**

```html
<div x-data="{ url: 'https://jsonplaceholder.typicode.com/todos/1' }"
    x-init="$fetch({ url: url, method: 'post' }).then(({ data }) => console.log(data))">
</div>
```
> Note that this will return the entire response object, whereas by default `$fetch` will only return the data

---

### `$interval`
**Example:**
```html
<div
    x-data="{
        timer: 500,
        funtionToRun: function() {
            console.log('Hello console')
        }
    }"
    x-init="$interval(funtionToRun, timer)">
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/xxVVoaX?editors=1010)

**Optionally pass in options**

By default, `$interval ` will run your function every `nth` millisecond when browser provides an animation frame (via `requestAnimationFrame`). This means that the function will not run if the browser tab is not visible. Optionally, you may pass in the following options as the second parameter:
| Property | Description |
| --- | --- |
| `timer` | Timer in milliseconds.  |
| `delay` | Delay the first run. N.B. The first run is also delayed by the timer time. |
| `forceInterval` |  Ignore the browser animation request mechanism. Default is false |

> ⚠️ We also add a hidden property `autoIntervalTest` that will play/pause the timer depending on it's "truthiness"

**Example:**

```html
<div
    x-data="{
        timer: 500,
        autoIntervalTest: true, // optional to start/stop the timer
        funtionToRun: function() {
            console.log('Hi again!')
        }
    }"
    x-init="$interval(funtionToRun, { timer: 1000, delay: 5000, forceInterval: true })"
    @click="autoIntervalTest = !autoIntervalTest">
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/poyyXQy?editors=1010)

---

### `$range`
**Example:**

The `$range` helper mostly mimics implementations found in other languages `$range(start, stop, step = 1)`
```html
<div x-data>
    <template x-for="item in $range(1, 5)">
        ...
    </template>
</div>
<!-- This will output 5 iterations [1, 2, 3, 4, 5], modelled after PHP's implimentation of range() -->
```
[Demo](https://codepen.io/KevinBatdorf/pen/vYKbPBd)

> N.B: You may use `$range(10)` which will compute to `[1...10]`

---

### `$screen`
**Example:**

The `$screen` helper detects if the current browser width is equal or greater than a given breakpoint and returns `true` or `false` based on the result.

```html
<div x-data>
    <span x-show="$screen('lg')">This will be visible if the window width is equal or greater than 1024px.</span>
</div>
```

*By default the `$screen` helper uses the following endpoint borrowed by **Tailwind CSS**:*
- `xs`: 0px
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

> ⚠️ **NOTE**: A single breakpoint is only going to tell you if the browser width is equal or greater than the given breakpoint. If you want to restrict the check to a specific range, you will need to negate the next endpoint as:

```html
<div x-data>
    <span x-show="$screen('md') && !$screen('lg')">This will be visible if screen width is equal or greater than 768px but smaller then 1024px.</span>
</div>
```

**Custom breakpoints**

You can pass a numeric value to use an ad-hoc breakpoint.
```html
<div x-data>
    <span x-show="$screen(999)">This will be visible if screen width is equal or greater than 999px.</span>
</div>
```

You can also override the default breakpoints including the following `<script>` tag in the `<head>` of your document

```html
<!-- this example uses Bulma's breakpoints. -->
<script>
    window.AlpineMagicHelpersConfig = {
        breakpoints: {
            mobile: 0,
            tablet: 769,
            desktop: 1024,
            widescreen: 1216,
            fullhd: 1408
        }
    }
</script>
```
And using those breakpoints in your page.
```html
<div x-data>
    <span x-show="$screen('tablet')">This will be visible if screen width is equal or greater than 769px.</span>
</div>
```

[Demo](https://codepen.io/KevinBatdorf/pen/OJXKRXE?editors=1000)

---

### `$scroll`
**Example:**
```html
<div x-data>
    <div x-ref="foo">
        ...
    </div>
    <button x-on:click="$scroll($refs.foo)">Scroll to foo</scroll>
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/PozVLPy?editors=1000)

Alternatively, you can pass a css selector to scroll to an element at any position.
```html
<div id="foo">
</div>
<div x-data>
    <button x-on:click="$scroll('#foo')">Scroll to #foo</scroll>
</div>
```

`$scroll` also supports integers to scroll to a specific point of the page.
```html
<button x-data x-on:click="$scroll(0)">Scroll to top</scroll>
```
[Demo](https://codepen.io/KevinBatdorf/pen/PozVLPy?editors=1000) (same as above)

`$scroll` optionally supports a second parameter where it's possible to define the behavior mode, `auto|smooth` (default smooth):
```html
<div x-data>
    <div x-ref="foo">
        ...
    </div>
    <button x-on:click="$scroll($refs.foo, {behavior: auto})">Jump to foo</scroll>
</div>
...
<div id="foo">
</div>
<div x-data>
    <button x-on:click="$scroll('#foo, {behavior: auto}')">Jump to #foo</scroll>
</div>
...
<button x-data x-on:click="$scroll(0, {behavior: auto}">Jump to top</scroll>
```
With offset:
```html
<div x-data>
    <div x-ref="foo">
        ...
    </div>
    <button x-on:click="$scroll($refs.foo, {offset: 50})">Scroll to 50px before foo</scroll>
</div>
...
<div id="foo">
</div>
<div x-data>
    <button x-on:click="$scroll('#foo, {offset: 50}')">Scroll to 50px before #foo</scroll>
</div>
...
<button x-data x-on:click="$scroll(0, {offset: 50}">Jump to 50px before top (a bit daft but supported)</scroll>
```
With both:
```html
<div x-data>
    <div x-ref="foo">
        ...
    </div>
    <button x-on:click="$scroll($refs.foo, {behavior: auto, offset: 50})">Jump to 50px before foo</scroll>
</div>
...
<div id="foo">
</div>
<div x-data>
    <button x-on:click="$scroll('#foo, {behavior: auto, offset: 50}')">Jump to 50px before #foo</scroll>
</div>
...
<button x-data x-on:click="$scroll(0, {behavior: auto, offset: 50}">Jump to 50px before top</scroll>
```
[Demo](https://codepen.io/KevinBatdorf/pen/PozVLPy?editors=1000) (same as above)

---

### `$truncate`
**Example:**
```html
<div
    x-data="{ characters: 50, string: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'}"
    x-text="$truncate(string, characters)"
    @click="characters = undefined">
    <!-- Text will show 'Lorem ipsum dolor sit amet, consectetur adipiscing…' and will reveal all when clicked-->
</div>
```
You may also pass a third argument to change the string that will be appended to the end:
```html
<div
    x-data="{ characters: 50, string: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'}"
    x-text="$truncate(string, characters, ' (...)')">
    <!-- Text will show 'Lorem ipsum dolor sit amet, consectetur adipiscing (...)' -->
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/BaKKgGg?editors=1000)

**Optionally pass in options**

By default, `$truncate` will return take characters as a parameter. Instead you can pass in an object and trim by words. You may also update the ellipsis.

**Example:**

```html
<div
    x-data="{ count: 5, string: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'}"
    x-text="$truncate(string, { words: words, ellipsis: ' ...read more' })"
    @click="count = 0">
    <!-- Will start with 5 words, then increase to unlimited when clicked -->
</div>
```
[Demo](https://codepen.io/KevinBatdorf/pen/BaKKgGg?editors=1000) (same as above)
> Behind the scenes, for words, this uses `sentence.split(" ").splice(0, words).join(" ")` which does not define a word in all languages.

---

## License

Copyright (c) 2020 Alpine Collective

Licensed under the MIT license, see [LICENSE.md](LICENSE.md) for details.
