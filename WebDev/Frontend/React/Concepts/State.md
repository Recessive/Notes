---
tags:
  - webdev/react
  - js
link: https://stackoverflow.com/questions/37755997/why-cant-i-directly-modify-a-components-state-really/40309023#40309023
---
State is the mechanism used by React to allow [[Component|components]] to change over time. When a [[Component|components]] state is updated, the [[Component|component]] is re-rendered. This allows for responsive behaviors and immediate re-renders on value updates.

State is designed to be immutable. There are two ways of updating state, [[useState]] and [[setState]]. If you mutate the state whilst using either of these two methods, you will run into unique problems.

## useState
[[useState]] is used by functional [[Component|components]] and gives a state variable and a setter function for said variable. 

This state is stored in a linked list that is associated with the [[Component]]. It does not retain what variable should have what value, instead relying on order, so it is important that [[useState]] is called every time and always in the same order. (see [[Rule of hooks]])

If you modify the variable directly (i.e. circumvent the setter function) the change simply wont be reflected. React does not know its value has changed. To notify react of the change, you must call the setter function.

However, when you call the setter function you provide a new value to the state variable, which will overwrite its current value. This essentially means the mutation you did earlier is redundant, as you end up replacing that value anyway.

## setState
[[setState]] is used by class [[component|components]] and allows you to modify the classes `this.state` value. This method is a bit more nuanced, it actually performs a shallow merge and join between the previous state and whatever value you passed to it (which is considered the state update).

By mutating the previous state (`this.state`) directly, you interfere with the merge. No re-render will be triggered (much like [[useState]]) until [[setState]] is called, but when it is called the old state that has been mutated will be compared to whatever new state you have passed to [[setState]]. This leads to very unclear code.

If you are mutating `this.state`, it makes infinitely more sense to just wait until calling [[setState]] and pass a new, updated version so you can be clear on what the value of `this.state` is at all times.

## Asynchronicity

Interacting with the state is an asynchronous operation. The only guarantee you have on state is upon re-render, where the state will have updated. From line-to-line, the value wont have updated.

For example:

```js
const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
    setTotal(left + right)  
}
```

In here, `set_total` will receive the outdated value of `left`, as `left` was updated with `setLeft` but this state update has not occurred yet. This outdated version of `left` is considered *stale*.

React waits until the function the setter function was called in to complete before re-rendering.

## Rendering pit fall
If you have a state update directly inside a component that always triggers, you will cause infinite re-renders. This is because the function the state update was called in is the component itself, so when the component is finished rendering, React then re-renders it since its state was updated. 

This causes another state update, then re-render, then state update and so on.