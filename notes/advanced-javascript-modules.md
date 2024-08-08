---
title: Advanced javascript modules
summary: Advanced parts of javascript modules like dynamic imports, top level await and isomorphic modules
slug: advanced-javascript-modules
project-tags: 
  - self education
thematic-tags:
  - dynamic imports 
  - top level await
  - isomorphic modules
---

# Advanced javascript modules

*Advanced parts of javascript modules like dynamic imports, top level await and isomorphic modules*

---
## Credits

Those are final notes from [JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) article 

## Dynamic module loading

- allows you to dynamically load modules only when they are needed, rather than having to load everything up front. This has some obvious performance advantages.
```javascript
import("./modules/myModule.js").then((module) => {
  // Do something with the module.
});
```
- modules can be loaded dynamically even into a regular script

```javascript
<script>
  import("./modules/square.js").then((module) => {
    // Do something with the module.
  });
  // Other code that operates on the global scope and is not
  // ready to be refactored into modules yet.
  var btn = document.querySelector(".square");
</script>
```

## Top level await

**Top level await** is a feature available within modules.
This means the `await` keyword can be used. It allows modules to act as big asynchronous functions meaning code can be evaluated before use in parent modules, but without blocking sibling modules from loading.

```json colors.json
{
  "yellow": "#F4D03F",
  "green": "#52BE80",
  "blue": "#5499C7",
  "red": "#CD6155",
  "orange": "#F39C12"
}
```

```javascript

// getColors.js
const colors = fetch("../data/colors.json").then((response) => response.json());

export default await colors;

// main.js
import colors from "./modules/getColors.js";
import { Square } from "./modules/square.js";

const square1 = new Square(
  myCanvas.ctx,
  myCanvas.listId,
  50,
  50,
  100,
  colors.blue,
);

```
Result: The code within main.js won't execute until the code in getColors has run. However it won't block other modules being loaded. F.e. the `square.js` module will continue to load while `colors` is being fetched.

## Isomorphic modules
Not every module will run the same way in every environment. F.e. browser environment has a global variable `window`. On the other hand `node` environment has a global variable `process`. Using a module using the global `window` variable will fail in `node` environment.

In order to maximize the reusability of a module, it is advised to make the code **`isomorphic`** - that means it behaves the same in every runtime.
It can be achieved:
- by splitting a module to isomorphic `core` and environment specific `bindings`
- by detecting environment and adjusting behaviour
- by using polyfills for missing functionality (if available)

Typical variables defined based on environment: `process`, `window`, `document`, `fetch`, ...
