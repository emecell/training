***Before reading***, *you should be familiar with the [basics of HTML & CSS](https://github.com/egid/training/blob/master/frontend/Front%20End%20101%20-%20HTML%20and%20CSS.md)*.

# LESS is moar at Redfin

**LESS** is a *preprocessor* that adds a number of features not in the CSS spec. These include nested rules, mixins, variables & globals, and a limited set of functions. The other popular CSS preprocessor is **Sass**, and it's awesome, but it's not compatible and won't be discussed here.

At Redfin, we run a modified version of LESS that strips out globals, as they caused some problems in our initial implementations. Instead, we use a preamble rollup that dynamically appends a set of mixins and variables to every LESS stylesheet. This allows us to maintain the benefits of globals without the risk of variables bleeding across sections of the site.

A major risk with writing LESS comes from the ability to nest rules. If the feature is overused, it's easy to wind up with CSS that is far, *far* too specific and screws with inheritance. **Use the awesome [LESS2CSS][less2css] convertor to test your code,** and you'll do pretty well.


## The preamble

The **LESS preamble** is a Redfin customization that rolls up several other stylesheets and lives in stingrayStatic:

    main/redfin.stingrayStatic/src/main/resources/images/text/css/common/less-preamble/preamble.less

It's pretty simple. What this file does is import `_colors.less`, `_mixins.less`, and `_media-queries.less`. The **colors** are [documented in the Redfin Style Guide](rsg-colors); the mixins and media-queries utilities are not *but ought to be* (**TODO**: add documentation).

As stated in the Preamble's intro:

>  Before adding this to this file (or any file underneath the less-preamble/ directory), you should talk to some people and decide if that thing you want to add really needs to be global, and available in EVERY file on the site.

> **PLEASE NOTE**: Any changes to the preamble files require a server restart to take effect.

Keep this in mind when adding to the preamble and testing your changes.


## Nesting

Unlike CSS, LESS allows the nesting of classes. This can be convenient for organization, but can create something of a rats' nest in the output if you aren't careful.

Here's some example LESS:

````css
.foo {
	font-size: 1em;
	.bar {
		font-size: 2em;		/* Compiles to .foo .bar */
		&.baz {				/* Compiles to .foo .bar.baz  ... `&` compiles to the previous classes (.foo .bar) */
			color: red;
		}
	}
	> .bar {				/* Compiles to .foo > .bar */
		font-size: blue;
	}
}
````
Which will output this CSS (try it with [LESS2CSS][less2css]):

````css
.foo {
	font-size: 1em;
}
.foo .bar {
	font-size: 2em;
}
.foo .bar.baz {
	color: red;
}
.foo > .bar {
	font-size: blue;
}
````


## Variables

LESS supports variables. Nice.

Normally, variables defined in LESS would be globals, always. This is not so great — how do you know if a variable name is in use already? What if you redefine it? At Redfin, **we've disabled global variables** and replaced them with the Preamble, which makes *some* variables available globally. This more or less solves the problem.




## Functions




<!-- LINKS -->
 [less2css]: http://less2css.org


**UP NEXT**: *[Javascript](https://github.com/egid/training/blob/master/frontend/Javascript.md)*.
