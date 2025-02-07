---
tags:
  - webdev
  - concept
  - js
aliases:
  - JavaScript XML
---
A syntax extension for JavaScript that allows inline HTML-like markup. Extremely similar to HTML, but not identical. 

On complication, the JSX is transformed to standard JavaScript, using direct [[WebDev/Frontend/Document Object Model|DOM]] manipulation:
```jsx
const element = <h1>Hello, world!</h1>;
```
Goes to:
```js
const element = React.createElement("h1", null, "Hello, world!");
```

Translation is handled by Babel