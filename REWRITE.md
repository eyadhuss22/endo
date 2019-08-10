## export const

```js
export const { abc: abc2, nest: [def2] } = obj;
```

```js
// temporal dead zone for const decls until...
const { abc: abc2, nest: [def2] } = obj;
$h_once.abc2(abc2); $h_once.def2(def2); // ... here.
```

## export let

```js
export let { abc: abc2, nest: [def2] } = obj;
```

```js
// temporal dead zone for let decls until...
{
  // Private scope so that the let expression can be
  // reused without interfering with the proxy traps.
  let { abc: abc2, nest: [def2] } = obj;
  $h_live.abc2(abc2); $h_live.def2(def2); // ... here.
  undefined;
};
```

## export var

```js
export var { abc: abc2, nest: [def2] } = obj;
```

```js
$h_live.abc2(); $h_live.def2(); // hoisted decls (no tdz)
...
{
  // Same as `let`, since our proxy traps will treat the
  // identifier as a hoisted declaration.
  let { abc: abc2, nest: [def2] } = obj;
  $h_live.abc2(abc2); $h_live.def2(def2);
  undefined;
}
```

## export function

```js
export function fn() {
  ...
};
```

```js
$h_live.fn(); // hoisted decl (no tdz)
...
(() => {
  // Use an IIFE to preserve function semantics of `fn`
  // without polluting the global scope (we need the proxy
  // traps for fn)
  function fn() {
    ...
  }
  $h_live.fn(fn);
})();
```