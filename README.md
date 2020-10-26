# ☕️ **What-I-Learned-In-Week-6** ☕️
![dude](img/three-arms.gif)
## The conditional (`ternary`) `operator` ⇲<br>
 iIt's the only JavaScript `operator` that takes `three operands`: a `condition` followed by a question mark (`?`), then an `expression` to execute if the condition is truthy followed by a colon (`:`), and finally the `expression` to execute `if` the condition is `falsy`. This operator is frequently used as a shortcut for the if statement.<br>
 * for example ↘︎<br>
```javascript
function getFee(isMember) {
  return (isMember ? '$2.00' : '$10.00');
}

console.log(getFee(true));
// expected output: "$2.00"

console.log(getFee(false));
// expected output: "$10.00"

console.log(getFee(null));
// expected output: "$10.00"

```
## Syntax ⇲<br>
```javascript
condition ? exprIfTrue : exprIfFalse
```

* condition ⇲<br>
    An expression whose value is used as a condition.
* expression `If` `True` ⇲<br>
    An expression which is evaluated if the condition evaluates to a truthy value (one which equals or can be converted to true).
* expression `If` `False` ⇲<br>
    An expression which is executed if the condition is falsy (that is, has a value which can be converted to false). 
## Description ⇲<br>
Besides `false`, possible `falsy` expressions are: `null`, `NaN`, `0`, the empty string (`""`), and `undefined`. `If` condition is any of these, the result of the conditional expression will be the result of executing the expression If False.<br>
* example ⇲<br>
```javascript
var age = 26;
var beverage = (age >= 21) ? "Beer" : "Juice";
console.log(beverage); // "Beer"
```
---
## What’s a mutation? ⇲

An immutable value is one that, once created, can never be changed. In JavaScript, primitive values such as numbers, strings and booleans are always immutable. However, data structures like objects and arrays are not.<br>

By mutation I mean changing or affecting a source element. The goal is to keep the original element unchanged at all times.<br>

## Adding new elements to an array ⇲<br>

- [ ] Array.prototype.push 
- [ ] Array.prototype.unshift 
- [x] Array.prototype.concat 
- [x] Spread Operator.

Array.prototype.push allows us to push elements to the end of an array. This method does not return a new copy, rather mutates the original array by adding a new element and returns the new length property of the object upon which the method was called.<br>
```javascript
const numbers = [1, 2];numbers.push(3); // results in numbers being [1, 2, 3]
```
There’s also Array.prototype.unshift which we can use to add elements to the very beginning of an array. Just as push, unshift does not return a new copy of the modified array, rather the new length of the array.<br>
```javascript
const numbers = [2, 3];numbers.unshift(1); // results in numbers being [1, 2, 3]
```
In these two examples we are mutating or changing the original array. Remember, the goal is to keep the source array untouched. Let’s go through some alternatives to adding new elements to an array without altering the source.<br>

Perhaps the easiest way out would be to create a copy of the source array and push directly to it:<br>
```javascript
const numbers = [1, 2];
const moreNumbers = numbers.slice();moreNumbers.push(3);
```
We could call this a “controlled mutation” as we are indeed mutating an array, although not the original one, but a copy we created for this purpose. However, Array.prototype.slice can be tricky as it does not create a deep clone, rather a shallow copy of the source array.<br>

Array.prototype.concat does indeed return a new array, which is what we are after. We can make use of this method to return a new array with an extra element (or elements) appended to the original one:<br>
```javascript
const numbers = [1, 2];
const moreNumbers = numbers.concat([3]);
```
Just a little heads up: concat also returns a shallow copy of the source array (just as slice does). This means that it only copies the references to both objects and other arrays within our original list. Shall you want to have a complete brand new copy of your array, consider using Lodash’s cloneDeep which recursively clones the array or object passed in as a param:<br>
```javascript
const people = [{ name: 'Bob' }, { name: 'Alice' }];
const morePeople = cloneDeep(people).concat([{ name: 'John' }]);console.log(people[0] === morePeople[0]); // returns false
```
###### [Click here to find more info about this topic](https://medium.com/@fknussel/arrays-objects-and-mutations-6b23348b54aa)
---
## Default parameters ⇲<br>
Default function `parameters` allow named `parameters` to be initialized with default values if no value or `undefined` is passed.<br>
* example ⇲
```javascript
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5, 2));
// expected output: 10

console.log(multiply(5));
// expected output: 5

```
## Syntax ⇲<br>
```javascript
function [name]([param1[ = defaultValue1 ][, ..., paramN[ = defaultValueN ]]]) {
   statements
}
```
In JavaScript, function parameters default to undefined. However, it's often useful to set a different default value. This is where default parameters can help.<br>

In the past, the general strategy for setting defaults was to test parameter values in the function body and assign a value if they are undefined.<br>

In the following example, if no value is provided for b when multiply is called, b's value would be undefined when evaluating a * b and multiply would return NaN. ⇲<br>
```javascript
function multiply(a, b) {
  return a * b
}

multiply(5, 2)  // 10
multiply(5)     // NaN !
```
To guard against this, something like the second line would be used, where b is set to 1 if multiply is called with only one argument:<br>
```javascript
function multiply(a, b) {
  b = (typeof b !== 'undefined') ?  b : 1
  return a * b
}

multiply(5, 2)  // 10
multiply(5)     // 5
```
With default parameters in ES2015, checks in the function body are no longer necessary. Now, you can assign 1 as the default value for b in the function head:<br>
```javascript
function multiply(a, b = 1) {
  return a * b 
}

multiply(5, 2)          // 10
multiply(5)             // 5
multiply(5, undefined)  // 5
```
---
## `module.exports` and `require` ⇲<br>
The [CommonJS (CJS)](https://en.wikipedia.org/wiki/CommonJS) format is used in Node.js and uses `require` and `module.exports` to define dependencies and modules. The npm ecosystem is built upon this format.
## Requiring a `module` ⇲<br>
Node.js comes with a set of built-in modules that we can use in our code without having to install them. To do this, we need to require the module using the require keyword and assign the result to a variable. This can then be used to invoke any methods the module exposes.

For example, to list out the contents of a directory, you can use the file system module and its readdir method:
```javascript
const fs = require('fs');
const folderPath = '/home/jim/Desktop/';

fs.readdir(folderPath, (err, files) => {
  files.forEach(file => {
    console.log(file);
  });
});
```
## Creating and Exporting a `Module` ⇲

Now let’s look at how to create our own `module` and `export` it for use elsewhere in our program. Start off by creating a `user.js` file and adding the following ⇲<br>
```javascript
const getName = () => {
  return 'Jim';
};

exports.getName = getName;
```
Now create an `index.js` file in the same folder and add this ⇲<br>
```javascript
const user = require('./user');
console.log(`User: ${user.getName()}`);
```
Run the program using `node` `index.js` and you should see the following output to the terminal:
```javascript
User: Jim
```
So what has gone on here? Well, if you look at the `user.js` file, you’ll notice that we’re defining a `getName` function, then using the exports keyword to make it available for import elsewhere. Then in the `index.js` file, we’re importing this function and executing it. Also notice that in the require statement, the module name is prefixed with `./`, as it’s a `local file`. Also note that there’s no need to add the file extension.
