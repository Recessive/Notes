---
tags:
  - webdev/react
link: https://www.w3schools.com/react/react_components.asp
---
Independent reusable code. Are essentially immutable functions that return HTML (can also be classes). Should be pure


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

Pretty much everyone uses functions tho