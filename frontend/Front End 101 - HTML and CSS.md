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

Cascading Style Sheets are a [standard](http://www.w3.org/Style/CSS) that defines how browsers should display an element on a page.


## Why use CSS?

The biggest advantage of CSS is the ability to separate structure and content from presentation.Developers can theme widgets, or modify layouts, without touching the HTML. We can also name and reuse frequently-used groups of styles. These can include the following attributes (and more):

* text color
* font attributes (face, size, weight, style, line-height)
* width
* height
* background color, image, and repetition
* transparency
* animations and transitions

Like the rest of the web, CSS standards are something of a work in progress. The vast majority of the CSS2 spec is implemented in all browsers, and the important parts of CSS3 are available in the browsers we support. Many of the more advanced features can be added and will only affect browsers that support them, a form of **progressive enhancement**.


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

**Classes** are by far the most useful selectors, as you can apply them to any element in the DOM. They're also reusable, unlike **ID selectors**, which must be *unique per page*. Realistically, you should use IDs almost *never*, and only when you need to target a specific element. Dojo, our JavaScript library, has ways to select an element without using IDs that you really ought to use instead. **Type** or **element selectors** can be useful, but they are almost *always* combined with other selectors. Only when setting up the basics of a page or a reset-type stylesheet will you use a lot of pure type selectors.

The other useful selectors are **pseudo-classes** and **child and sibling selectors**. Pseudo-classes allow you to target elements based on their position in the DOM. `.foo:first-child`, for example, will find the [first child](https://developer.mozilla.org/en-US/docs/Web/CSS/:first-child) matching `.foo` in a given parent:

````css
.container {
	font-family: sans-serif;
}
.foo {
	color: red;
}
.foo:first-child {
	font-weight: bold;
}
````
````html
<div class="container">
	<span class="foo">one</span>		only this span will be bold
	<span class="foo">two</span>
</div>
````
![http://cl.ly/image/3S1O2s283W1n/Screen%20Shot%202014-03-06%20at%2015.07.23%20.png](http://cl.ly/image/3S1O2s283W1n/Screen%20Shot%202014-03-06%20at%2015.07.23%20.png)

**Child & sibling selectors** can allow you to target certain combinations of elements:

````css
.container {
	font-family: sans-serif;
}
.foo {
	color: red;
}
.bar > .foo {
	font-weight: bold;
}
.foo + .foo {
	font-style: italic;
}
````
````html
<div class="container">
	<span class="foo">one</span>
	<span class="foo">two</span>		this span will be italic
	<span class="foo">three</span>		so will this span
	<div class="bar">
		<span class="foo">four</span>	only this span will be bold
	</div>
</div>
````
![http://cl.ly/image/0S3c241I3M1R/Screen%20Shot%202014-03-06%20at%2015.29.35%20.png](http://cl.ly/image/0S3c241I3M1R/Screen%20Shot%202014-03-06%20at%2015.29.35%20.png)


#### Chaining selectors

You may **chain** selectors to create more specific style combinations.

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

Also known as "direct descendant", child selectors target *only* immediate children of a DOM node. These use `>` a 'greater-than' sign to define that relationship.

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

The **cascade** is how a browser determines which CSS declaration "wins" if multiple rules are competing. Different sources of CSS have varying levels of importance. At the high end is an `!important` override rule in a visitor's user stylesheet, and the low end is a normal rule in the browser's default user agent styles. 

For a given node, the browser will: 

1. Find all matching selectors
2. Sort according to importance and origin

	|   | Origin    			 | importance 		|
	|---|------------------------|------------------|
	| 1 | user agent (browser)	 | normal			|
	| 2 | user agent (browser)	 | !important		|
	| 3 | user (visitor)		 | normal			|
	| 4 | author (website)		 | normal			|
	| 5 | author (website)		 | !important		|
	| 6 | user (visitor)		 | !important		|

3. Sort rules with same importance and origin by specificity
4. Finally, sort by order specified. Last rule specified wins

For a lot more detail, read the [MDN article][mdncascade] on the CSS Cascade.


### Specificity

Go read Smashing Mag's [CSS Specificity: Things You Should Know](http://coding.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/) by Vitaly Friedman. Come back when you're done. If you're just looking for a refresher, check out Andy Clarke's [CSS: Specificity Wars](http://www.stuffandnonsense.co.uk/archives/css_specificity_wars.html), a Star Wars-themed illustration of specificity. 


### Inheritance

**Inheritance** is similar to the cascade, but tied to the DOM structure of the HTML you're styling. In general, rules applied to an HTML element will be *inherited* by that element's children. There are a few exceptions, like borders and backgrounds, which are *non-inherited properties*.

Some of the easiest examples of inheritance are font color, family, and size:

````css
div {
	color: grey;
	font-family: sans-serif;
}
p {
	color: black;
	font-size: 1.5em;
}
em {
	color: red;
	font-family: serif;
}
````
````html
<div>
	<h1>Headline</h1>
	<div>Non-paragraph text, that is inheriting the body's font size.</div>
	<p>Paragraph text at 1.5em, including an emphasized block that will be red. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. <em>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.</em> Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
</div>
````
![http://cl.ly/image/413d3X2y2a41/Screen%20Shot%202014-03-07%20at%2015.21.56%20.png](http://cl.ly/image/413d3X2y2a41/Screen%20Shot%202014-03-07%20at%2015.21.56%20.png)

In the above example, the grey font color and sans-serif font are inherited by the children until overriden by more specific rules. These rules create the black text and larger font in the paragraph, and the red serif emphasized text.

For more reading about inheritance, take a look at the [MDN documentation][mdninheritance].

## Layout

[READ THIS](https://docs.google.com/a/redfin.com/presentation/d/1VGEjuIGqJ1mYu5F8fVmQrNt48Tf_7tPHSnlpbgtDgzs/edit#slide=id.g29518a1bb_05)

<!--

TODO: Reformat the deck!

### There are many ways to do layout

- Inline-Block
- Floats
- Absolute Positioning
- Tables
- Flexbox

### Box Model

- Layoyout Diagram
- Box Sizing

This is very important to understand, you almost always want border-box!


### Origin of HTML
HTML was created a text document format
Browsers calculate “lines” and position most elements according to the line
Line height, floats, and display are all “line level” layout mechanisms
Position is coordinate based
table is … cell based

### Display Property
- Block
- Inline
- Inline-Block
- Table + Table-Cell

#### Block

#### Inline

#### Inline-Block

#### Table + Table-Cell


-->

**UP NEXT**: *[an introduction to LESS CSS](https://github.com/egid/training/blob/master/frontend/Front%20End%20120%20-%20LESS.md)*.



### Resources
* [caniuse.com][cani] - a series of compatibility tables for HTML5, CSS3, and other features.


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
 [mdncascade]: https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade
 [mdninheritance]: https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance

 [meyer]: http://meyerweb.com/eric/css/
 [codec]: http://www.codecademy.com/tracks/web
 
 [abcss]: http://www.abookapart.com/products/css3-for-web-designers
 [abhtml]: http://www.abookapart.com/products/html5-for-web-designers

 [rsg-colors]: https://trunk.redfintest.com/admin/style-guide/stingray/brand-colors