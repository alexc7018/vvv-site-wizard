# VVV Site Wizard

Version 1.2

This bash script makes it easy to spin up a new WordPress site using [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV).

## Installation

Download the script and run `./vvv` from the directory the script is placed in.

To run the script from anywhere, you can place `vvv` in a folder included in your PATH environment variable.

If you don't want to define the path each time you run the script, open the file and uncomment the line at the top defining `path`. Set that to the root folder of VVV on your machine. **Note:** You do not need to do this if VVV is installed in the default location (`~/vagrant-local/`).

## Usage

Type `vvv` in the command line to use. None of the options are required: if a required piece of information is not included in the original command, the wizard will prompt you for it.

### Options

|Option|Name       |Description|
|------|-----------|-----------|
|`-a`  |action     |`new`/`create`/`make` runs the site creation wizard, `rm`/`delete`/`teardown` runs the site teardown wizard, `list` lists all current VVV sites|
|`-d`  |domain     |Desired domain (e.g. mysite.dev)|
|`-f`  |files only |Do not provision Vagrant, just create the site directory and files|
|`-i`  |image proxy|Load images by proxy from the live site (so you don't have to download the uploads folder)|
|`-l`  |live URL   |URL of the live site, currently only used if loading images by proxy|
|`-m`  |multisite  |Install as a multisite network (takes argument: 'subdirectory' or 'subdomain')|
|`-n`  |site name  |Desired name for the site directory (e.g. mysite)|
|`-p`  |path       |Path to VVV root (e.g. ~/vagrant-local)|
|`-v`  |version    |Version of WordPress to install|
|`-x`  |debug      |Turn on WP_DEBUG and WP_DEBUG_LOG|

Examples:

```
vvv -a new
vvv -a create -n mysite -d mysite.dev -v 3.9.1 -x
vvv -a make -fxil mysite.com -n mysite -d mysite.dev -v 3.9.1
vvv -a delete mysite
vvv -a list
```

## Site Creation Wizard

Creating a site does the following:

* Halts Vagrant (if running)
* Creates a web root for the site in the `www` folder containing three files: `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts`
	* `vvv-init.sh` tells Vagrant to create a database if one does not exist and install the latest version of WordPress (via WP-CLI) the next time Vagrant is provisioned
	* `wp-cli.yml` tells WP-CLI that WordPress is in the htdocs folder
	* `vvv-hosts` contains the hosts entry to give your site a nice custom domain (the domain is set in the wizard)
* Creates a file in the `nginx-config` folder to handle server settings for your site
* Restarts Vagrant with `vagrant up --provision`

Provisioning Vagrant takes a couple of minutes, but this is a crucial step as it downloads WordPress into your site's htdocs directory and runs the installation. If you want to skip provisioning and install WordPress manually, you can run the new site's `vvv-init.sh` file directly in the Vagrant shell.

## Site Teardown Wizard

Deleting a site does the following:

* Halts Vagrant (if running)
* Deletes the site's web root (which deletes the `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts` files as well)
* Deletes the file in the `nginx-config` folder pertaining to the site

Note that it does not delete the site's database.

## Questions?

Ping me on Twitter at [@alisothegeek](http://twitter.com/alisothegeek).

## Changelog

### 1.2

* Add multisite option (works with both subdirectory and subdomain installs)
* Move database creation to `database/init-custom.sql`

### 1.1.2

* Add the ability to load images by proxy from the live site (props [@TheLastCicada](https://gist.github.com/TheLastCicada/ee6775c5f269f5f5389f))
* Fix bug where wp_debug was not set properly when set via command option

### 1.1.1

* Add default VVV install path to script (if VVV is installed in default location, `path` no longer needs to be explicitly defined)

### 1.1

* Add ability to select WordPress version (props [@adamsilverstein](https://github.com/aliso/vvv-site-wizard/pull/10))
* Add ability to define `WP_DEBUG` and `WP_DEBUG_LOG`
* Convert positional parameters to command line options for maximum flexibility

### 1.0.1

* Switch to using WP-CLI instead of SVN to install WordPress
* Allow for tab completions when defining directories

### 1.0

* Initial release
