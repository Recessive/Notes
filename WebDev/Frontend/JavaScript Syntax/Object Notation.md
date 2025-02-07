---
tags:
  - webdev
  - js
---
Objects can contain anything. Follows JSON.

Interestingly, you can define functions that self reference in an object:

```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',

  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

arto.greet()  // "hello, my name is Arto Hellas" gets printed
```
and can be assigned after definition:
```js
arto.growOlder = function() {  
	this.age += 1
}
```


Defining a function using [[Arrow functions|Arrow notation]] however means you cannot access `this` at all.

Accessing an arbitrary key inside an object is done so inline via:
```js
arto["name"]
```

Creating an object with an arbitrary key is done via:
```js
const age_key = "age"
const arto = {
	["name"]: 'Arto Hellas',
	[age_key]: 35
}
```
