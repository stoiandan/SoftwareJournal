# Memory Allocation in JS

In Ecamscript (or JS, but don't tell Oracle), the `Object` type has a bunch of methods to copy memory around. We'll look at two, but first:


## Introduction to prototyping

So in Ecmascript, objects have a special property `__proto__`, that is used in the so called "lookup-chain" mechanins. Where basically, 
if a property of on object is not found, it's further down searches by accessing the `__proto__` property, which, besids holding properties, it often itself has
a `__proto__` property, and so on, up to simply `null`.

It's like inheritence in OOP, only sort of more explicit, by having the `__proto__` property point to the next ancestor, up to `Object` and then `null`.

if you have a type:

```js
   function Dog(name) {
      this.name = name
   }
   const spark = new Dog('spark');
```

You can add properties to every `Dog` instance by:

```js
   function Dog(name) {
      this.name = name
   }
   Dog.prototype.bark = () => console.log("how-how!"); 
   const spark = new Dog('spark');
```

So now all dogs have a `bark` method. Note instances of `Dog` have a `__proto__` method that point to `Dog.prototype`, they're not the same things, one points to another.

Ecmascript recomments not to use `__proto__`, but `Object.getPrototypeOf()` method. For example:

```js
 function Dog(name) {
      this.name = name
   }
const spark = new Dog('spark');

Object.getPrototypeOf(spark) === Dog.prototype // true
```
So in that sense, `__proto__` and `Object.prototype` (of any specific type, not just Object) are the same. But you could change a object instance's `__proto__` property to anything, in that sense, they're not the same.


## Object.assing


Moving forward, `Object.assign` will take a target and a source, and copy all properites of the target, to the _source_ (you can specify as many sources as you like).

```js
const target = { a: 1,};
const source = { b: 4};

const returnedTarget = Object.assign(target, source);

source.b = 123;

console.log(target);
// Expected output: Object { a: 1, b: 4 }

```

However, when dealing with simple value types like numbers, this might fool us into thinking `Object.assign` can be used to create deep copies, by having the target be a new, empty object (e.g. `Object.assign({},…)`), but:

```js

const complexObject = { a: { b: 4} };

const newObject = Object.assign({}, complexObject);

complexObject.a.b = 5;

newObject.a.b // also 5
```

Deep copying needs other methods like `JSON.stringify` to write in "solid rock" and the turning back into an object with `JSON.parse`.


## Object.create

`Object.create` now becomes much more easy to explain, all it does is to create a new object who's _prototype_ is the paramaeter(s) of the `create` method
