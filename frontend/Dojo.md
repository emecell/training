# Dojo

## What is Dojo?

A JavaScript Framework like JQuery, etc.

### Why frameworks?

- Abstracts away browser differences
- Common functionality is already written

### Why Dojo Specifically?

- Provides a framework for writing Widgets (classes)
- Implements a form of inheritance in JavaScript
- Provides a mechanism for encapsulating a piece of code and defining its dependencies. 

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

### More on declare()
You may have noticed in the previous example that declare() takes an array.  Dojo provides us with a mechanism for mutliple inheritance.

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


