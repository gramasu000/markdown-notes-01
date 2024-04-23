# Javascript Memory Management

Javascript has seven primitive data types, namely `number`, `string`, `boolean`, `undefined`, `null`, `bigint` and `symbol`.

When we declare a variable and assign it to a primitive value, the value is stored in stack memory.

Javascript has an eighth data type, one which is not primitive, namely, the `Object` type.

When we declare a variable and assign it to an object, the object is stored in heap memory. The variable, located in the stack, holds a reference (i.e. pointer or memory address) of the object in heap memory.

A javascript object is a collection of values indexed by keys, which are strings. It is possible that object `A` can have object `B` nested inside of it.

In that case, inside object `A` (in the heap) there is a reference to another part of the heap where object `B` exists. The reference to object `A` will be in the stack, and the reference to object `B` is inside object `A`.

An object is "reachable" from the stack if there exists a sequence of references that leads to the object. In the above example, object `B` is reachable from the stack, because the stack has a reference to `A`, and object `A` has a reference to `B`. There is a sequence of two references that leads from the stack to object `B`.

There is a garbage collector that is running whenever javascript code runs. The basic task of this garbage collector is to delete objects that are no longer reachable from the stack and thus free up that heap memory.

_Author: Gautam Ramasubramanian_

_Last Updated: April 23, 2024_
