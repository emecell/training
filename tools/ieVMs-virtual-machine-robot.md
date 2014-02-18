# Using `ievms` and `iectrl` to streamline IE testing

# Getting started

Install [ievms][ievm] for our major IE versions (8, 9, 10, 11)
````
curl -s https://raw.github.com/xdissent/ievms/master/ievms.sh | env IEVMS_VERSIONS="8 9 10 11" bash
````

Install [NVM][nvm] (node version manager) if you haven't already
````
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
nvm install 0.10
nvm alias default 0.10
echo "nvm use 0.10" >> ~/.bash_profile ## tells your system to use .10 by default
source ~./bash_profile ## reloads terminal session
````

Install [iectrl][iect]
````
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
 [nvm]: https://github.com/creationix/nvm
