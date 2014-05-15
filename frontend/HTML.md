# HTML: It's so great, the inventor was knighted.

*HyperText Markup Language*, or **HTML**, is internet Lego. It's the basic building blocks used to create a website; it's purely ***markup***, "tags that [...] provide additional information about the text" ([Linux Information Project](http://www.linfo.org/markup_language.html)). With HTML, we can break content and concepts into distinct chunks of markup, and then do things to them later with technologies like *Cascading Style Sheets* and *JavaScript*. 

HTML was first proposed [in 1990](http://www.w3.org/Proposal.html) by Tim Berners-Lee, and was officially published in 1995 as HTML 2.0, which sounded more trustworthy or something. Since then, HTML has gone through a handful of changes – HTML5 is what's currently supported by modern browsers, and what I'll discuss here. Here's an extremely simple [valid HTML5 webpage](http://validator.w3.org/check) (try a [demo](http://jsbin.com/figot/1/edit?html,output)):

````html
<!DOCTYPE html>
<html>
	<head>
		<title>This title shows up in a tab or window</title>
	</head>
	<body>
		<p>This paragraph text will show up in your browser's viewport. Heck yes.</p>
	</body>
</html>
````

You might be wondering what in blazes "valid HTML5" is, and why you should care. Valid markup is going to be correctly understood by a browser, by a search engine bot like Google, or by a screenreader. If you want your content to get parsed correctly, in any context, you ought to be writing valid HTML. Thankfully, HTML is reasonably simple.

## The basics!

If any of this is ***too*** basic, feel free to move on. But it might be worth a refresher anyway. If nothing else, you can correct mistakes, right?

### What the blazes is a tag?

A tag is surrounded by angle brackets, carets, less/greater-than-signs, whatever you want to call them. Alligators. Anyway, they generally *open* and *close*, which looks like this: `<html> foo </html>`. Notice how the second instance of the tag starts with a forward slash? That ends or *closes* the tag.

Tags and their contents are referred to as an *element*. Here's a paragraph element:

````html
<p>This paragraph, and its surrounding tags, is a single element</p>
````


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

**UP NEXT**: *[an introduction to basic CSS](https://github.com/egid/training/blob/master/frontend/CSS.md)*.

### Resources
* [caniuse.com][cani] - a series of compatibility tables for HTML5, CSS3, and other features.


# Additional reading (optional)

* [HTML5 for Web Designers, by Jeremy Keith][abhtml]
* [CSS3 for Web Designers, by Dan Cederholm][abcss]
* [Codecademy HTML & CSS track][codec]
	* great series of starting tutorials if you're very new to HTML & CSS
* [MDN Intro to HTML][mdnintro]

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