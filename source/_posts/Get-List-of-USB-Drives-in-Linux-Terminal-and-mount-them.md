---
title: 'Get List of USB Drives in Linux Terminal, and mount them'
date: 2016-03-24 23:54:50
tags: 
- usb
- mount
- mount drives
- mount usb in linux
category: Linux 
---

# Get List of USB Drives in Linux Terminal, and mount them


Get List of USB Drives in Linux Terminal, and mount them
Instead of using lsusb, use the following command to get a comma separated list of USB disk drives,


```bash
$ lsblk | grep “sd[^a]” | tail -n +2 | cut -c7-10 | sed ‘/^ $/d’ | paste -s -d,
```


<!-- more -->

This has been tested on ubuntu only.