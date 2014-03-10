# Javascript

[READ THIS](https://docs.google.com/a/redfin.com/presentation/d/1L4F_Po7tLhDyv8SPMUY2kz-K6ip0GKICGcxxaLgRW4U/edit#slide=id.p14)

A great deal of this section will show you how to do somthing, then tell you never to use it.  Javascript is a very loose language.  Instead of having a compiler and IDE that will help you write better code, you have to rely more on your own knowlage.  






## Built Ins (Fundemental objects)
JavaScript has a number of built-in types that you'll use all the time.  Almost everything is Javascript extends the basic Object, so everything behaves somewhat like an Object.

*** Common Object Types ***

- Objects - { foo: 'bar'}
- Strings - "a string!"
- Numbers- 293.33241
- Booleans - true, false
- Arrays - [1, 2, 3]
- Functions
- Dates
- Errors


### Objects

### Strings

### Numbers

All JavaScript numbers are floating point, with all the quirks that implies

### Arrays

### Booleans

### Errors

There are quite a few types of Errors in Javascript, but generally you won't have to use them.  

### Dates

### Functions

Functions are very special.  We'll talk more about them later.

If you don't specify a return statement, a function will return undefined





## Debugging

You'll see in later examples a couple ways that we can do debugging.

### The Console

console.log()

### The Debugger











## Variable Declaration 

Variables are declared with the **var** keyword.

However they can also be declared without it.  

### Function Scope

Variables declared without the **var** keyword go into the global scope, and we almost never want this.  Make a mental note, then don't use it.

Locally declared variables (those declared with **var**) have **function scope** not block scope (or any other scope)

````JS
function setBar() {
	var bar = 42;
	console.log(bar) 
}
setBar();
console.log(bar);

````

** Output **
````JS 
42
Undefined
````


### More on Functions

Functions can be either declared, expressed, or both.  (whooo?!?!?)

#### Function Declaration

````JS
function sayHello(name) {
	console.log('Hello ' + name);
}
sayHello('Kevin');

````

**Output**
````JS 
Hello Kevin
````

#### Function Expression

````JS
var sayHello = function(name) {
	console.log('Hello ' + name);
}
sayHello('Kevin');
````

**Output**
````JS 
Hello Kevin
````


#### Named Function Expression



````JS 
var sayHello = function doHello(name) {
	console.log('Hello ' + name);
}
sayHello('Kevin');
doHello('Kevin');

````

**Output**

````JS 
Hello Kevin
error: doHello is not defined
````

Take a second to look at this. It appears to have two names: sayHello and doHello. There are a couple of key differences between those names though: doHello can only be called inside the function - it isn't available outside.

The primary use case for these is giving JS debuggers a hint as to what they should display as the name of the function in a stack trace.  Without the doHello, the debugger wouldn't know what the name of the function was -- it's technically an anonymous function assigned to a variable. With the name, it has something to display. 

Our JS framework does some internal trickery to to make it so that we don't have to name all of our class methods like this. 


### Variable Hoisting

````JS
function sumNumbers(){
	var numbers = [1, 2, 3, 4, 5];
	var sum = 0;
	for (var i = 0; i < numbers.length; i++) {
		var number = numbers[i];
		sum = sum + number;
	}
	console.log(sum, number);
}
````

Actually, its like this:


````JS
function sumNumbers(){
	var numbers;
	var sum;
	var number;
	var i;
	
	numbers = [1, 2, 3, 4, 5];
	sum = 0;
	
	for (i = 0; i < numbers.length; i++) {
		number = numbers[i];
		sum = sum + number;
	}
	console.log(sum, number);
}
````







## Truthyness & Equality

Truthyness in Javascript gets a little bit tricky, and people new to the language often make mistakes due to value coersion. Generally, this section blows peoples mind, in a rather annoying way. In short JavaScript has two kinds of equality:

=== means the objects are identical

== tries to perform type coercion on its arguments before comparing

Effective JavaScript has a table for coercion rules. You should study it, and play with it, and then use ===

Again, prefer === and !==


Every value in JavaScript coerces to true except:

- 0
- NaN (not a number)
- ""
- false
- null
- undefined


### Examples

var x = 0;
if (!x) {
	// will execute
}

var y = null;
if (y) {
	// will not execute
}
if (!y) {
	// will execute
}

if (37) {
	// will execute
}

var obj = {};
if (obj.foo /* undefined */) {
	// will not execute
}

var x = 0;
if (!x) {
	// will execute
}

var y = null;
if (y) {
	// will not execute
}
if (!y) {
	// will execute
}

if (37) {
	// will execute
}

var obj = {};
if (obj.foo /* undefined */) {
	// will not execute
}

var x = 0;
if (!x) {
	// will execute
}

var y = null;
if (y) {
	// will not execute
}
if (!y) {
	// will execute
}

if (37) {
	// will execute
}

var obj = {};
if (obj.foo /* undefined */) {
	// will not execute
}

Equality Testing

if ("123" == 123) {
	// this block will run
}

if ("123" === 123) {
	// this block will not run
}

0 == false 		// => true

null == undefined 	// => true

null === undefined 	// => false

typeof null 		// => "object"

typeof undefined 	// => "undefined"




