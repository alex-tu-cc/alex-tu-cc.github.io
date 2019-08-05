---
layout: "post"
title: "buy a certified Ubuntu machine - secret of point release"
date: "2019-08-04 14:18"
---

# Background
Following another post [buy a certified Ubuntu machine][], there's a forum post shared OEM image of XPS 13 9380. So, let's based on that image to check what's the different between OEM image and stock Ubuntu. And there're some interesting things about point release. They are not actually secret, but easily be ignored.

# Before Start
 - the easiest way to compare instaleld packages
    - [dpkg -l of mockup][] which comes from "[buy a certified Ubuntu machine][]"
    - oem image from [foru mpost][] : Dell_XPS_9380_20190321_210_A02.iso
        - sha256sum : 0704f7a9cb24698b9f18de0e59272b2063b35b3147f88a28d96d983413ecda2b
        - md5sum : 4627a3fcae7dd10ebd956cc2488213c4
    - [manifest of 18.04.2][]
    - [manifest of 18.04][]
 - also can install each image on vm to check the difference precesly. the 
    - [iso of older 18.04][]
    - 18.04.2 (or maybe 18.04.3 after this post)

# Let's start it
## mockup vs. OEM image
 - [dpkg -l of mockup][]
 - [packages on machine installed oem image then do system update](http://paste.ubuntu.com/p/G67nDckDtg/)
 - diff oem image and mockup: [diff-oem-mockup][]
    - obviousely the main change which could impact functionality is hwe graphic`

## OEM image vs. Stock
 - compare to 18.04.2

## 18.04 vs. 18.04.2
```
Your 18.04.2 is not 18.04.2
```

## Get close to preloaded image
 - search OEM image from internet.

## Get close to latest stock
 - [LTS Enablement Stacks][]


# Wrap it up
 - what's the most stable environment.

[diff-oem-mockup]:http://paste.ubuntu.com/p/CcRjRt8RSv/
[manifest of 18.04]:http://old-releases.ubuntu.com/releases/18.04.1/ubuntu-18.04-desktop-amd64.manifest
[iso of older 18.04]:http://old-releases.ubuntu.com/releases/18.04.1/  
[dpkg -l of mockup]:http://paste.ubuntu.com/p/TXFgTZMNnk/
[LTS Enablement Stacks]:https://wiki.ubuntu.com/Kernel/LTSEnablementStack
[forum]:https://askubuntu.com/questions/1136409/new-xps-13-9380-with-ubuntu-18-04-flicker-problems/1149630#1149630
[buy a certified Ubuntu machine]:https://alex-tu-cc.github.io/2019/07/buy-a-certified-ubuntu-machine/
[18.04]:https://wiki.ubuntu.com/BionicBeaver
[manifest of 18.04.2]:http://releases.ubuntu.com/18.04/ubuntu-18.04.2-desktop-amd64.manifest
