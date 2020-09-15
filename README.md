# js-cheatsheet

# New ES6 JS features
### Variable declaration with let and const
##### Var
The scope of *var* is global when a *var* variable is declared outside a function.
*var* is function scoped when it is declared within a function.
*var* variables are hoisted to the top of their scope and initialized with a value of undefined.
``` js
// If we do this --
console.log (greeter);
var greeter = "say hello"

// It is interpreted as --
var greeter;
console.log(greeter); // greeter is undefined
greeter = "say hello"

// var variables can be re-declared and updated
// This is valid --
var greeter = "hey hi";
greeter = "say Hello instead";

// And so is this --
var greeter = "hey hi";
var greeter = "say Hello instead";

// And this re-declaration feature can sometimes cause problems. Example --
var greeter = "hey hi";
var times = 4;

if (times > 3) {
    var greeter = "say Hello instead"; 
}

console.log(greeter) // "say Hello instead"
```

This is not a problem if you knowingly want greeter to be redefined, it becomes a problem when you do not realize that a variable greeter has already been defined before.

##### Let
*let* is block scoped. *let* can be updated but not re-declared. However, if the same variable is defined in different scopes, there will be no error.

```js
// error
let greeter = "hey hi";
let greeter = "say Hello instead";

// no error
let greeting = "say Hi";
if (true) {
    let greeting = "say Hello instead";
    console.log(greeting); // "say Hello instead"
}
console.log(greeting); // "say Hi"
```

This is because both instances are treated as different variables since they have different scopes. This fact makes let a better choice than var. 

Just like  var, let declarations are hoisted to the top. Unlike var which is initialized as undefined, the let keyword is not initialized. So if you try to use a let variable before declaration, you'll get a Reference Error.

##### Const
Variables declared with the *const* maintain constant values. *const* declarations are also block scoped.
const cannot be updated or re-declared. Every const declaration, therefore, must be initialized at the time of declaration. --

```js
//error
const greeting = "say Hi";
greeting = "say Hello instead";// error: Assignment to constant variable.

// also error
const greeting = "say Hi";
const greeting = "say Hello instead";// error: Identifier 'greeting' has already been declared
```

NOTE: While a const object cannot be updated, the properties of this objects can be updated.
```js
//  if we declare a const object as this -
const greeting = {
    message: "say Hi",
    times: 4
}

// while we cannot do this
const greeting = {
    words: "Hello",
    number: "five"
} // error:  Assignment to constant variable.

// we can do this
greeting.message = "say Hello instead";
```
### Immediately Invoked Function Expression (IIFE) and Block-Scope
It is a JS function that runs as soon as it is defined. 
Examople -
```js
// Anonymous IIFE
(funciton(){
    console.log("You are amazing")
})();
```

Note that this function has no name and is not stored in a variable. The first enclosing paranthesis makes the function an expression. The last two paranthesis tell JS to invoke or call this anonymous function immediately.

```js
// Named IIFE
(praiseMe = function(){
    console.log("You are amazing")
})();

praiseMe()
```

This is mostly used to avoid declaring variables in the global scope (when we use var), and to create closures.
We don't necessarily need to use IIFE with let and const because they are block scoped variables.

##### IIFE vs Block-Scope

```js
// IIFE
var a = 3;
(function(){
    var a = 4;
    console.log(a); //outputs 4 
})();
console.log(a); //outputs 3

// Block Scope
let b=2;
{
    let b=3;
    console.log(b) // outputs 3
}
console.log(b) // outputs 2
```
### Template Strings (Template Literals)
ES6 introduces a new kind of string literal syntax called template strings. They look like ordinary strings, except using the backtick character ( \` ) rather than the usual quote marks ( ' ) or ( " ). 

In the simplest case, they really are just strings --
```js
`Hello World, I am Akshat Agrawal`
```

But this is where the actual magic begins -
```js
function whatsMyName(firstName, lastName){
    return `My name is ${firstName} ${lastName}`
}

console.log(whatsMyName("Akshat", "Agrawal")) // output: My name is Akshat Agrawal
```

### Arrow Functions
```js
// Normal Functions
function hello() {
  return "Hello World!";
}

// Arrow Functions
hello = () => {
  return "Hello World!";
}

// If you have parameters, you pass them inside the parentheses:
hello = (val) => "Hello " + val;

// If you have only one parameter, you can skip the parentheses as well
hello = val => "Hello " + val;
```

### Spread and Rest Operators

##### Spread
These operators help add the elements of an existing array into a new array.
```js
// Consider the following --
let arr1 = [1, 2, 3, 4, 5]
let arr2 = [arr1, 6, 7, 8]
console.log(arr2) // output: [[1, 2, 3, 4, 5], 6, 7, 8]

// But we wanted a non-nested array. In order to do this, we use spread operator --
let arr1 = [1, 2, 3, 4, 5]
let arr2 = [...arr1, 6, 7, 8]
console.log(arr2) // output: [1, 2, 3, 4, 5, 6, 7, 8]
```

##### Rest
It has same syntax as that of Spread. The rest parameter allows us to pass an indefinite number of parameters to a function and access them in an array.

###### Spread vs Rest
```js
// Spread
function spreadEx(x, y, z){
    // do something
}
let arr = [1, 2, 3]
spreadEx(...arr)

// Rest
function restEx(...args){
    // do something
}

restEx(1, 2, 3)
```

### Destructuring
Destructuring Assignment is a JavaScript expression that allows to unpack values from containers like arrays, objects etc.
##### Array Destructuring
```js
// Example 1
var x, y;
[x, y] = [10, 20];
console.log(x); // 10
console.log(y); // 20

// Example 2
[x, y, ...restof] = [10, 20, 30, 40, 50];
console.log(x); // 10
console.log(y); // 20
console.log(restof); // [30, 40, 50]
```

##### Object Destructuring 
```js
// Example 1
({ x, y} = { x: 10, y: 20 });
console.log(x); // 10
console.log(y); // 20

// Example 2
({x, y, ...restof} = {x: 10, y: 20, m: 30, n: 40});
console.log(x); // 10
console.log(y); // 20
console.log(restof); // {m: 30, n: 40}
```

##### Some more use-cases of Array Destructuring
```js
// Example 1: Partial Destructuring
var [firstName, secondName] = ["alpha", "beta", "gamma", "delta"]; 
console.log(firstName);//"alpha" 
console.log(secondName);//"beta 

// Example 2: array elements can be skipped
var [firstName,,thirdName] = ["alpha", "beta", "gamma", "delta"];
console.log(firstName);//"alpha" 
console.log(thirdName);//"gamma"

// Example 3: Swapping of variables
[firstName, secondName] = [secondName, firstName] 
```
