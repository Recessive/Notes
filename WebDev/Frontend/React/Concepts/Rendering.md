---
tags:
  - webdev/react
  - js
---
## Rendering happens in order
[[WebDev/Frontend/React/React|React]] expects [[WebDev/Frontend/React/Concepts/Element|Elements]] to appear in the same position within a [[Component]]. If this position changes (e.g., due to conditional rendering) [[State]] may be lost because [[WebDev/Frontend/React/React|React]] tracks [[WebDev/Frontend/React/Concepts/Element|Elements]] by their position in the render tree unless uniquely [[Key|keyed]]. If [[WebDev/Frontend/React/Concepts/Element|Elements]] are *removed* and later re-added, [[State]] will also be lost as [[WebDev/Frontend/React/React|React]] treats them as new instances.

Beyond [[State]], [[WebDev/Frontend/React/React|React]] also suffers in its *rendering* if [[WebDev/Frontend/React/Concepts/Element|Element]] positions change. Because [[WebDev/Frontend/React/React|React]] expects a consistent structure, it uses a **linked-list-based reconciliation process** to compare [[WebDev/Frontend/React/Concepts/Element|Elements]] across renders. If these differ, [[WebDev/Frontend/React/React|React]] will re-render these [[WebDev/Frontend/React/Concepts/Element|Elements]]. By changing the positioning, you are all but guaranteeing the [[WebDev/Frontend/React/Concepts/Element|Elements]] will differ, causing a potentially unnecessary re-render.



## Reconciliation
React has a reconciliation function for determining which areas of a [[Component]] can be re-rendered in place. This allows for [[WebDev/Frontend/React/Concepts/Element|Elements]] to be re-rendered whilst the rest of the unchanged [[Component]] stays the same. This function relies on [[WebDev/Frontend/React/Concepts/Element|Element]] ordering and [[Key|Keys]] where order might change.

**2 things can cause a component to render:**
## Initial Render
Occurs by calling `createRoot`. You will find this in your `main.jsx` file, likely looking something like:
```js
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

During the initial render, React creates the [[WebDev/Frontend/Document Object Model|DOM]] nodes for each [[Component|component]]. It then adds them to the [[WebDev/Frontend/Document Object Model|DOM]] using the `appendChild` DOM API

## State update
Occurs when the [[State|state]] of a [[Component|component]] is updated. This will cause the entire [[Component|component]] to re-run all its code. However, only [[WebDev/Frontend/React/Concepts/Element|elements]] that have changed will be re-rendered.

[[useState]] [[Rule of hooks|hooks]] are slightly hacky: from second renders onwards, they don't initialize the value anymore. Instead, they pass the updated [[State|state]] to the state variable to be used within the component.
