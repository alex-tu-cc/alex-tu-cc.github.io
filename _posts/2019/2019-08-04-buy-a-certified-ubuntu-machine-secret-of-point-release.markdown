---
layout: "post"
title: "buy a certified Ubuntu machine - secret of point release"
date: "2019-08-04 14:18"
---

<p style="font-size: 0.9rem;font-style: italic;"><a href="https://www.flickr.com/photos/28809618@N06/4843823526">"Ubuntu CD cover"</a><span>by <a href="https://www.flickr.com/photos/28809618@N06">Fotografie di Bernardo.</a></span> is licensed under <a href="https://creativecommons.org/licenses/by-sa/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC BY-SA 2.0</a><a href="https://creativecommons.org/licenses/by-sa/2.0/?ref=ccsearch&atype=html" target="_blank" rel="noopener noreferrer" style="display: inline-block;white-space: none;opacity: .7;margin-top: 2px;margin-left: 3px;height: 22px !important;"><img style="height: inherit;margin-right: 3px;display: inline-block;" src="https://search.creativecommons.org/static/img/cc_icon.svg" /><img style="height: inherit;margin-right: 3px;display: inline-block;" src="https://search.creativecommons.org/static/img/cc-by_icon.svg" /><img style="height: inherit;margin-right: 3px;display: inline-block;" src="https://search.creativecommons.org/static/img/cc-sa_icon.svg" /></a></p>

# Background
Following another post "[buy a certified Ubuntu machine][]", there's a forum post shared OEM image of XPS 13 9380. So, let's based on that image to check what's the different between OEM image and stock Ubuntu. And there are some interesting things about point release. They are not actually secret, but easily be ignored.

# Before Start
 - the easiest way to compare installed packages
    - [dpkg -l of mockup][] which comes from "[buy a certified Ubuntu machine][]"
    - oem image from [forum post](https://askubuntu.com/questions/1136409/new-xps-13-9380-with-ubuntu-18-04-flicker-problems/1149630#1149630) : Dell_XPS_9380_20190321_210_A02.iso
        - sha256sum : 0704f7a9cb24698b9f18de0e59272b2063b35b3147f88a28d96d983413ecda2b
        - md5sum : 4627a3fcae7dd10ebd956cc2488213c4
    - [manifest of 18.04.2][]
    - [manifest of 18.04][]
 - another way is to install each image on vm to check the difference.
    - [iso of older 18.04][]
    - 18.04.2 (or maybe 18.04.3 after this post)

# Let's start it
## mockup vs. OEM image

```
  The significant difference is HWE graphic
```
- [dpkg -l of mockup][]
- [packages on machine installed OEM image then do system update](http://paste.ubuntu.com/p/G67nDckDtg/)
- diff OEM image and mockup: [diff-oem-mockup][]
   - The significant difference is HWE graphic

Because the mockup environment is built from stock Ubuntu, so the easier way is to find out what are the difference between stock and OEM image.

## OEM image vs. Stock
```
  OEM image came from 1st version of 18.04
```

- [dpkg -l of 18.04.2 after system upgraded by UI](http://paste.ubuntu.com/p/ZbfNWZS7Dy)
- [packages on machine installed OEM image then do system update](http://paste.ubuntu.com/p/G67nDckDtg/)
- [diff 18.04.2 and OEM](http://paste.ubuntu.com/p/H87dFfgk6P/)
  - the significant difference is hwe kernel and HWE graphic, so we know that after 18.04.2, kernel and graphic stack is changed to HWE. (End user can also follow the [Ubuntu wiki][change to HWE stack] to change their system to HWE stack as well.)

So, here we can know the OEM image came from first version of 18.04 which also explained why the OEM image was adopted with 4.15 kernel.

## 18.04 vs. 18.04.2

Because we know OEM image came from 1st version of 18.04, then don't you curious the difference between 18.04 and 18.04.2?
The easiest way to diff them is by their manifest.
- [manifest of 18.04.2][]
- [manifest of 18.04][]
- [diff 18.04 and 18.04.2](http://paste.ubuntu.com/p/gbNmD6wvrR/)
  - the significant difference is hwe kernel and HWE graphic

## Upgrade 18.04 to 18.04.2
```
  Your 18.04.2 is not 18.04.2
```
Some user might expect the system upgrading by UI can bring your system up to latest point release. But the fact is a bit different.
- 18.04 after upgrading by UI
  - [lsb_release -a](http://paste.ubuntu.com/p/xvR3Mwv4RK)
  - [dpkg -l](http://paste.ubuntu.com/p/BNDJSB39nh)
- 18.04.2 after upgrading by UI
  - [lsb_release -a](http://paste.ubuntu.com/p/DXftrhmNK2)
  - [dpkg -l](http://paste.ubuntu.com/p/ZbfNWZS7Dy)
- [diff 18.04 and 18.04.2](http://paste.ubuntu.com/p/2pyj9JxYJ6/)
  - the difference is still there.

From lsb_release, it shows the same that system is 18.04.2. But different from dpkg -l. Your 18.04.2 is not 18.04.2. Following [Ubuntu wiki][change to HWE stack] can bring the system to same as stock Ubuntu 18.04.2. Same as stock Ubuntu, OEM image follow the same cadence.

So, there are 2 ways you can do on your certified machine.
## Get close to preloaded image
After all, the easiest way is to find OEM image from internet. But it's not always be easy, because logically the OEM images only can be requested by service tag of Dell Ubuntu edition machine.

Then, the 2nd way to achieve this target is to understand what point release of your machine be certified. Install that point release, then change kernel to OEM kernel.
## Get close to latest stock
This way is just same as all Ubuntu users, but remember to find the difference between point release, and manually install the difference.

# Wrap it up
- What's the most stable environment?
  - No doubt, the most stable environment should be the one which used to pass certification.


- Ubuntu edition or Windows edition, which one is better choice?
  - save money
    - Ubuntu edition usually save some money than Windows edition(In my latest searching, it can saved USD 100).
  - Windows driver always can be found on OEM website.
      - And almost all of Windows driver can be found on OEM website, so it's not hard to make a preloaded Windows environment. In opposite, it need a bit manually operations to make a preloaded Ubuntu environment.
  - Windows 10 is free for end user.

So for me, I will save that 100 USD.

[change to HWE stack]:https://wiki.ubuntu.com/Kernel/LTSEnablementStack
[diff-oem-mockup]:http://paste.ubuntu.com/p/CcRjRt8RSv/
[manifest of 18.04]:http://old-releases.ubuntu.com/releases/18.04.1/ubuntu-18.04-desktop-amd64.manifest
[iso of older 18.04]:http://old-releases.ubuntu.com/releases/18.04.1/  
[dpkg -l of mockup]:http://paste.ubuntu.com/p/TXFgTZMNnk/
[LTS Enablement Stacks]:https://wiki.ubuntu.com/Kernel/LTSEnablementStack
[forum]:https://askubuntu.com/questions/1136409/new-xps-13-9380-with-ubuntu-18-04-flicker-problems/1149630#1149630
[buy a certified Ubuntu machine]:https://alex-tu-cc.github.io/2019/07/buy-a-certified-ubuntu-machine/
[18.04]:https://wiki.ubuntu.com/BionicBeaver
[manifest of 18.04.2]:http://releases.ubuntu.com/18.04/ubuntu-18.04.2-desktop-amd64.manifest
