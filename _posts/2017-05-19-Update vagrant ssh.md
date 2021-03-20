---
layout: post
title:  "Update vagrant ssh key"
date:   2017-05-19 12:08:00 +0000
categories: vagrant
---

If vagrant errors with as follows:
```
default: Warning: Authentication failure. Retrying...
```
then the ssh key within the guest needs updating.
## Find the identity on the vagrant host
```
vagrant ssh-config
```
This will result with something like this - it's the identity file we need
```
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/billjinks/.vagrant.d/boxes/bjscotch-VAGRANTSLASH-box/0/virtualbox/vagrant_private_key
  IdentitiesOnly yes
  LogLevel FATAL
  ```
  ## Get the key
  Issue the following command to get the key we need for the guest (identity filename taken from above)
  ```
  ssh-keygen -y -f /Users/billjinks/.vagrant.d/boxes/bjscotch-VAGRANTSLASH-box/0/virtualbox/vagrant_private_key
  ```
  Copy the output to the clipboard
  ## Apply key in guest
  Login to the guest using ```vagrant shh``` - you will need to login with a password. Once logged in, navigate to 
  ```~/.ssh``` and edit the ```authorized_keys``` file. Add a line to the file and paste the key.