# Front End 250: Mobile all the Things


## The Basics

1. terms to know:
	* viewport (the size of the browser window)
	* **TODO** ADD MORE?
2. types of layout:
	* fixed: pixel-based; like it sounds. never changes.
	* fluid: percentage-based; proportional layout changes with viewport
	* adaptive: uses media queries to implement multiple fixed layouts
	* responsive: uses media queries to implement multiple fluid layouts

[Responsive Web Design][rwd] has become something of a buzzword in the last few years. It's just one technique available that attempts to create web content that works well on multiple devices. Nearly any attempt to optimize a site for various screen sizes requires developers to target specific **visual viewport** sizes with different CSS and JavaScript; the [viewport][quirks-viewport] is simply the space available in a device or browser that can show HTML content.

There are four categories of layout:

	* **Fixed** is pixel-based; like it sounds. never changes.
	* **Fluid** is percentage-based; proportional layout changes with viewport
	* **Adaptive** uses media queries to implement several *fixed* layouts
	* **Responsive** uses media queries to implement several *fluid* layouts

They're best compared using a tool like [Liquidapsive](http://liquidapsive.com), which lets you quickly jump between layouts at different viewport sizes. You'll notice that **fixed** and **fluid** work well at larger viewport sizes -- and rapidly break down at smaller viewport sizes. **Adaptive** and **Responsive** move between a series of layouts that match the viewport well, and rapidly became the techniques most commonly used to optimize for multiple screens.

**Adaptive** and **Responsive** techniques are actually interchangeable; it's reasonable to design a hybrid website that switches from a fixed desktop layout to fluid tablet and smartphone layouts.


## media queries

So, you've decided that you want to switch between a few different layouts - mobile, tablet, and desktop; that's swell. The majority of the work gets handled by **CSS3 Media Queries**, which format html differently for different mediums. Early usage was mostly for things like [print or screenreaders](http://www.w3.org/TR/CSS2/media.html). The CSS3 spec added [significantly more power](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries) to media queries, allowing developers to target viewport or device width and height. There are other queries, but they're less useful or poorly supported.

Browser support for CSS3 has become quite good, with the newer browsers all auto-updating themselves and making browsers less of an issue than they used to be. However, it's important to note early on that **Internet Explorer 8 has no support for CSS3, including CSS3 Media Queries**. Keep this in mind, as there are some implications that might affect you -- and some workarounds that might help out.

2. state of the internet
	* browser support: no IE8
	* redfin support: no IE <10 unless otherwise specified
3. breakpoints
4. syntax / convention at Redfin
	* Media Query mixin that builds for IE8+


## javascript PE / degradation

Ideally, pages viewed on mobile devices should be as lightweight as possible. This includes javascript. Most mobile browsers block page rendering or interaction while javascript runs, as they currently have performance limitations compared to even the slowest desktops. [Sencha][sencha-perf] describes the performance situation like this:

> Mobile JavaScript + mobile DOM access is getting progressively faster, but **you should still treat the iPhone 5 as if it’s a Chrome 1.0 browser on a 2008-era desktop** (aka 5–10x faster than desktop IE8). *[emphasis added]*

Drew Crawford has written a stunning [10,000 words on why web apps are slow][sealed], which is also worth a read if you've got a month or so to spare (seriously, it's comprehensive and a great read).

* has.js


## mobile web at Redfin

We have two main ways to handle the construction of mobile web pages and apps:

1. Mobile First
	* Build the minimum viable product first
	* Build it on mobile
	* Use PE to improve the experience for tablet and desktop
2. Mobile Last
	* Build a desktop page
	* Shoehorn in some CSS to make it look right
	* Attempt to use has.js after the fact to trim out the bits that perform poorly

From a performance standpoint, Mobile First is the preferable approach. It's only really practical on a clean-sheet design, though.


# Additional reading (optional)

* [Responsive Web Design, by Ethan Marcotte][abrw]
* [Mobile First, by Luke Wroblewski][abmf]


<!-- LINKS -->

 [sencha-perf]: http://www.sencha.com/blog/5-myths-about-mobile-web-performance/
 [sealed]: http://sealedabstract.com/rants/why-mobile-web-apps-are-slow/
 [rwd]: http://alistapart.com/article/responsive-web-design/
 [abrw]: http://www.abookapart.com/products/responsive-web-design
 [abmf]: http://www.abookapart.com/products/mobile-first
 [quirks-viewport]: http://www.quirksmode.org/mobile/viewports2.html