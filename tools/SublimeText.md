# Setting up Subl

* [Package Control][spc] is a package manager built into sublime. Install it. Now.
* Must-have packages:
	* [GitGutter][gg] - git +/-/• in your sidebar
	* [LESS][less] - syntax highlighting for everybody's favorite CSS preprocessor (not really).
	* [Sass][sass] - if you happen to need Sass, which is better than LESS, here is syntax highlighting.
* workspace - add `~/$REDFIN_MAIN/` folder
	* exclude target folders:
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

* keyboard shortcuts

	| hotkey		| result			|
	| ------------- | ----------------- |
	| ⌘-P 			| goto anything		|
	| ⌘-shift-R		| full text search	|
	| ⌘-shift-P		| invoke SPM		|
	| ⌘-,			| preferences		|

* terminal
	* `subl {file}` opens that file in sublime


# Setting up Eclipse

* Prefs > search 'refresh' 
	* [x] build automatically
	* [x] native hooks
* keyboard shortcuts

	| hotkey		| result			     |
	| ------------- | ---------------------- |
	| F5			| refresh				 |
	| ⌘-shift-F11	| last run configuration |


# Tweaks

* themes
* syntax highlighting
* minimap


<!-- LINKS -->
 [spc]: https://sublime.wbond.net/
 [gg]: https://sublime.wbond.net/packages/GitGutter
 [less]: https://sublime.wbond.net/packages/LESS
 [sass]: https://sublime.wbond.net/packages/Sass