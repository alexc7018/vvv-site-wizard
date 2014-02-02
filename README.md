# VVV Site Wizard

This bash script makes it easy to spin up a new WordPress site using [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV).

## Installation

Place `vvv` in any folder included in your PATH environment variable. To add a folder to PATH:

1. Open `~/.bashrc` (or `~/.bash_profile`)
1. If `PATH` is not defined anywhere in the file, add this line:

		export PATH=/path/to/folder:$PATH

	Where `/path/to/folder` is the path to the folder containing the script.
1. Changes take effect in a new Terminal session.
1. To run the script from anywhere, open the file and uncomment the line at the top defining `path`. Set that to the root folder of VVV on your machine.

## Usage

Type `vvv` in the command line to use. When running the site setup or teardown wizards, you can include the desired name of your site's directory as a parameter. Example: `vvv new testsite`

### `vvv list`

This lists all the sites currently in VVV's `www` folder.

### `vvv new|create|make [site]`

Creating a site does the following:

* Halts Vagrant (if running)
* Creates a web root for the site in the `www` folder containing two files: `vvv-init.sh` and `vvv-hosts`
	* `vvv-init.sh` tells Vagrant to create a database if one does not exist and download or update WordPress from trunk the next time Vagrant is provisioned
	* `vvv-hosts` contains the hosts entry to give your site a nice custom domain (the domain is set in the wizard)
* Creates a file in the `nginx-config` folder to handle server settings for your site
* Restarts Vagrant with `vagrant up --provision`

Provisioning Vagrant takes a couple of minutes, but this is a crucial step as it checks out WordPress trunk into your site's htdocs directory.

#### `vvv new|create|make [site] [filesonly]`

Adding the `filesonly` parameter to this command will do everything in the list above except restart Vagrant. This is useful for testing the script quickly (as provisioning Vagrant takes a while).

### `vvv delete|rm|teardown [site]`

Deleting a site does the following:

* Halts Vagrant (if running)
* Deletes the site's web root (which deletes the `vvv-init.sh` and `vvv-hosts` files as well)
* Deletes the file in the `nginx-config` folder pertaining to the site

## Questions?

Ping me on Twitter at [@alisothegeek](http://twitter.com/alisothegeek).
