---
layout: post
title:  "How to repackage a Vagrant Box"
date:   2017-05-14 17:18:00 +0000
categories: vagrant
---

## SSH into the Box and Customize It

We'll now SSH into the box and start customizing it. If you don't know anything about servers,
you can always just use Scotch Box since all of this is already done for you. Here's how to SSH into the box:

```
vagrant ssh
```

Now, we need to setup our server by installing whatever we want on it. This example will install a basic LAMP stack,
but you can do whatever you like (Ruby, Nginx, etc.). The point of this article isn't to teach you how to setup a server,
but instead how to turn your virtual environment into a Vagrant Box. So this step might be more involved than what is actually shown:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vim
sudo apt-get install apache2
sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
sudo mysql_install_db
sudo /usr/bin/mysql_secure_installation
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
sudo apt-get install php5-cgi php5-cli php5-curl php5-common php5-gd php5-mysql
sudo service apache2 restart
```
## Make the Box as Small as possible

We're now going to clean up disk space on the VM so when we package it into a new Vagrant box,
it's as clean as possible. First, remove APT cache

```
sudo apt-get clean
```
Then, "zero out" the drive (this is for Ubuntu):
```
sudo dd if=/dev/zero of=/EMPTY bs=1M
sudo rm -f /EMPTY
```
Lastly, let's clear the Bash History and exit the VM:
```
cat /dev/null > ~/.bash_history && history -c && exit
```
## Repackage the VM into a New Vagrant Box

We're now going to repackage the server we just created into a new Vagrant Base Box. It's very easy with Vagrant:
```
vagrant package --output mynew.box
```
## Add the Box into Your Vagrant Install

The previous command will have created a "mynew.box" file. You can technically put this wherever you want on your computer. Now, let's add this new Vagrant Box into Vagrant:
```
vagrant box add mynewbox mynew.box
```
This now will "download" the box into your Vagrant install allowing to initiate this from any folder, but before we do this, let's delete and remove the Vagrant file we built this box from.
```
vagrant destroy
rm Vagrantfile
```
## Initialize Your New Vagrant Box

We need to now initialize a Vagrant environment from our brand new box using the same command from earlier but referencing the new Box.
```
vagrant init mynewbox
```
