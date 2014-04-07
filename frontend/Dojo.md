# Dojo

## Why this document?

You'd be right to assume that Dojo has its own extensive documnentation.  However, the project is huge, and it's frankly not worth your time to read through all of it.  We've used Dojo for a while here at Redfin, so this document aims to help you learn what we already have.  Specifically, this document lightly details the parts of Dojo that we like and find useful. 

## What is Dojo?

A JavaScript Framework like JQuery, etc.

### Why frameworks?

- Abstracts away browser differences
- Common functionality is already written

### Why Dojo Specifically?

- Provides a framework for writing Widgets (classes)
- Implements a form of inheritance in JavaScript
- Provides a mechanism for encapsulating a piece of code and defining its dependencies.
- Dojo is one of the most Java like JS frameworks

### AMD Style and Require.js

The Dojo project is in the middle of a pretty major update to the core way the javascript files are included (or required)  This, unfortunately for you, makes it a little tricky to learn how to properly Dojo by just reading existing files that are checked into our codebase.  



## Dojo Modules

Dojo uses AMD syntax to define the requirements for a chunk of code.


### Basic Definition Example

The key concept here is the "define" keyword that is ... well ... defined ... by Dojo and Require.js

#### The 'user' module

***redfin/training/user.js***
````JS
define([], function () {
	var module = {
		getUserName: function () {
			return "John Smith";
		}
	};
	return module;
});
````

- File name starts with a lowercase letter; that's a convention for modules that don't export a class.  More on this later
- Our module name is "redfin/training/user". That's not directly declared anywhere -- it's implicit in the file path.
- We use define(...) to define the module. It takes an array of module dependencies, and a callback function invoked when those dependencies are satisfied. 
- Whatever is returned from the module is the publicly-visible "API" wihcih will typicallybe one of three things:
	- A constructor function (the result of dojo/_base/declare -- coming up soon)
	- An object literal with one or more functions on it (seen here)
	- A single function (we'll talk about dojo/on eventually)


#### Using the 'user' module

Let's use our 'user' module in a basic example here.

***redfin/training/myModule.js***
````JS
define([
	"redfin/training/user"
], function (
	user
) {
	var module = {
		sayHi: function () {
			alert('Hello ' + user.getUserName());
		}
	};
	return module;
});
````

***Usage***
````JS
require(["redfin/training/myModule"], function (myModule) {
	myModule.sayHi();
});
````

***Output***
````JS
Hello John Smith
````

- Filename still starts with a lowercase letter
- We have a dependency now!  Module is requiring "redfin/training/user"
	- When we declare a module as a dependency in this way, we need to include an entry for it in the module callback.
	- Convention: generally the same as the last entry in the module path (i.e., the filename) redfin/training/user --> user
	- There are some exceptions, e.g.: dojo/dom-class --> domClass
- 'user' in the callback function contains the object we returned in the body of redfin/training/user.js:


#### The require() syntax

You may have noticed the "require" syntax in the usage.
- require() is sort of like define()
- Think of define() as doing a combination of require() and "some stuff that defines a module"
- Require is generally used at page load to ensure dependencies are ready before code starts executing
- You'll almost always be using define()



## Writing A Class

A class is very similar to a module.
- Write a module like before, but declare() the class
- File name: "MyClass.js"; not "myClass.js", "my_class.js"
	- myFoo.js is correct for "static" classes, or collections of functions
	- my_foo.js is never the right call.  (╯°□°）╯︵ ┻━┻

### Basic Class Example

***redfin/training/Greeting.js***
````JS
define([
	"dojo/_base/declare"
], function (
	declare
){
	return declare("redfin.training.Greeting", [], {
		constructor: function () {
			console.log("Im a Greeting!");
		},
		say: function () {
			return "Hello";
		}
	});
});
````

***Usage***
````JS
require(["redfin/training/Greeting"], function (Greeting) {
	var g = new Greeting();
	console.log(g.say());
});
````

***Output***
````JS
Im a Greeting
Hello
````

- Filename is capitalized now, since we're defining a class
- The call to declare() looks confusingly similar to define()
- declare() takes three parameters:
	- the class path + name
	- an array of superclasses/mixins
	- an object literal containing the functions and properties you want on your class
	- The class name is technically optional, except for on widgets (which we'll get to later). It seems silly to have some classes be named and some not though, so let's just go all the way. Not specifying the class name will also break the build ;)
- You can optionally define a constructor for your class.
- It behaves similarly to constructors in Java
	- It's called first, as part of the class instantiation
	- It's optional; you don't have to define it
	- Superclass constructors are called automatically during instantiation
- Note that we don't specify the fully-qualified class name when instantiating - it's all inferred from the file (module) path. 



## Extending A Class

Lets extend Greeting to SpanishGreeting

***redfin/training/SpanishGreeting.js***
````JS
define([
	"dojo/_base/declare",
	"redfin/training/Greeting"
], function (
	declare, Greeting
) {
	return declare("redfin.training.SpanishGreeting", [Greeting], {
		constructor: function () {
			console.log("Im a SpanishGreeting!");
		},
		say: function () {
			return "In Spanish, we say Hola! instead of " + this.inherited(arguments);
		}
	});
});
````
***Usage***
````JS
require(["redfin/training/SpanishGreeting"], function (Greeting) {
	var g = new Greeting();
	console.log(g.say());
});
````

***Output***
````JS
Im a SpanishGreeting!
Im a Greeting
In Spanish, we say Hola! instead of Hello
````

- To extend Greeting, we add it as a dependency of our module, and then pass it into our superclass array
- We override the say method, and call this.inherited(arguments) to call the superclass version
- Note: that we don't call this.inherited(arguments) in the constructor, but the superclass constructor still gets called. 
- Note: the order of the log statements in the output, this is important.
- Note: this.inherited is very similar to super.foo() in Java

### this.inherited()
- inherited() is a Dojo thing, not a JavaScript thing
- this.inherited(arguments) passes the method arguments up to the superclass method
- if you want to modify the arguments, pass an additional array: this.inherited(arguments, [newArg1, newArg2, ...]); The new array will be passed to the superclass method instead of arguments

### Mutliple Inheritance
A bit more on declare() You may have noticed in the previous example that declare() takes an array.  Dojo provides us with a mechanism for mutliple inheritance.

***redfin/training/ClassC.js***
````JS

define([
	"redfin/training/ClassA",
	"redfin/training/ClassB"
], function (
	ClassA, ClassB
) {
	return declare("redfin.training.ClassC", [ClassA, ClassB], {
		// Class C goes here
	});
});
````

- The first thing in the array is considered the superclass; everything after it is a mixin.
- The line between a superclass and a mixin is blurry.  Basically determined by whether or not the entry is first in the array
- Mixin properties are copied onto an object in the prototype chain at declare time, superclasses are direct references
- Gory details: If you modify a mixin at runtime, classes inheriting from it won't be updated. If you modify a superclass's prototype at runtime, inheriting classes will see the changes. (But you won't be doing either of those things, so don't worry about it)

#### How are calls dispatched

***Usage***
````JS
require(["redfin/training/ClassC"], function (ClassC) {
	var c = new ClassC();
	c.say();
});
````

- The say function is looked for on ClassC first
- If not found, dojo works its way through the inheritance chain from right to left and recurses
- In this example, this is what happens:
	- Look at C for 'say' function
	- Look at B - Recursively look at inheritance chain of B, working from right to left
	- Look at A - Recursively look at inheritance chain of A, working from right to left
- This is a good mental model to have, but it breaks down in the case where two or more the mixins (or the superclass) share a common mixin. In that case, you should go read about the algorithm they use to determine where in the hierarchy the method gets executed here:  http://dojotoolkit.org/reference-guide/1.8/dojo/_base/declare.html#multiple-inheritance
- Honestly, it gets a lot harder to think about if you have two classes/mixins in your inheritance chain that both inherit from the same class/mixin. For now, suffice it to say that dojo ensures that only one copy of the duplicated class's method is called

#### declare() Gotchas

***redfin/training/ClassD.js***
````Js
define([], function () {
	return declare("redfin.training.ClassD", [], {
		aList: [],
		aMap: {},
		someFunction: function () {
			// ...
		},
	});
});
````
- As a consequence of JavaScript's prototypal nature, aList and aMap will be shared with all instances of ClassD.
- This will bite you eventually. You have been warned!
- The array and object literals are evaluated when the callback function is invoked, not when the class is instantiated. That's an important difference from Java
- If you want to have a separate aList and aMap for each class you should instantiate them in the constructor:

***redfin/training/ClassD.js***
````Js
define([], function () {
	return declare("redfin.training.ClassD", [], {
		aList: null,
		aMap: null,
		constructor: function () {
			this.aList = [];
			this.aMap = {};
		}
	});
});
````


## Useful Dojo API Functionality

### dojo/_base/lang

#### lang.mixin()

***Example***
````JS
require(["dojo/_base/lang"], function (lang) {

	var obj = {
		x: 1,
		y: 2
	};

	lang.mixin(obj, {
		y: 20,
		z: 30
	});

	console.log(obj);

});
````

***Output***
````JS
{ x: 1, y: 20, z: 30}
````

- lang.mixin() copies properties from one object to another, overwriting existing properties

#### lang.hitch()

***Example***
````JS
require(["dojo/_base/lang"], function (lang) {

	var objA = {
		foo: 'bar'
	};

	var objB = {
		foo: 'baz'
	};

	var printFoo = function () {
		console.log(this.foo);
	};

	var printFooA = lang.hitch(objA, printFoo);
	var printFooB = lang.hitch(objB, printFoo);

	printFoo();
	printFooA();
	printFooB();
});
````
***Output***
````JS
undefined
bar
baz
````

- lang.hitch() returns a function that will always be executed in the context of the given object. It provides a cross-browser compatible version of Function.bind() that was introduced in the ECMAScript 5 standard.
- This example is somewhat contrived, but it should serve to illustrate the point. 

### dojo/_base/array

Basic array utilities.  Full list: http://dojotoolkit.org/reference-guide/1.8/dojo/_base/array.html.  These array functions were all standardized as part of ECMAScript 5. Not all browsers support the standard yet though, so we use Dojo's implementations instead of the browser built-ins.

The parameters to the callback functions can be found in the API docs. Generally they are: element, index, array

See also: 
- indexOf
- lastIndexOf
- some
- every



#### array.map()

Create new array by applying the function to every element in the input

***Example***
````JS
require(["dojo/_base/array"], function (array) {
	var myArr = [1, 2, 3, 4];
	var doubledArr = array.map(myArr, function (elt, idx) {
		return elt * 2;
	}); 
	console.log(doubledArr);
});
````

***Output***
````JS
[2, 4, 6, 8]
````

#### array.forEach()

Loop over every element in the array

***Example***
````JS
require(["dojo/_base/array"], function (array) {
	var myArr = [1, 2, 3, 4];
	array.forEach(myArr, function (elt, idx) {
		console.log("Value at index " + idx + " is " + elt);
	}, this);
});
````

***Output***
````JS
Value at index 0 is 1
Value at index 1 is 2
Value at index 2 is 3
Value at index 3 is 4
````

- This is very useful
- Mostly for cross browser support, as older browsers don't support native .forEach on array instances
- Note: the ', this' at the end of the function binds the function to the 'this' scope, and is optional


#### array.filter()

Return a new array w/ elements matching the predicate

***Example***
````JS
require(["dojo/_base/array"], function (array) {
	var myArr = [1, 2, 3, 4];
	var eves = array.filter(myArr, function (elt) {
		return elt % 2 === 0;
	});
});
````

***Output***
````JS
[2, 4]
````

### dojo/dom

Full list: http://dojotoolkit.org/reference-guide/1.8/dojo/dom.html
See also:
- dojo/dom-style
- dojo/dom-attr
- dojo/dom-geometry

#### dom.byId()

Finds the first node in the dom that has the matching id

***Example***
````JS
require(["dojo/dom"], function (dom) {
	var node = dom.byId('myNode'); 
});
````

#### dom.isDescendant()

Returns a boolean if one node is a child of the other

***Example***
````JS
require(["dojo/dom"], function (dom) {
	dom.isDescendant(dom.byId('myChildNode'), dom.byId('myNode'))
});
````


### dojo/dom-class

Full list: http://dojotoolkit.org/reference-guide/1.8/dojo/dom-class.html

#### domClass.add()

Adds a class to a specified node
add(node, className)

***Example***
````JS
require(["dojo/dom-class"], function (domClass) {
	domClass.add(node, 'hidden');
});
````

#### domClass.remove()

Removes a class from a specified code
remove(node, className)

***Example***
````JS
require(["dojo/dom-class"], function (domClass) {
	domClass.remove(node, 'hidden');
});
````

#### domClass.replace()

Replaces a class (or classes on a specified node)
replace(node, toAdd, toRemove)

***Example***
````JS
require(["dojo/dom-class"], function (domClass) {
	domClass.replace(node, 'someOtherClass', 'someClass');
	domClass.replace(node, ['three', 'four'], ['one', 'two']);
});
````

#### domClass.toggle()

Adds or removes a class on a node based on a boolean conditional
toggle(node, className, condition)

***Example***
````JS
require(["dojo/dom-class"], function (domClass) {
	domClass.toggle(node, 'hidden', true);
	domClass.toggle(node, 'hidden', false);
});
````

### dojo/Deferred


## Dojo Widgets

### Dojo vs. Dijit

### What is a Widget?

### Examples

### dojo/domReady

### Widget Lifecycle

### Templated Widgets

### Events




## Redfin API 

### redfin/request/xhr

redfin/request/xhr is a wrapper around dojo/request/xhr that sets defaults (method: GET, handleAs: json) and adds a CSRF token to protect our form posts. 

Further info: http://dojotoolkit.org/reference-guide/1.8/dojo/request.html


