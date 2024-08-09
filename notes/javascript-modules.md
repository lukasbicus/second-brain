---
title: JavaScript modules
summary: JavaScript modules history, fundamentals, examples
slug: javascript-modules
project-tags: 
  - self education
thematic-tags:
  - modules
  - imports
  - exports
  - amd
  - commonjs
---

# JavaScript modules

*JavaScript modules history, fundamentals, examples*

---
## Credits
I take some notes from [JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) and combined with some wiki articles. 


## History
JavaScript programs used to be small - at early days it was isolated scripts providing a bit of interactivity to webpages. Larger scripts was generally not needed.
Many years later - now - we have complete applications being run in browsers / on backends.

In recent years it made sense to start thinking about providing mechanisms for slitting JavaScript programs up to separate modules that can be imported, when needed. Node has had this ability for a long time and there are number of libraries that enable module usage (f.e.: **CommonJS** and **AMD** - based module systems like `RequireJS` and `Webpack`)

### CommonJs

- is a project to standardize module ecosystem for javascript outside of web browsers (e.g. on web servers or in native desktop applications)
- CommonJs specifies, how modules should work
- is widely used with `Node.js`
- browsers don't support CommonJs directly
- browser side javascript can use modules packed by commonJs standard after they are packed with a transpiler
- it can be recognized by the use of `require()` function and `module.exports`

### Asynchronous Module Definition (AMD)

- is a specification for JavaScript
- AMD specifies a mechanism for defining modules such that the module and it's dependencies can be asynchronously loaded
- this is well suited for browser environments where synchronous loading of modules incurs issues
- the `RequireJS` library implements AMD

### Module loaders, bundlers

- **RequireJS** - is a javascript file and **module loader** optimized for browsers
- **Webpack**
  - is a static **module bundler** for javascript applications
  - it combine modules needed for application into one or more static assets (bundles)
  - with help of loaders, it can transform front-end assets such as HTML, CSS and images into bundles
- **Parcel**
- **Rollup**
- ...


### Babel
- is a JavaScript trans-compiler
- it translates ECMAScript 2015+ code into backwards compatible Javascript
- can compile TypeScript to JavaScript
- can transform non standard JavaScript syntax like JSX
- usually it's used in conjunction with bundlers like **Webpack**, **Parcel** and **Rollup**


## ECMAScript (ES modules)
The good news is, that modern browsers have started to support module functionality natively.

Use of native JavaScript modules is dependent on the `import` and `export` statements, available for both browser and backend development.

The basic usage of `import`s, `export`s and modules is demonstrated in [module examples](https://github.com/mdn/js-examples/tree/main/module-examples)

### A note on .mjs vs .js

`.mjs` stands for module javascript. [V8 documentation](https://v8.dev/features/modules#mjs) recommends using `.mjs`.

The file extension is not a deciding factor.
The browser recognizes a file as a file with javascript code according it's mime type `text/javascript`.
The browser recognizes a javascript file as a module by setting type attribute to `module`

```html
<script type="module" src="./a-script.js" />
```

Using `.mjs` extension is good for clarity. It ensures files are parsed a module by runtimes such as a `Node.js`.

However not all servers serve `.mjs` files correctly, so `.js` files are widely used.

### Export / import example

Example of a `square.js` module:

```javascript 
// square.js
export const name = "square";

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, length, length);

  return { length, x, y, color };
}
```

`square.js` is used by `main.mjs`
```javascript
import {create, draw} from './square.js'
const myCanvas = create("myCanvas", document.body, 480, 320);

const square = draw(myCanvas.ctx, 50, 50, 100, "blue");
```

The javascript import in browser should be absolute/relative path to a file with extension.
```javascript
// in browser
import {create, draw} from './square.js'
```

The javascript import in `nodeJs` can look like bellow when there is a package in npm modules called `square` with `index.js` file
```javascript
// in node
import {create, draw} from 'square'
```

Ecosystems like `NodeJs` use package managers such as `npm` to manage modules and their dependencies.

### Other differences between modules and standard scripts

Check all differences [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#other_differences_between_modules_and_standard_scripts).
Major differences:
- modules are executed only once (even they can be referenced multiple times)
- module features are imported into scope of a single script. They won't be available in global scope

### Types of exports and imports

Exports examples:
```javascript
//square.js

// named export
export function randomSquare() {
  // do stuff
}

// default export
export default randomSquare;
```

Imports examples:
```javascript
// example of named import
import {randomSquare} from './square.js'

// example of default import
import defaultSquare from './square.js'

// example of namespace import
import * as square from './square.js'

// example of side effect import - this is often used for polyfills, which mutate the global variables.
import './square.js'
```

## Practical implications

It's easy to add and use any library to your frontend project from https://esm.sh/. Example:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>React App</title>
    <link rel="stylesheet" href="./style.css">
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./main.tsx"></script>
  </body>
</html>
```

```tsx
import React, { type MouseEvent } from "https://esm.sh/react@18.2.0"
import confetti from "https://esm.sh/canvas-confetti@1.6.0"

const App = () => {
  function onMouseMove(e: MouseEvent) {
    confetti({
      particleCount: 5,
      origin: {
        x: e.pageX / window.innerWidth,
        y: (e.pageY + 20) / window.innerHeight,
      }
    })
  }

  return (
    <div onMouseMove={onMouseMove}>
      <h1>Hello React! ⚛️</h1>
      <p>Building user interfaces.</p>
    </div>
  )
}

export default App
```



 



