---
tags:
  - webdev/react/hooks
  - js
---
Syntax:
```js
const [ counter, setCounter ] = useState(0)
```
Used to define a state. It returns two values, the state variable and a setter function for updating the state variable.

Every time the setter function is called, all [[Component|components]] that depend on the state variable will be re-rendered.
