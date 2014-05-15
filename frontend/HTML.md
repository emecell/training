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

If any of this is ***too*** basic, feel free to move on. But it might be worth a refresher anyway. If nothing else, you can correct mistakes in this document, right?

### What the blazes is a tag?

A tag is surrounded by angle brackets, carets, less/greater-than-signs, whatever you want to call them. Alligators. Anyway, they generally *open* and *close*, which looks like this: `<html> foo </html>`. Notice how the second instance of the tag starts with a forward slash? That ends or *closes* the tag.

Tags and their contents are referred to as an *element*. Here's a paragraph element:

````html
<p>This paragraph, and its surrounding tags, is a single element</p>
````

To be valid code, tags must be *nested* properly. This means that the opening and closing tags of one element must lie within their **parent** element:

````html
<p>This paragraph is <strong>valid HTML</strong>, because the 'strong' tags lie within the 'p' tags.</p>

<p>This paragraph is <strong>invalid HTML</p></strong>, because it's nested improperly.
````

> **NOTE**: The best way to know that your code is going to work is to use a text editor or IDE that will syntax-highlight HTML. Some auto-complete or auto-close tags, which can be handy as well. I recommend [Sublime Text][sublimeintro], but it's your call.

Some tags are self-contained, and must not have contents; these **void elements** (such science fiction. wow.) are often marked up as "self-closing" tags, which (optionally, but do it anyway) end with a `/`; the image tag is the most typical example, and at its simplest looks like `<img />`. This example wouldn't actually *do* anything, but it's valid. Here's a more real-world example of an image tag:

````html
<img src="http://bukk.it/howimakeweb.gif" alt="Basic HTML" />
````

Notice how the tag has some key-value pairs, like `src` and `alt`? This brings us to:

### Attributes

That's right, attributes. HTML elements obtain their attributes from tags, and these can be used to provide the browser information about how they should look and behave. They consist of an attribute *name* and *value*, and the value should be surrounded by quotation marks, like so: `attr="value"`.

Here's an example of some basic attributes and tags in use:

````html
<div id="unique" class="reusable multiple" style="">
	div			block html element. contains attributes:
		id		is unique, and can only be used once per page. *use sparingly*.
		class	this is your swiss army knife. can reference multiple classes.
		style	defines an inline style. DO NOT USE. We will find you.

	inline html elements:
		<span class="highlight">this would inherit the style of the 'highlight' CSS class</span>
		<em>em</em> is the proper way to make text <i>italic</i>.
		<strong>strong</strong> is the proper way to make text <b>bold</b>.
</div>
````

### Semantics

One of the big challenges in writing HTML is to make sure that for a given task, the correct element is used. There are entire [chapters][html5semantics] written about semantics, probably unsurprisingly. Still, there are some simple guidelines (note: not rules) to follow that can get things on the right track.

**You should use elements that reflect their contents.** This means wrapping paragraphs in `<p>`, bulleted lists in `<ul>`, headlines in a `<h2>` or `<h3>` - that sort of thing. Don't create an unordered list and then use CSS to turn it into a numbered list later - that's unsemantic *and* happens to be pretty stupid.

Conversely: **You should not use elements just because they *look* correct**. Just because a mockup has big gaps between paragraphs, it's not acceptable to throw a ton of line breaks in there to make it happen. If I see `<br/><br/><br/><br/><br/>` anywhere, I'm running `git blame` and chasing you down.

Basically, use elements as they were intended, as much as possible. Keep the *visual presentation* of the page separate from the markup, and do the layout and interaction with Cascading Style Sheets and JavaScript. Lucky for you, it's now time to read about CSS!

**UP NEXT**: *[an introduction to basic CSS](https://github.com/egid/training/blob/master/frontend/CSS.md)*.

### Resources
* [caniuse.com][cani] - a series of compatibility tables for HTML5, CSS3, and other features.


# Additional reading (optional)

* [HTML5 for Web Designers, by Jeremy Keith][abhtml]
* [CSS3 for Web Designers, by Dan Cederholm][abcss]
* [Codecademy HTML & CSS track][codec]
	* great series of starting tutorials if you're very new to HTML & CSS
* [MDN Intro to HTML][mdnintro] provided a ton of inspiration for this document.

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
 [sublimeintro]: https://github.com/egid/training/blob/master/tools/SublimeText.md

 [html5semantics]: http://diveintohtml5.info/semantics.html