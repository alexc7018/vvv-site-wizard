# VVV Site Wizard

This bash script makes it easy to spin up a new WordPress site using [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV).

## Installation

Download the script and run `./vvv` from the directory the script is placed in.

To run the script from anywhere, you can place `vvv` in a folder included in your PATH environment variable.

If you don't want to define the path each time you run the script, open the file and uncomment the line at the top defining `path`. Set that to the root folder of VVV on your machine.

## Usage

Type `vvv` in the command line to use. When running the site setup or teardown wizards, you can include the desired name of your site's directory as a parameter. Example: `vvv new testsite`

### `vvv list`

This lists all the sites currently in VVV's `www` folder.

### `vvv new|create|make [site]`

Creating a site does the following:

* Halts Vagrant (if running)
* Creates a web root for the site in the `www` folder containing three files: `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts`
	* `vvv-init.sh` tells Vagrant to create a database if one does not exist and install the latest version of WordPress (via WP-CLI) the next time Vagrant is provisioned
	* `wp-cli.yml` tells WP-CLI that WordPress is in the htdocs folder
	* `vvv-hosts` contains the hosts entry to give your site a nice custom domain (the domain is set in the wizard)
* Creates a file in the `nginx-config` folder to handle server settings for your site
* Restarts Vagrant with `vagrant up --provision`

Provisioning Vagrant takes a couple of minutes, but this is a crucial step as it downloads WordPress into your site's htdocs directory and runs the installation. If you want to skip provisioning and install WordPress manually, you can run the new site's `vvv-init.sh` file directly in the Vagrant shell.

#### `vvv new|create|make [site] [filesonly]`

Adding the `filesonly` parameter to this command will do everything in the list above except restart Vagrant. This is useful for testing the script quickly, or if you're creating multiple sites (as provisioning Vagrant takes a while).

### `vvv delete|rm|teardown [site]`

Deleting a site does the following:

* Halts Vagrant (if running)
* Deletes the site's web root (which deletes the `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts` files as well)
* Deletes the file in the `nginx-config` folder pertaining to the site

Note that it does not delete the site's database.

## Questions?

Ping me on Twitter at [@alisothegeek](http://twitter.com/alisothegeek).
