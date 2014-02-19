# Sublime Text!

Here's what y'all need to know:

1. It is very nice, and very fast.
2. It looks rather good.
3. It is not free. At $70 per user, it's not even cheap. But it's very nice.

# Setting up Sublime Text

* [Package Control][spc] is a package manager that runs inside Sublime. Install it. Now.
	* Invoke SPM by pressing ⌘-shift-P on a Mac, or something else on Windows.
	* Start typing "install" once the field is up to get the searchable list of packages.

* Must-have packages:
	* [GitGutter][gg] - git +/-/• in your sidebar
	* [LESS][less] - syntax highlighting for everybody's favorite CSS preprocessor (not really).
	* [Sass][sass] - if you happen to need Sass, which is better than LESS, here is syntax highlighting.
* Project (workspace) - add `~/$REDFIN_MAIN/` folder
	* exclude target folders for your project:
		* menu > Project > Edit Project
		* make sure you have `folder_exclude_patterns`, like so:

			````js
			{
				"folders":
				[
					{
						"follow_symlinks": true,
						// UPDATE WITH YOUR PATH
						// "path": "/Users/you/path/to/redfin/main", 
						"folder_exclude_patterns": [".metadata","target","target-eclipse"]
					}
				]
			}
			````
* One of the best parts of Sublime is the Goto menu, which includes a ton of various ways to go to things. I use a couple of shortcuts pretty regularly:

	| hotkey		| result			|
	| ------------- | ----------------- |
	| ⌘-P 			| goto anything		|
	| ⌘-shift-R		| full text search	|
	| ⌘-shift-P		| invoke SPM		|
	| ⌘-,			| preferences		|

* terminal
	* `subl {file}` opens that file in sublime


# Setting up Eclipse

When setting up Eclipse for an external text editor (whether or not you're using Sublime) you need to enable auto-refresh using native hooks and polling. You also probably want to build automatically:

* Prefs `(⌘ + ,)`; search 'refresh'
	* enable build automatically
	* enable native hooks
* Some handy keyboard shortcuts:

	| hotkey		| result			     |
	| ------------- | ---------------------- |
	| F5			| refresh				 |
	| ⌘-shift-F11	| last run configuration |


# More tweaks

* Recommended themes / skins:
	* [Flatland](https://github.com/thinkpixellab/flatland)
* syntax highlighting
	* Freemarker - for HTML emails sent programmatically
	* Markdown - for editing Style Guide content
	* HAML


<!-- # LINKS -->
 [spc]: https://sublime.wbond.net/
 [gg]: https://sublime.wbond.net/packages/GitGutter
 [less]: https://sublime.wbond.net/packages/LESS
 [sass]: https://sublime.wbond.net/packages/Sass