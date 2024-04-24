# Javascript Functions

A function in Javascript, either regular or arrow, is basically a special type of object.

It is stored in the heap and a reference to that function is held by the function identifier/name.

It is special type of object because it is callable, i.e. it behaves like a function.

There are two types of functions in javascript.

- Functions that are declared with the `function` keyword.
- Arrow functions, declared with the `() => {}` syntax

Regular functions have certain properties that arrow functions do not have. Regular functions can

- Bind an object to the `this` keyword
- Use the `arguments` variable
- Can be called with `new` keyword
- Can use `super` keyword
