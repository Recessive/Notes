---
tags:
  - webdev
  - js
aliases:
  - Object spread
---
Behaves the same as a python spread syntax, works on objects too:

```js
const f = (a, b, c) => {console.log(`${a}, ${b}, ${c}`)}

const arr = [1, 2, 3]

f(...arr) // Logs "1, 2, 3"
```