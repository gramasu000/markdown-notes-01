# Javascript: `this` keyword in functions

A function (defined with `function` keyword) can be a property of an object, in which case it is called a method. Below is an example.

```
const LoggerObject = {
    log: function(logOutput) {
        console.log(logOutput)
    }
}
```

In this example, `log` is a method of `LoggerObject`, and can be called as follows

```
LoggerObject.log('Hello World');
```
