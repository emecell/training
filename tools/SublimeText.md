# Setting up Subl

* package manager
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