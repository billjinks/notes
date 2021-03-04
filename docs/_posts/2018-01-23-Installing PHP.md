---
layout: post
title:  "Installing PHP on Ubuntu"
date:   2018-01-23 19:19:00 +0000
categories: virtualbox
---
Thanks to Ondřej Surý for maintaining PPA of latest PHP5 versions on launchpad. If you want to install specific version of php, then this article can be helpful for you. This article will help you for installing PHP 5.6 or PHP 7.1 using PPA on Ubuntu 16.10, 16.04 LTS, 14.04 LTS or 12.04 LTS systems. If you have already have higher version installed on your system and you need to install lower version, then you have to remove higher version first and remove apt repository from system.

## Install PHP 5.6 on Ubuntu

Use the following set of command to add PPA for PHP 5.6 in your Ubuntu system and install PHP 5.6.

```
$ sudo apt-get install python-software-properties
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install -y php5.6
```

### Check Installed PHP Version:

```
$ php -v
```
## Install PHP 7.1 on Ubuntu
Use the following set of command to add PPA for PHP 7.1 in your Ubuntu system and install PHP 7.1.
```
$ sudo apt-get install python-software-properties
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install -y php7.1
```
## Switch Between PHP Versions

If you have installed multiple php versions and want to use different than default. Use following steps to switch between php5.6 and php7.1 version. You can use the same command for other php versions. 

### From PHP 5.6 => PHP 7.1

For Apache:
```bash
$ sudo a2dismod php5.6
$ sudo a2enmod php7.1
$ sudo service apache2 restart
```
For CLI:
```bash
$ update-alternatives --set php /usr/bin/php7.1
```

### From PHP 7.1 => PHP 5.6 

For Apache:
```bash
$ sudo a2dismod php7.1
$ sudo a2enmod php5.6
$ sudo service apache2 restart
```
For CLI:
```bash
$ sudo update-alternatives --set php /usr/bin/php5.6
```

## Setup XDebug ##

The configuration file is found in ```/etc/php/[ver]/apache2/conf.d/20-xdebug.ini``` where [ver] is 5.6 or 7.1. To enable xdebug
to work remotely from the local machine to the vagrant guest, use the following:

```
zend_extension=xdebug.so
xdebug.remote_enable=1
; xdebug.remote_connect_back=1
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
xdebug.remote_host=10.0.2.2
xdebug.remote_port=9000
xdebug.remote_log="/home/vagrant/xdebug.log"
; xdebug.remote_autostart=1
```
The two commented out lines should not need to enabled

### Debugging with VS Code

The XDebug settings above will allow remote debugging from VS Code to the Vagrant VM.

* Install the PHP Debug extension
* Go to the debug page in VS Code and open the `launch.json` settings (via the gear icon)
* add the path mapping, e.g.

```json
"pathMappings": {
   "/var/www/web": "${workspaceRoot}/web",
   "/var/www/protected": "${workspaceRoot}/protected"
}
```

Because `xdebug.remote_autostart=0` you need to manually start the server sending XDebug

* For Firefox, install and use the **theeasiestxdebug** extension, which adds a toolbar button to toggle debug
* For CLI (running in VM), you need to run the command `export XDEBUG_CONFIG="idekey=xdebug"` before running the PHP script you want to debug


## missing packages in PHP 7.1 ##

The default packaging of PHP7.1 on the box does not include the mbstring or mysqli functions. To install:

```bash
$ sudo apt-get install php7.1-mbstring
$ sudo apt-get install php7.1-mysqli
```