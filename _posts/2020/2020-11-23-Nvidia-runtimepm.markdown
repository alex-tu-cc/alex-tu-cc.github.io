---
layout: "post"
title: "Nvidia runtime PM"
date: "2020-11-23 02:00"
---

# Background
 - The latest Nvidia drivers are maintained on Ubuntu archive. (e.g. [nvidia-graphics-drivers-450][])
 - Ubuntu's [nvidia-prime][] provided [a convenient interface][prime-select] selecting [on-demand][], nvidia or intel mode.
    - User can easily [assign Nvidia rendering][1] to any application
 - Nvidia graphic is consuming power even it is idle in on-demand mode.
 - Nvidia driver starts to support runtime PM on part of graphic cards.
    - The supported graphic product ids are collected in supported-gpus.json which released from the driver(.run binary) released by Nvidia).
    - For example, the [supported-gpus.json](https://paste.ubuntu.com/p/mdrg2jYKkt/) from Nvidia-driver-450
        - The latest supported-gpus.json : https://alex-tu-cc.github.io/get-nvidia-supported-gpus-json/

# The automatical Nvidia runtime PM in on-demand mode.
With [nvidia-prime][], [gpu manager][] and Nvidia drivers, Ubuntu able to detect if runtime PM is supported on your graphic card then operates properly.   
So that, Nvidia getphic can sleep well when idle in on-demand.  
[My script][3] can check if it behaves as we expected.(say, rendering as expected, sleep as expected)  

The feature was landed to groovy but not yet to Focal.  
Before the SRU done on Focal, you can get the debian package on [my PPA][2] which monitoring the 2 developing git trees for [nvidia-prime][nvidia-prime ts] and [gpu manager][].  



[nvidia-graphics-drivers-450]: https://launchpad.net/ubuntu/+source/nvidia-graphics-drivers-450
[prime-select]: https://www.oceanlinux.club/2019/08/nvidia-43517-linux-beta-driver-adds.html
[nvidia-prime]: https://launchpad.net/ubuntu/+source/nvidia-prime
[nvidia-prime ts]: https://github.com/tseliot/nvidia-prime
[on-demand]: https://askubuntu.com/questions/1201072/how-nvidia-on-demand-option-works-in-nvidia-x-server-settings
[gpu manager]: https://github.com/tseliot/ubuntu-drivers-common/blob/master/share/hybrid/gpu-manager.c
[1]: https://www.newsbreak.com/news/1560657803815/the-linux-desktop-entry-specification-gets-a-way-to-automatically-use-a-discrete-gpu-merged-into-gnome
[2]: https://launchpad.net/~alextu/+archive/ubuntu/nvidia-drivers-testing
[3]: https://code.launchpad.net/~alextu/plainbox-provider-pc-sanity/+git/plainbox-provider-pc-sanity/+merge/394049
# Reference:
 - https://download.nvidia.com/XFree86/Linux-x86_64/435.17/README/dynamicpowermanagement.html

