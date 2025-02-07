---
tags:
  - webdev/react
  - js
aliases:
  - Keys
link: https://react.dev/learn/preserving-and-resetting-state#option-2-resetting-state-with-a-key
---
[[JSX]] keys in React serve as a way to circumvent the [[Rendering]] order [[WebDev/Frontend/React/React|React]] would typically follow. 

They are required for iterables that produce [[JSX]].

**Do not use the index of an element as its key**. This would cause an issue if the array changes in any way *other* than appending an element to the end. If an element is removed or added anywhere else, the indices no longer align with the same [[JSX]] they referred to on the previous render, and as such [[WebDev/Frontend/React/React|React]] will shift the state to the wrong element.

Additionally, even if the elements don't have any state, [[WebDev/Frontend/React/React|React]] will not be able to reconcile the elements to the previous state, and will re-render all mis-matched elements, causing many unnecessary re-renders.

## Retaining unique state
Consider the following
```js
import { useState } from 'react';

export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button>
    </div>
  );
}

function Counter({ person }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
```

If the code were instead:


```js
{isPlayerA ? (
	<Counter person="Taylor" />
  ) : (
	<Counter person="Sarah" />
  )}
```

the state between each `Counter` [[Component]] would be shared, as [[WebDev/Frontend/React/React|React]] sees them as the same [[Component]] since they occupy the same space in the [[JSX]] ordering.