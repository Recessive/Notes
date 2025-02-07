---
tags:
  - webdev/react
  - js
---
Iterators that contain [[WebDev/Frontend/React/Concepts/Element|Elements]] in [[WebDev/Frontend/React/React|React]] must have a [[Key]] for each element. This is because [[WebDev/Frontend/React/React|React]] uses the [[Key]] attributes in an array to determine how to update the JSX on re-render.

**Do not use indices as keys**. This is because if the iterator ever changes, the indices might not align with the same elements they did on the previous [[Rendering|render]], causing all kinds of issues. See [[Rendering]] for more.