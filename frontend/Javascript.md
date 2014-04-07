# Javascript

[READ THIS](https://docs.google.com/a/redfin.com/presentation/d/1L4F_Po7tLhDyv8SPMUY2kz-K6ip0GKICGcxxaLgRW4U/edit#slide=id.p14)

A great deal of this section will show you how to do somthing, then tell you never to use it.  Javascript is a very loose language.  Instead of having a compiler and IDE that will help you write better code, you have to rely more on your own knowlage.  






## Built Ins (Fundemental objects)
JavaScript has a number of built-in types that you'll use all the time.  Almost everything is Javascript extends the basic Object, so everything behaves somewhat like an Object.

**Common Object Types**

- Objects - { foo: 'bar'}
- Strings - "a string!"
- Numbers- 293.33241
- Booleans - true, false
- Arrays - [1, 2, 3]
- Functions
- Dates
- Errors

You can often detirmine the type of variable by using *typeof*

````JS
var name = 'Kevin';
console.log(typeof(name));
````
**Output**
````JS 
"string"
````



### Objects

Javascript objects are a store for unordered name value pairs called properties.  A properties value can be almost anything in Javascript such as numbers, strings, and other objects or even errors and functions!  For those of you with other programming experience Objects are like *dictionaries* or *maps*.

Object properties can be accessed by either dot notation or bracket notation.  A property name can be any valid string, but not all names can be accessed via dot-notation.  Reserved words must be set with bracket notation.  Other names that are not valid javascript identifiers (e.g. starts with a number or contains an hyphen) must be accessed and set with bracket notation.


````JS
var house = {
	price: 450000,
	location: {
		street: "123 Main St",
		city: "Seattle",
		state: "WA"
	},
	rooms: [
		kitchen,
		bathroom,
		bedroom
	],
	getNeighborhood: function() {
		return "Capitol Hill";
	},
	"class": "townhouse",   				// Reserved words must be quoted
	"display-online": true  				// Invalid names must be quoted
}

console.log(house.price);
console.log(house.location.street); 
console.log(house.class)  					// Names of reserved words can be accessed with dot-notation
console.log(house['display-online']);    	// Invalid names must be accessed with bracket notation
````


### Strings

Strings are simple text stores, but also have handy utility functions such as .length.

Strings can be concatenated with a plus symbol + ' ';

````JS
var location = 'Seattle, ' + 'Washington';
console.log(location);
console.log(typeof(location));
console.log(location.length)
````
**Output**
````JS 
"Seattle, Washington"
"string"
19
````

### Numbers

All JavaScript numbers are floating point, with all the quirks that implies.  A number that is concatenated with a string outputs a string.

### Arrays

### Booleans

### Errors

There are quite a few types of Errors in Javascript, but generally you won't have to use them.  

### Dates

### Functions

Functions are very special.  We'll talk more about them later.

If you don't specify a return statement, a function will return undefined.

Functions are first class objects, they can be passed around just like any other variable.





## Debugging

Most modern browsers have fairly good debugging capabilites built in.  They allow you to set breakpoints, log things out, and inspect various objects. This tutorial applies mostly to Chrome, but other browsers have debugging capabilities as well.

### The Console

````JS 
console.log();
````

You can use the console.log() statement in your scripts to output information to the javascript console.  The log statment does not stop execution and is an exelent way to quickly check if variables are set to their intended values or if functions are running in the expected order.

While the debugger is paused (see next section), you can used the console to examine and manipulate variables that are in scope.

### The Debugger

````JS 
debugger;
````

Add a debugger statement on any line in a JS file to have the browser debugging tools pause there.

Caveat: the dev tools usually have to be open in order for the debugger statement to be respected. 

In Chrome, you can also add breakpoints in the by opening the sources tab and clicking a line number.

Once the debugger has triggered, in the sources tab you will have controls to step though the code.

The watch pane allows you to type arbitrary expressions that will be evaluated while stepping

Scope variables show you both local variables, and the variables available via the closure. 

You can edit the values in the Scope Variables pane as well. 

To quickly test a code change, you can edit scripts directly in the Sources pane. 
Click into the file's contents and start editing. 
Ctrl+S/Cmd+S "saves". 

Changes aren't saved to disk, and don't persist across page loads. 






## Variable Declaration 

Variables are declared with the **var** keyword.

However they can also be declared without it.

### The "new" Keyword

The new keyword in javascript creates an instance of either a user defined object or a built in.  There is a great deal of debate in the Javscript community on what proper best practices are for using the new keyword.  For brevity I'll offer this advice

***Do***

- Use 'new' for creating new instances of dojo classes
- Use 'new' for creating new Date instances
- Create instances of built ins with their simple constructor
````JS
var myArray = ['a', 'b', 'c'];
````

***Don't***

- Use 'new' for creating arrays, objects, strings, and other built ins.  
````JS
var myArray = new Array('a', 'b', 'c');
````


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

**Output**
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

Variable declarations in JavaScript are hoisted to the top of their containing function.  Consider pre-empting confusion, and doing the hoisting yourself. 

#### Simple Variable Hoisting

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

#### Functions and Hoisting

Function declarations are subject to hoisting, but function expressions are not.


### Closures

All javascript functions are closures.  This means that they can refer to variables that were in scope when they were defined.  This is an extemely useful tool.

````JS
function makeCounter() {
	var count = 0;
	return function() {
		count += 1;
		return count;
	}
}
var count = makeCounter();
console.log(count());
console.log(count());
console.log(count());
````
**Output**

````JS 
1
2
3
````

One of the reasons that closures are so useful is when dealing with actions taken by the user, such as clicking on a button.  The example below illustrates how closures can be used to provide a variable to other functions that could be run at any time or in any order.  Both functions have a reference **to the same food**.

````JS
function attachClickEvent() {
	var food = 'Burrito';
	var nodeOne = document.getElementById("one");
	nodeOne.onclick = function() {
		console.log(food + ' is a type of food.')
	};
	var nodeTwo = document.getElementById("two");
	nodeTwo.onclick = function() {
		console.log('I really like ' + food + 's');
	};
};
attachClickEvent();
````


## The "this" reference

Remember how we saw that functions created variable scope?  There's more to it, functions not only create scope, but context.  When functions are run they run within a *context* this context is accessable via *.this*  In other programming languages you may call this ".self"


### Window Context

Not all functions are bound to a specifc context however.

Functions that are not explcitly bound to an object run in the *window* context.  This, for all purposes, can be described as the global context, but in a browser, its the window object.  So, when you see your .this object be *window* in your debugger or console, your code is running in the *window* context, which in almost every case, you don't want. (Globals bad)

### Object Context

When the function that is running is a method on an object, the .this keyword is a reference to the instance of that object.  This is incredibly powerful as it allows us to store information on the instance instead of in the global scope.  Dojo relies heavily on this mechanism to create the class like usage of widget.



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

TODO: Clean this up

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




