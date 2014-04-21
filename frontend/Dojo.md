# Dojo

## Why this document?

You'd be right to assume that Dojo has its own extensive documnentation.  However, the project is huge, and it's frankly not worth your time to read through all of it.  We've used Dojo for a while here at Redfin, so this document aims to help you learn what we already have.  Specifically, this document lightly details the parts of Dojo that we like and find useful. 

## What is Dojo?

A JavaScript framework like AngularJS, Backbone.js, or Ember.js.

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

Changes the context that a function runs in.

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

dojo/Deferred is a way to return a "promise" that a value will be computed and made available to the caller later
Instead of accessing the value directly, you give it a callback that will be executed when the value is available. 

API Docs: http://dojotoolkit.org/reference-guide/1.8/dojo/Deferred.html

Good tutorial: http://dojotoolkit.org/documentation/tutorials/1.8/deferreds/

Effective JavaScript: Item #68

***redfin/training/DeferredExample.js***
````JS
require([
	"dojo/Deferred"
], function (Deferred) {
	var dfd = new Deferred();
	dfd.then(function (value) {
		console.log("The value I received is: " + value);
	});
	dfd.resolve(3);
});
````

***Output***
````JS
The value I received is 3
````


***redfin/training/DeferredExample2.js***
````JS
require([
	"dojo/Deferred"
], function (Deferred) {
	var dfd = new Deferred();
	dfd.then(
		function (value) {
			console.log("Success: " + value);
		},
		function (value) {
			console.log("Error: " + value);
		}
	);
	dfd.reject("This is an error!");
});

````

***Output***
````JS
This is an error!
````



## Dojo Widgets


### What is Dijit?

- A part of Dojo
- A framework for creating UI widgets

### What is a Widget?

Any reusable UI element.  We have thousands, ranging from a simple display of a item in a list to whole pages.  Where Carl Sagan said "We are made of star stuff" our web interface is made of widget stuff.  Lots of widget stuff.  A loose definition of a widget is a tight paring of a dom fragment (a chunk of html) and a Dojo class

Why widgets?
- Modular: you can easily write code that can be reused
- Templating happens in Javascript on the client side
	- No compile step for small UI changes
	- Instant, dynamic updates to the UI with no server round trip
	- Easy to inspect and debug in the browser
- It's not JSP (which we used to use)


### Widget Basics

Lets start with an example

***redfin/training/WidgetExample.js***
````JS
define([
	"dojo/_base/declare",
	"dijit/_WidgetBase"
], function (
	declare,
	_WidgetBase
) {
	return declare("redfin.training.WidgetExample", [_WidgetBase], {
		postCreate: function () {
			this.inherited(arguments);
			this.domNode.innerHTML = 'Hello World!';
		}
	});
});
````

***Usage***
````HTML
<html>
	<script type="text/javascript">
		require([
			"dojo/dom", 
			"redfin/training/WidgetExample",
			"dojo/domReady!"
		], function (
			dom,
			WidgetExample
		) {
			new WidgetExample({}, dom.byId('widgetNode'))
		});
	</script>
	<body>
		<div id="widgetNode"></div>
	</body>
</html>
````

***Output***
````JS
Hello World
````

#### dijit/_WidgetBase

- The base class for all Dijits; defines the widget lifecycle.  More on this soon.
- Old version: dijit/_Widget (lots of code still extends this, but don't use it)
- Also provides generic get()/set() property accessors, and the ability to write custom accessors for individual properties
- Every widget takes two arguments
	1. An argument object containing all the parameters for the widget that will be "mixed in" to the widget as part of its instantiation. 
	2. An optional node reference.


More info: http://dojotoolkit.org/documentation/tutorials/1.8/understanding_widgetbase/

##### _WidgetBase.own

- Manages storing references to event handles, more on that later, but remember this
- If you subscribe to a bunch of events, you have to remember to tear them down when the widget is destroyed, or else you'll end up with a memory leak.
- calling this.own(...) will register the handles returned from the event connection and will call remove() when the widget is destroyed


### Templated Widgets

- Creates tempalted dom from a stored snippet

#### Basic Example

***redfin/training/TemplatedWidget.js***
````JS
define([
	"dojo/_base/declare",
	"dijit/_WidgetBase",
	"dijit/_TemplatedMixin",
], function (
	declare,
	_WidgetBase,
	_TemplatedMixin,
) {
	return declare("redfin.training.TemplatedWidget", [_WidgetBase, _TemplatedMixin], {
		templateString: '<div class="TemplatedWidget">Hello World!</div>'
	});

});
````

***Usage***
````HTML
<html>
	<script type="text/javascript">
		require([
			"dojo/dom", 
			"redfin/training/TemplatedWidget",
			"dojo/domReady!"
		], function (
			dom, 
			TemplatedWidget
		) {
			new TemplatedWidget({}, dom.byId('widgetNode'))
		});
	</script>
	<body>
		<div id="widgetNode"></div>
	</body>
</html>
````

- The template for the widget will replace the node that its instanciated at. The node where it was where it was create will no longer exist.



#### dojo/text!

Dojo text loads a files contents into memory as a string.  We can use this functionality to pull the template HTML out the the javascript file and into an external HTML file.  You should almost always do this.  Don't write HTML in JS files!


***redfin/training/TextTemplatedWidget.js***
````JS
define([
	"dojo/_base/declare",
	"dijit/_WidgetBase",
	"dijit/_TemplatedMixin",
	"dojo/text!./templates/TemplatedWidget.html"
], function (
	declare,
	_WidgetBase,
	_TemplatedMixin,
	template
) {
	return declare("redfin.training.TextTemplatedWidget", [_WidgetBase, _TemplatedMixin], {
		templateString: template
	});

});
````

***redfin/training/templatesTextTemplatedWidget.html***
````HTML
	<div class="TextTemplatedWidget">Hello World!</div>
````

- This does effectively the same thing as the previous example, but the HTML is in an external file!
- We always put template files 'templates' folder that is a sibling to the widget class file
- Example:
	- .../widget/MyWidget.js
	- .../widget/templates/MyWidget.html
- Always put the name of the Widget Class as a CSS class on the root template node! ALWAYS! 
	- Easier to scope styles to a specific widget
	- Easier to quickly figure out which widget is generating a certain block of HTML when you're debugging
	- Widget names should be PascalCase, not camelCase



#### Advanced Template Functionality

Notes in templates can have certain annotations that templated uses.

##### data-dojo-attach-point 
Nodes that have a declared attach point have a reference as the attch point name to them copied to the class instance.  This is extremely useful, we use it every day.

##### data-dojo-type
This allows you to instanciate new widgets directly from the html template.  Do not use this!  TODO: explain why

##### data-dojo-attach-event
Allows you to register event handlers directly drom the html template.  Do not use this either! TODO: explain why

##### Variable insertion
You can insert strings into the template with ${} syntax.  Do not use this either either! TODO: explain why



### Getters and Setters

Since javascript doesn't have a way to specify getters and setters Dojo has created a way to do it.  
- Explicit getters and setters are not required for every attribute, but its highly recommended to write them out as it makes your class much easier to understand
- Getters and setters are generated by specifying functions that have a specifically formatted name.  

***redfin/training/GettersAndSetters.js***
````JS
define([
	"dojo/_base/declare",
	"dijit/_WidgetBase"
], function (
	declare,
	_WidgetBase
) {
	return declare("redfin.training.GettersAndSetters", [_WidgetBase], {

		_setValueAttr: function(value) {
			this._set('value', value);
			console.log('setting attribute "value" to ', this.value)
		},

		_getValueAttr: function() {
			return this.value;
		}
	});

});
````

***Usage***
````HTML
<html>
	<script type="text/javascript">
		require([
			"dojo/dom", 
			"redfin/training/GettersAndSetters",
			"dojo/domReady!"
		], function (
			dom, 
			GettersAndSetters
		) {
			var gettersAndSettersExample =  new GettersAndSetters({}, dom.byId('widgetNode'));
			gettersAndSettersExample.set('value', 2);
			console.log('the value is ', gettersAndSettersExample.get('value'));
		});
	</script>
	<body>
		<div id="widgetNode"></div>
	</body>
</html>
````

***Output***
````JS
setting attribute "value" to 2
the value is 2
````

- Setters are created by functions that have their name as such "_setXXXXattr"
- Getters are created by functions that have their name as such "_getXXXXattr"
- In setters Dijit copies the value to the instance in the private this._set function, you should always remember to implement the private setter in the public method
- The this._set function also fires change events for that attribute
- Do not set properties on the instance directly without a setter



### Dijit Lifecycle

There are various steps in the instanciation of a dijit.  Each has a special purpose.  

1. constructor()
2. postMixInProperties()
3. buildRendering()
4. Setter Execution 
5. postCreate()
6. startup()

***redfin/training/WidgetLifecycle.js***
````JS
define([
	"dojo/_base/declare",
	"dijit/_WidgetBase",
	"dijit/_TemplatedMixin"
], function (
	declare,
	_WidgetBase,
	_TemplatedMixin
) {
	return declare("redfin.training.WidgetLifecycle", [_WidgetBase, _TemplatedMixin], {
		templateString: "<div class='WidgetLifecycle' data-dojo-attach-point='containerNode'></div>",

		constructor: function() {
			console.log('constructor')
		},

		postMixInProperties: function() {
			this.inherited(arguments);
			console.log('postMixInProperties')
		},

		buildRendering: function() {
			this.inherited(arguments);
			console.log('buildRendering')
		},

		postCreate: function() {
			this.inherited(arguments);
			console.log('postCreate')
		},

		startup: function() {
			this.inherited(arguments);
			console.log('startup')
		},

		_setValueAttr: function(value) {
			this._set('value', value)
			console.log('setting attribute "value" to ', this.value)
		}
	});

});
````

***Usage***
````HTML
<html>
	<script type="text/javascript">
		require([
			"dojo/dom", 
			"redfin/training/WidgetLifecycle",
			"dojo/domReady!"
		], function (
			dom, 
			WidgetLifecycle
		) {
			var widgetLifecycleExample =  new WidgetLifecycle({
				value: 4
			}, dom.byId('widgetNode'));
			widgetLifecycleExample.startup();
		});
	</script>
	<body>
		<div id="widgetNode"></div>
	</body>
</html>
````

***Output***
````JS
constructor
postMixInProperties
buildRendering
setting attribute "value" to 4
postCreate
startup
````

#### constructor()

- Most widgets won't implement a constructor explicitly 
- Byproduct of declare(); actually not Dijit-specific
- Raw arguments to the class constructor are available here on the attributes object, but its recommended not to modify them here
- No widget DOM setup has been done yet for templated widgets
- Create new arrays and objects here


#### postMixInProperties()

- Most widgets won't implement this explicitly 
- Called after the arguments object has been mixed in to the widget; at this point, any arguments passed in are available on the instance.  However its perferred to only access properties through their setters and not here.
- No DOM setup work has been done at this point - the this.domNode reference isn't available


#### buildRendering()

- If you're instantiating nodes programmatically, or manipulating the dom in any way as part of widget setup, it should be done in this method!
- After this method returns, your widget will be added to the DOM; the cost of manipulating nodes is increased since the browser may have to redraw
- Don't forget to call this.inherited(arguments) - otherwise the widget won't work, and it won't be obvious why the widget's domNode is available after inherited() is called


#### Setter Execution 

- Setters execute in a certain order
- Any properties that were specified on instanciation are run.
- Falsy setters don't run, this seems like a dojo bug

#### postCreate()

- Don't forget to call this.inherited(arguments)
- PostCreate is called after the widget and its children are created, but before it is visible to the user*
- Widget customization that doesn't involve modifying the dom should occur in this method
- PostCreate is called after custom setters have run


#### startup()

- Don't forget to call this.inherited(arguments)
- Startup is called automatically if your widgets are being instantiated via a template (which we'll see in a minute)
- If you programmatically instantiate a widget, don't forget to call startup. It will appear to work, and probably will, until somebody uses a dijit that expects to have startup called on it, and then it's a mess to track down.
- startup() automaticaly calls startup() on the child nodes of a templated widget that has a "containerNode" node reference.  You can see this attach point in the example above.  Thats why its there.



### Events

#### dojo/on

TODO: Basic Example

- Dojo referres to dojo/on as their "event normalization api"
- Use dojo/on to connect to DOM events: click, keypress, keydown, etc.
- The callback is executed in the context of the caller, so in most cases you'll want to use lang.hitch callback executed in context of class
- dojo/on returns a handle with a remove() function that cleans up the connection
	- Always clean up!
	- For widgets, the destroy() function is usually the place to put it. We'll see in a minute how to handle this automatically
- Use for an abstraction of regular browser events, not synthetic events.  We (Redfin) use this in very odd ways, so be careful what you use for example in the code base.


TODO: Widget Example

- _WidgetBase defines an on() method that can be used similarly to dojo/on
- dojo/on delegates to a class's on() method if it exists, so: on(this.myButton, 'blah', ...) will call through to: this.myButton.on('blah', ...)
- Backwards compatibility notes
	- pre-Dojo 1.8, you exposed callbacks by having empty methods like onBlah() and other widgets connected to them When your widget called onBlah(), their callbacks were invoked
	- For now, you can connect to those methods with on('blah', ...), but that won't last forever. That functionality is going away in dojo 2.0. So we should work on converting those kinds of connections when you see them in code.



#### dojo/aspect

TODO: Aspect example

- Use dojo/aspect to connect to methods
- after() arguments: 
	- watched instance
	- method name to watch
	- callback
	- optional boolean
- argument to callback defaults to return value of watched method
- if you return a value from your after() callback, it will replace the original return value of the watched 
- fourth argument to after() determines whether the callback receives the return value or the arguments to the watched function (default is return value)
- dojo/aspect also provides before() and around(), but usage is less common
- Dirty little secret: these methods will replace the original method -- that's how they implement the before/after/around behavior. If you have a reference to the original method hanging around somewhere, you might get odd behavior. It hasn't bit us yet though...



#### dojo/topic

TODO: Topic Example

- dojo/topic implements a publish/subscribe event system 
- there are some cases where publish/subscribe is the right call, but it effectively makes a widget dependent on a global piece of information (the topic), which can hinder reuse if not used/documented wisely



#### dojo/Evented

TODO: Evented Example

dojo/Evented is designed to provide a class that allows a developer to emit events and provide an easy way to allow those events to be connected to by downstream users. It leverages the API concepts of dojo/on.
- used for what are commonly referred to as "sythetic" events, which are different than DOM events, which dojo/on normalises.
- Useful for firing and listening to data changes or non user based actions




## Redfin API 

### redfin/request/xhr

redfin/request/xhr is a wrapper around dojo/request/xhr that sets defaults (method: GET, handleAs: json) and adds a CSRF token to protect our form posts. 

You should always use our XHR and not Dojos

- Common Calls
	- xhr.get(url, options) - makes a GET request
	- xhr.post(url, options) - makes a POST request
- Common Options
	- handleAs - will nearly always be 'json' (this is optional; it defaults to 'json')
	- data - used to send data to the server in a POST request
	- query - used to send data to the server in the URL of a GET or POST request
- Note: data and query will probably not often be used together, at least in our code
- get() and post() return deferred promises


Further info: http://dojotoolkit.org/reference-guide/1.8/dojo/request.html

#### xhr.post()

***redfin/training/XhrPostClass.js***
````JS
define([
	"dojo/_base/declare",
	"dojo/_base/lang",
	"redfin/request/xhr"
], function (
	declare,
	lang,
	xhr
) {
	return declare("redfin.training.XhrPostClass", [], {

		doRequest: function () {
			xhr.post(g_secureWebServerUrl + '/tools/dev/api/bindEnumExample', { 
				data: { 
					name: "John Smith",
					number: 123
				}
			}).then(
				lang.hitch(this, this._handleSuccess),
				lang.hitch(this, this._handleError)
			);
		},

		_handleSuccess: function (response) { console.log(response); },
		_handleError: function (response) { console.log(response); }

	});
});
````
- Makes a HTTP POST request to https://www.redfin.com/some/other/url
- Passes POST parameters in the body of the request: 
	- name = "John Smith"
	- number = 123
- Parses the response as JSON


***redfin/training/XhrPostClassWithQuery.js***
````JS
define([
	"dojo/_base/declare",
	"dojo/_base/lang",
	"redfin/request/xhr"
], function (
	declare,
	lang,
	xhr
) {
	return declare("redfin.training.XhrPostClassWithQuery", [], {

		doRequest: function () {
			xhr.post(g_secureWebServerUrl + '/tools/dev/api/bindEnumExample', { 
				data: { 
					name: "John Smith"
				},
				query: { id: 3 }
			}).then(
				lang.hitch(this, this._handleSuccess),
				lang.hitch(this, this._handleError)
			);
		},

		_handleSuccess: function (response) { console.log(response); },
		_handleError: function (response) { console.log(response); }

	});
});
````
- Makes a HTTP POST request to https://www.redfin.com/a/final/url?id=3
- Passes POST parameters in the body of the request: 
	- name = "John Smith"
- Parses the response as JSON


#### xhr.get()

***redfin/training/XhrGetClassWithQuery.js***
````JS
define([
	"dojo/_base/declare",
	"dojo/_base/lang",
	"redfin/request/xhr"
], function (
	declare,
	lang,
	xhr
) {
	return declare("redfin.training.XhrPostClassWithQuery", [], {

		doRequest: function () {
			xhr.get(g_secureWebServerUrl + '/tools/dev/api/bindEnumExample', { 
				query: { 
					parameterOne: "Hello World",
					parameterTwo: 123
				}
			}).then(
				lang.hitch(this, this._handleSuccess),
				lang.hitch(this, this._handleError)
			);
		},

		_handleSuccess: function (response) { console.log(response); },
		_handleError: function (response) { console.log(response); }

	});
});
````

- Makes a HTTP GET request to https://www.redfin.com/some/url?parameterOne=Hello%20World&parameterTwo=123
- Parses the response as JSON

#### Handling Responses

- The response object is Redfin-defined, it essentially corresponds to ApiResult.java
- ResultCode is defined by ApiResult.Code in Java
- errorMessage will be an exception message in the event of an uncaught error, or a friendly-ish message in the event of a validation error. 
- Generally, if resultCode === g_apiResultCode.NO_ERROR, the errorMessage string is ignored payload is determined by the API endpoint being called, but if data is being returned from the server it will generally be in that object. In simple responses, this might be null. 
- the second callback is the error callback (which may be obvious)
- for dojo/request, its role is slightly confusing
	- it's called if there's some sort of JS error in the success handler (e.g. referencing an undefined variable)
	- it's called if the server returns a non-200 response (i.e., unexpected server error)
	- whether or not it's called has nothing to do with the *Redfin result code* from the server
	- Note: we need an error callback because async callbacks can't throw exceptions in the current context -- there's no appropriate place for a try/catch

### redfin/common/aspected

TODO


### UI Elements

We have many templated UI widgets at your disposal.  Not only do they make life easier for you as a dev, they help the design team maintain graphic standards and ensure a uniform user experience.  Check the style guide for the full list, but here are some:

- Flyout
- Dialog
- Form Elements
- Button

You should not be coding your own variants of these widgets nor using native browser controls


## Redfin Best Practices

- Don't write HTML in JS files
- Use query as little as possible
- Pascal case class names
- Don't use dojo create to create dom, use tempalated! 

