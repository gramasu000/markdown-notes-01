# React Effects

### Overview

A React Effect function is an arrow function which executes at certain times.

Depending on how an effect is coded, the arrow function can run

- Once when the component is mounted and once every time the component is rerendered
- Once when the component is mounted and once every time certain variables change value
- Once when the component is mounted and that's it

With every React Effect, the developer can specify an optional cleanup function. A cleanup function is a function that literally "cleans up" after an Effect.

For example, if an Effect opens up a connection to an API that you need to disconnect from later, you can perform that disconnect in the cleanup function.

A cleanup function is called once before every _subsequent_ Effect function call (i.e. every Effect function call minus the first) plus once when the component unmounts.

### Caveat

Everything mentioned above about when Effect functions and cleanup functions are executed are true in production mode.

However, in React development mode, there is a caveat.

During the component mount, in prod mode, a React Effect function executes once.

However, in dev mode, during component mount, the React Effect function executes once, the cleanup function executes once (if it exists) and the React Effect function executes again.
The React Effect is in fact executed twice during component mount.

This behavior was decided upon by the people who design React so that the React developer can identify bugs in the cleanup function.
