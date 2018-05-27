### How does `this` work?

#### Answer

The `this` keyword is an object that represents the context of an executing function. Regular functions can have their `this` value changed with `.call`, `.apply` and `.bind`. Arrow functions implicitly bind `this` so that it refers to the context of its lexical environment, regardless of whether or not its context is set explicitly with `call`.

Here are some common examples of `this`:

```js
// Object literals
var myObject = {
  regularFunction: function() {
    return this
  },
  arrowFunction: () => {
    return this
  }
}
myObject.regularFunction() // myObject
myObject.arrowFunction() // NOT myObject
const withoutContextFunction = myObject.regularFunction
withoutContextFunction() // NOT myObject

// Event listeners
document.body.addEventListener("click", function() {
  console.log(this) // document.body
})

// Classes
class myClass {
  constructor() {
    console.log(this) // myClassInstance
  }
}
var myClassInstance = new myClass()

// Manual
var myFunction = function() {
  return this
}
myFunction.call({ customThis: true }) // { customThis: true }

// Unwanted `this`
var obj = {
  arr: [1, 2, 3],
  doubleArr() {
    return this.arr.map(function(value) {
      // this === this.arr
      return this.double(value)
    })
  },
  double() {
    return value * 2
  }
}
obj.doubleArr() // Uncaught TypeError: this.double is not a function
```

#### Good to hear

* In non-strict mode, global `this` is the global object (`window` in browsers), while in non-strict mode global `this` is `undefined`.
* `Function.prototype.call` and `Function.prototype.apply` set the `this` context of an executing function as the first argument, with `call` accepting a variadic number of arguments thereafter, and `apply` accepting an array as the second argument which are fed to the function in a variadic manner.
* `Function.prototype.bind` returns a new function that enforces the `this` context as the first argument which cannot be changed by other functions.
* If a function requires its `this` context to be changed based on how it is called, you must use the `function` keyword. Use arrow functions when you want `this` to be the surrounding (lexical) context.

##### Additional links

* [`this` on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

<!-- tags: (javascript) -->

<!-- expertise: (2) -->