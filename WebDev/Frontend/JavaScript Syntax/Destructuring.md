---
tags:
  - webdev/react
  - js
---
Used to unpack iterables.

Quite often used in React in regards to [[Prop|props]]. Syntax is:
```js
const person = {
	name: "Me",
	age: 25
}

const {name, age} = person
```

However in React the most common location is in [[Component]] definitions. The destructuring can occur implicitly in the definition, making a function that looks like:
```js
const Component = (props) => {
	const {name, age} = props
	...
}
```
look like:
```js
const Component = ({name, age}) => {
	...
}
```
for brevity.