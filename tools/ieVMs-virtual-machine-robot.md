# Using `ievms` and `iectrl` to streamline IE testing

# Getting started

Install [ievms][ievm] for our major IE versions (8, 9, 10, 11)
````bash
	curl -s https://raw.github.com/xdissent/ievms/master/ievms.sh | env IEVMS_VERSIONS="8 9 10 11" bash
````

Install [node.js][node], if you haven't already. If eg doesn't work, or nvm barfs, you probably have a screwy install. You'll need to delete `~/.nvm` and reinstall node from nodejs.org, instead of via homebrew.	

Install [iectrl][iect]
````bash
sudo npm install -g iectrl
````

# Using iectrl

* Launch multiple browsers at once with the same URL: <br/>
	`iectrl open -s 9,11 http://eric-gideon.redfintest.com`
* Stop all open VMs: <br/>
	`iectrl stop`
* Get the status of all ievms-managed VMs, including Windows expiration dates: <br/>
	`iectrl status`
* Reset all your VMs to their clean snapshot: <br/>
	`iectrl clean`
* "Register" windows for another period: <br/>
	`iectrl rearm`




 [ievm]: https://github.com/xdissent/ievms
 [iect]: https://github.com/xdissent/iectrl
 [node]: http://nodejs.org
