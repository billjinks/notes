---
layout: post
title:  "Forcing VM monitor size in VirtualBox"
date:   2018-01-15 18:53:00 +0000
categories: virtualbox
---
You need to execute the following commands:

```bash
VBoxManage setextradata global GUI/MaxGuestResolution any
VBoxManage setextradata "Machine Name" "CustomVideoMode1" "Width x Height x Bpp"
VBoxManage controlvm "Machine Name" setvideomodehint Width Height Bpp
```

* The first command unlocks all possible display resolutions for virtual machines.
* The second command defines a custom video mode for the specific virtual machine with name "Machine Name".
* Finally, the third command sets this custom video mode for your virtual machine.

**You must run these commands after the virtual machine has been started, when the guest operating system is ready to use and its Guest Additions are installed properly and loaded.**

In my case, I need to execute the following commands:

```bash
VBoxManage setextradata global GUI/MaxGuestResolution any
VBoxManage setextradata "Windows 7" "CustomVideoMode1" "1400x900x32"
VBoxManage controlvm "Windows 7" setvideomodehint 1400 900 32
````
