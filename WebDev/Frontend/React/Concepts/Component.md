---
tags:
  - webdev/react
link: https://www.w3schools.com/react/react_components.asp
---
Independent reusable code. Are essentially immutable functions that returns [[JSX]] (can also be classes). Should be pure. Returns an [[WebDev/Frontend/React/Concepts/Element|Element]]

The first letter of the class, function name or variable storing the function **must be capital**. This is what tells [[JSX]] the code is a component instance, and should be treated as [[JSX]], otherwise it is treated as HTML.


To create a component, either declare a standard js/ts function, or create a class that inherits from `React.Component`:

### Class:
```tsx
class Car extends React.Component {
  render() { // render must be included
    return <h2>Hi, I am a Car!</h2>;
  }
}
```

### Function:
```tsx
function Car() {
  return <h2>Hi, I am a Car!</h2>;
}
```

Pretty much everyone uses functions tho.


Every time a state variable that is used within a component is updated via its setter function defined in [[useState]], the entire component is re-rendered. This effectively means all code within the component will be run again. React doesn't know if the updated value will effectively change anything upon render, so it's important to be careful when using state.

## Special props
`key` and `ref` are reserved [[Prop|props]] for a component.

They are used internally for [[Rendering#Reconciliation|reconciliation]] (in the case of `key`) or to reference [[WebDev/Frontend/Document Object Model|DOM]] nodes or component instances (in the case of `ref`). Importantly, they are **not available** inside your component as part of the `props` object.

## Pitfalls
Never define components inside other components:

```js
const Car = () => {
	const CarHeader = () => {
		return <h1> Header </h1>
	}

	return (<>
	<>
		<CarHeader />
		<h2>Hi, I am a Car!</h2>
	</>
	)
}
```

This will cause `CarHeader` to be recreated every [[Rendering|render]]. If `CarHeader` has any kind of [[State|state]], it will always lose it between renders. The recreation of this component prevents React from optimizing it as well.