---
tags:
  - webdev
  - js
link: https://egghead.io/courses/understand-javascript-s-this-keyword-in-depth
---
`This` is fucking braindead.

Following the [[Object Notation]] note, you can see you can define a function in an object. However, if you create a reference to that function, it loses the context of `this`.
```js
arto.greet()       // "hello, my name is Arto Hellas" gets printed

const referenceToGreet = arto.greet
referenceToGreet() // prints "hello, my name is undefined"
```

What `this` is depends on *who* is calling the function. When you do `arto.greet`, *arto* is calling the function. However if you create a reference to the function, and call that reference, whatever context that reference is in is the one calling the function, therefor `this` no longer points to *arto*.

You can preserve the original `this` by using `.bind`:
```js
arto.greet()       // "hello, my name is Arto Hellas" gets printed

const referenceToGreet = arto.greet.bind(arto)
referenceToGreet() // prints "hello, my name is Arto Hellas"
```