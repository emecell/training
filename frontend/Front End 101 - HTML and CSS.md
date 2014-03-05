# HTML overview

1. tags
	* basic css: block vs inline
2. html5 syntax
	* valid code (nested properly, no weird attributes)
	* void elements are self-contained, must not have contents, and can be marked up as 'self-closing' tags with an end `/` to better illustrate this.
		For example, this image tag is self-closing: `<img src="foo" ... />`.
3. attributes / data attrs
4. semantic HTML
	* use the right thing in the right place
	* nesting of block vs inline elements

````html
<div id="unique" class="reusable multiple" style="">
	div			block html element. contains attributes:
		id		once per page. use sparingly.
		class	this is your swiss army knife. can reference multiple classes.
		style	defines an inline style. DO NOT USE.

	inline html elements:
		<span class="highlight">inline styling</span>
		<em>em</em>, not <i>italic</i>.
		<strong>strong</strong>, not <b>bold</b>.
</div>
````

# CSS overview

Cascading Style Sheets are a standard that defines how browsers should display an element on a page.

## Why use CSS?

* We can separate structure and content from presentation.
	* We can theme widgets, and leave the HTML alone, but change the way they're displayed.
* CSS allows us to name and reuse frequently-used groups of styles. These can include the following attributes (and more):
	* text color
	* font attributes (face, size, weight, style)
	* width
	* height
	* height of lines
	* background color, transparency, repetition, image
	* etc.
* What version of the spec is supported?
	* the vast majority of CSS2 is available in all browsers
	* most of CSS3 is available in the browsers we support (use for progressive enhancement)

## What does CSS entail?

### Anatomy of a CSS rule

````css
/* CSS Rule: */			/* E & .classname are "simple selectors" */
E .classname {			/* Selector {} */
	/* Declaration */
	color: #000000;		/* Property: Value; */
	
	/* Declaration */
	font-weight: bold	/* Property: Value; */
}
````


### Basic selectors

Selectors are a way of describing a node (or nodes) in the DOM tree. There are several levels of specificity, which we will dig into later.

| example			   | Usefulness | selector description
| -------------------- | ---------- | ------------------------------------
| `E`				   | medium	  	| type selector (`E` can be any type)
| `.myClass`		   | **XTREME** | class selector
| `#id`			 	   | nil 		| id selector
| `E[foo=bar]`		   | low		| attribute selector
| `E:first-child`, etc | medium	  	| pseudo-class selector
| `E:before`		   | low		| pseudo-element selector
| `E > F`, `E + E`	   | medium	  	| child & sibling selectors

* type selector (`E` can be any type)
* class selector
	* the most useful selector, classes can be applied to any element in the DOM.
	* reusable
* id selector
	* going back to basic HTML, these **must** be unique per page.
	* should be used infrequently, approaching never.
	* targets a specific element.
* attribute selector
* pseudo-class selector
* pseudo-element selector
* child & sibling selectors

#### Chaining selectors

Selectors can be **chained** to create more specific style combinations.

````css
.agent {
	font-weight: bold;
}
.agent.lead {
	color: @redfin-red; /* this is a global color variable */
}
````
````html
<div class="agent">
	<h3>
		<span class="agent lead">John Redfin</span>
	</h3>
</div>
````

#### Descendant selectors

Sometimes, we want to target only the children of a certain selector or DOM node. Descendant selectors use ` ` whitespace to define that relationship.

````css
.agent {
	font-weight: bold;
}
.agent .lead {
	color: @redfin-red;
}
````
````html
<div class="agent">
	<h3>
		<span class="lead">John Redfin</span>
	</h3>
</div>
````

#### Child selectors

Also known as "direct descendant", child selectors target *only* the immediate children of a selector or DOM node. These use `>` a 'greater-than' sign to define that relationship.

````css
.agent {
	font-weight: bold;
}
.agent > .lead {
	color: @redfin-red;
}
````
````html
<div class="agent">
	<h3 class="lead">
		<span>John Redfin</span>
	</h3>
</div>
````


### Cascade 

The **cascade** is how a browser determines which CSS declaration "wins" if multiple rules are competing.

For a given node (from [MDN][^fn-mdncascade]):

1. Find all matching selectors
2. Sort according to importance and origin

	|   | Origin     | importance 		|
	|---|------------|------------------|
	| 1 | user agent | normal			|
	| 2 | user agent | !important		|
	| 3 | user		 | normal			|
	| 4 | author	 | normal			|
	| 5 | author 	 | !important		|
	| 6 | user		 | !important		|

3. Sort rules with same importance and origin by specificity
4. Finally, sort by order specified. Last rule specified wins


### specificity

### inheritance

### layout
* more detail on block vs inline
* float vs inline-block
* table-cell

### resources
* [caniuse.com][cani] - a series of compatibility tables for HTML5, CSS3, and other features.


# LESS is moar

1. differences from CSS
	* nesting
	* mixins
	* variables & globals
	* functions (sorta)
2. Redfin fork
	* no globals!
	* preamble mixins
3. CAUTION:
	* specificity
	* inheritance
4. make full use of:
	* variables (consistency, etc)
	* basic functions (math, etc)
	* preamble


# Additional reading (optional)

* [HTML5 for Web Designers, by Jeremy Keith][abhtml]
* [CSS3 for Web Designers, by Dan Cederholm][abcss]
* [Codecademy HTML & CSS track][codec]
	* great series of starting tutorials if you're very new to HTML & CSS
* [MDN Intro to HTML][mdnintro]
* [Eric Meyer][meyer]
 	* an authority on CSS. Articles, tools, & books.

<!-- LINKS -->

 [cani]: http://caniuse.com/
 [mdnintro]: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Introduction
 [^fn-mdncascade]: https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade
 [meyer]: http://meyerweb.com/eric/css/
 [codec]: http://www.codecademy.com/tracks/web
 [abcss]: http://www.abookapart.com/products/css3-for-web-designers
 [abhtml]: http://www.abookapart.com/products/html5-for-web-designers