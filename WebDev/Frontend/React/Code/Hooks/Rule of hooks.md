---
tags:
  - webdev/react
  - js
aliases:
  - hook
  - hooks
---
Hooks must be called *unconditionally* and in the *same order* on every render. These rules exist because hooks are stored in a linked list associated with the [[Component]] they're used in. [[WebDev/Frontend/React/React|React]] relies entirely on order to ensure hooks are associated with the correct values.

Hooks must not be called from within a loop, conditional expression or any place that isn't a *function* defining a component.

This essentially means, only components that aren't classes should have states used in them. Defining states elsewhere is psychotic.
```js
const App = () => {
  // these are ok
  const [age, setAge] = useState(0)
  const [name, setName] = useState('Juha Tauriainen')

  if ( age > 10 ) {
    // this does not work!
    const [foobar, setFoobar] = useState(null)
  }

  for ( let i = 0; i < age; i++ ) {
    // also this is not good
    const [rightWay, setRightWay] = useState(false)
  }

  const notGood = () => {
    // and this is also illegal
    const [x, setX] = useState(-1000)
  }

  return (
    //...
  )
}
```

## *... within a loop ...* 
Defining a hook within a loop is... insane. You're just overwriting the value, and needlessly fucking with React.

Even worse, if the number of iterations *varies*.... my god.

React *needs* the order and number of hook calls to stay the same in order to properly handle state, otherwise it might miss a state update between renders. The state only updating every nth render is a path to complete madness.

## *... conditional expression ...*
Much like a varying loop, a conditional expression will inevitably cause React to lose proper hook order and count in a [[Rendering|render]] when the condition isn't met. 

Absolute *best* case scenario, you are using only one hook and it doesn't matter when it gets triggered. But it's just horrendous practice, please don't do this

## *... outside a functional component ...*
[[State]] is bound to a [[Component|component]], inside a Class [[Component]] [[State|state]] is found in the `this.state` value, and updated using [[setState]].

However in a Functional [[Component]], the state can only be interacted with via [[useState]]. If you try to use the [[useState]] function outside a Functional [[Component]], there will be no [[State]] for [[WebDev/Technologies/React|React]] to bind to.

I'm pretty sure other hooks have similar issues.