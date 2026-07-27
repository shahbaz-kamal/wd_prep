# 6. Web / HTML / CSS

- Semantic tags: `<header> <nav> <main> <article> <section> <aside> <footer>`. Improve accessibility and SEO.
- Block vs inline vs inline-block. `display: none` (removed) vs `visibility: hidden` (space kept).
- Box model: content → padding → border → margin. `box-sizing: border-box` includes padding+border in width.
- Position: static, relative, absolute (nearest positioned ancestor), fixed (viewport), sticky.
- Specificity: inline (1000) > id (100) > class/attribute/pseudo-class (10) > element (1). `!important` overrides all.
- Flexbox = one dimension. Grid = two dimensions.
- `em` relative to parent font size; `rem` relative to root.
- Pseudo-class (`:hover`) vs pseudo-element (`::before`).

**JavaScript**
- `var` (function-scoped, hoisted as undefined), `let`/`const` (block-scoped, temporal dead zone).
- `==` coerces types, `===` does not.
- Closure — a function retaining access to its lexical scope.
- Event loop: call stack → microtasks (promises) → macrotasks (setTimeout). Microtasks drain first.
- Hoisting: declarations lifted, initializations not. Function declarations fully hoisted.
- `this` depends on call site; arrow functions inherit `this` lexically.
- Prototypal inheritance via the prototype chain.
- `null` (intentional absence) vs `undefined` (not assigned). `typeof null === "object"` — a known bug.
- Falsy values: `false, 0, -0, 0n, "", null, undefined, NaN`.
- Shallow copy: spread/`Object.assign`. Deep: `structuredClone`.
