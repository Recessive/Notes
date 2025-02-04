---
tags:
  - webdev/react
link: https://www.w3schools.com/react/react_state.asp
---

[[React]] states are used to call the renderer. When you update a stateful value, all components that use that value will be re-rendered. To create a stateful value, use `useState`

### Example Without `useState`

Consider a simple counter example without `useState`:
```js
import React from 'react';

function Counter() {
  let count = 0; // A regular variable, not state

  const handleClick = () => {
    count += 1; // Update the variable
    console.log(count); // Log the updated value
  };

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={handleClick}>Click me</button>
    </div>
  );
}

export default Counter;
```


In this example:

- The `count` variable is updated when the button is clicked.
- The console will log the updated value of `count`.
- However, the displayed value in the UI (`<p>You clicked {count} times</p>`) will not change because the component does not re-render when `count` changes.

### Example With `useState`

Now, consider the same example using `useState`:

```js
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // State variable

  const handleClick = () => {
    setCount(count + 1); // Update the state
  };

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={handleClick}>Click me</button>
    </div>
  );
}

export default Counter;
```

In this example:

- The `count` state variable is updated using the `setCount` function.
- When `setCount` is called, React schedules a re-render of the `Counter` component.
- The UI is updated to reflect the new state value, so the displayed count changes as expected.