---
tags:
  - webdev/react
  - js
aliases:
  - Elements
---
A [[WebDev/Frontend/React/React|React]] element is an object representing some part of the [[WebDev/Frontend/Document Object Model|DOM]], ie, representing a classic HTML [[WebDev/Concepts/Element|Element]]. It is normally [[JSX]], but can be pure JavaScript elements using the original syntax:
```js
React.createElement("h1", null, "Hello, world!");
```