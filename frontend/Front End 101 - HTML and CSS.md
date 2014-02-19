# Front End 101: HTML & CSS


## HTML overview

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
	div:	block html element. contains attributes:
		id=: 	once per page. use sparingly.
		class=:	this is your swiss army knife. can reference multiple classes.
		style=:	defines an inline style. DO NOT USE.

	inline html elements:
		<span class="highlight">inline styling</span>
		<em>em</em>, not <i>italic</i>.
		<strong>strong</strong>, not <b>bold</b>.
</div>
````

## CSS overview

Cascading Style Sheets are a standard that defines how browsers should display an element on a page.

### Why use CSS?

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

### What does CSS entail?

1. **selectors** can be chained together, to create more complex conditions or to build a new visual style

	| example			 | selector description
	| ------------------ | --------------------
	| `E`				 | type selector (`E` can be any type)
	| `.myClass`		 | class selector
	| `#id`			 	 | id selector
	| `E[foo=bar]`		 | attribute selector
	| `E:first-child`	 | pseudo-class selector
	| `E:before`		 | pseudo-element selector
	| `E > F`, `E + E`	 | child & sibling selectors

2. the **cascade** is how a browser determines which style "wins" if multiple styles are competing.
	* For a given element and property:
		1. Find all matching selectors
		2. Sort according to importance and origin
			* importance = whether or not property has !important
			* origin:
				* user-agent (browser default styles)
				* user (user overrides of browser defaults)
				* author (website)
		3. Sort rules with same importance and origin by specificity
		4. Finally, sort by order specified. Last rule specified wins
3. specificity
4. inheritance
5. layout
	* more detail on block vs inline
	* float vs inline-block
	* table-cell
6. resources
	* [caniuse.com][cani] - a series of compatibility tables for HTML5, CSS3, and other features.


## LESS is moar

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

1. [Codecademy HTML & CSS track][codec]
	* great series of starting tutorials if you're very new to HTML & CSS
2. [MDN Intro to HTML][mdnintro]
3. [Eric Meyer][meyer]
 	* an authority on CSS. Articles, tools, & books.

<!-- LINKS -->

 [cani]: http://caniuse.com/
 [mdnintro]: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Introduction
 [meyer]: http://meyerweb.com/eric/css/
 [codec]: http://www.codecademy.com/tracks/web