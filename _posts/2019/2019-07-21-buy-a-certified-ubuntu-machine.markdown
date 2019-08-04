---
layout: "post"
title: "buy a certified Ubuntu machine"
date: "2019-07-21 20:06"
---
# Background
Refer to the Ubuntu [certification page][], there are hundreds of machines has been certified. But as an end user, it’s still very hard to buy one. So I’m thinking about how to manually make the environment as close as the preloaded image on Ubuntu edition machines. A simple reason behind this idea is that both Dell and Canonical verified the preloaded image. So, it should be the best choice for a stable working environment.

Recently, I got a XPS 13 9380 windows edition, let’s base on that machine to find out the way to this target.

# Before Start
All machines which certified by Ubuntu can be found on the [certification page][], from that page, we know the certification was done by preloaded OEM image. In my case, searching 9380 on that page then get 4 machines and all of them were certified by 18.04 (LTS) + kernel 4.15.0-1027-oem

It’s also a way to understand what combination was certified before you order it. Say, if the you got a XPS 13 9380 with Intel wifi, then that configuration is not certified. (Note: it could still just work well , but not be certified. )

Take my 9380 as an example, the certified configuration which mostly close to mine is [this one][201810-26512] which have some key components:

- Processor: Intel Intel(R) Core(TM) i7-8565U CPU @ 1.80GHz
- Video: Intel UHD Graphics 620 (Whiskey Lake) [8086:3ea0]
- Wireless: Qualcomm Atheros Killer N1525 Wireless-AC  [168c:003e]
- Bluetooth: Foxconn / Hon Hai 0489:e0a2
- Touchpad: DELL08AF:00 06CB:76AF
- Touchscreen: ELAN292F:00 04F3:292F

But there's still some lack of information which might break the stability if it’s not the same as Ubuntu edition machines:

- BIOS version
- Panel
- Wifi module version. (e.g a01, a02)

# Make a plan
To make my environment as close as the preloaded image, the plan is to install latest [18.04][] then replace the kernel to [oem kernel][]. So, that I can mostly mimic the environment of a Ubuntu edition machine which get software updated by user.

# The operations

1. Install 18.04
2. Update system by “software updater” or even aggressively
3. executing command to do full system upgrade: `sudo apt full-upgrade`
4. Install oem kernel: `sudo apt-get install linux-oem`
5. reboot to oem kernel 4.15
6. Purge the kernel came with Stock Ubuntu 18.04
  - from [manifest of 18.04.2][], the native kernel is 4.18.0-15. Even it newer than target oem kernel, but our goal is to mimic certified environment. So, just replace it ;)
7. Install others that you need.
  - e.g.Install TLP to save power: `sudo apt-get install -y tlp`; This save a lot of power consumption, but a bit risky if you get devices which not works well with auto suspend.
8. Reboot and enjoy the relatively stable environment.

# Following up
My system works generally well so far. And this is all packages and their version I installed on system for reference: [dpkg-l][].

BTW,  as arecord, my BIOS is 1.3.2, in case BIOS OTA break stability later. I keep noticed a "system update" in software update, that's actually a LVFS update for BIOS. I rather postpone BIOS updating , because I assume the BIOS from factory should be relative stable. At least, the fundamental functions (e.g. wifi, bt..etc) should be verified before shipping.

If there’s still issue on this environment, then it’s likely an OTA retression and already be reported on launchpad[4].

BTW, someone shared the OEM image on [forum][1], it could also be a reference to understand what be installed in Ubuntu edition machine. But it should just similar to what we manually built.

[18.04]:https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes?_ga=2.119646856.1425105764.1563813986-519412228.1563630648
[oem kernel]:https://launchpad.net/ubuntu/+source/linux-oem
[certification page]:https://certification.ubuntu.com/
[dpkg-l]:http://paste.ubuntu.com/p/TXFgTZMNnk/
[201810-26512]:https://certification.ubuntu.com/hardware/201810-26512/
[manifest of 18.04.2]:http://releases.ubuntu.com/18.04/ubuntu-18.04.2-desktop-amd64.manifest
[1]:https://askubuntu.com/questions/1136409/new-xps-13-9380-with-ubuntu-18-04-flicker-problems/1149630#1149630
# Reference
- https://ubuntu.com/about/release-cycle
