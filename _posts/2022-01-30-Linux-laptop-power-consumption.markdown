---
Title: Your Linux laptop might consume less power than a bulb.
Layout: post
Date: 2022-01-30
Categories: [_posts]
Tags:
  - ubuntu
  - runtimepm
  - RTD3
  - power__consumption
---

Do you know the power consumption of your Linux laptop is less than a bulb? Let’s talk a bit about it.

The compliances related to power consumption we encountered during Linux enablement are mostly been energy star(e-sta)[1] and ErP[2] (BTW, a machine that passed e-star usually also passed ErP).

Because e-star compliance is to check the value translated from a formula with the measured power consumption under each power scenario. So we used to check the e-star compliance by energy-tools [3] with the measured consumption from a power meter. (That energy-tools is maintained by my team, enjoy it.) Normally the consumption of short idle is less than 6Wh, long idle less than 1Wh on a laptop that passed e-star. And e-star would request lower consumption for each new version (It’s version 8 now).

Now, it can be even easier to check the Intel CPU power state for each power scenario because a machine that can get PC10 used to pass e-star. So, a quick way is to check if the system can successfully get PC10 and s0ix during long idle or suspend to idle. (Also refer to power-management/cpu-low-power-idle and power-management/system-low-power-idle in checkbox [4] for the test scripts committed by my team)

It sounds so good, then why don’t we just make it on every machine? It counts on each hardware and software component vendor to make the device get into sleep during idle so that the CPU can get PC10 state. Most of the time, IHV tends to disable the runtime suspend[5][6] related features by default to work around issues introduced by the non-mature driver or device firmware. That’s also part of our major work to deal with during enablement for the e-star requirement. One target is to enable the runtime suspend feature by default on those verified devices. ([The Nvidia RTD3 feature that I introduced in another post is one of the cases]({{ site.baseurl }}/2021/05/nvidia-rtd3-in-ubuntu/))

If someone would like to improve the power consumption of their laptop. A quick way is to try the powertop command to find out and enable the runtime suspend feature for each device. If you are lucky that no function be impacted, then the easiest way is to install TLP[7] so that TLP daemon can help to enable the runtime suspend for each device for each boot.

Power consumption is a complicated topic that involved multiple parts:
- Hardware:  
Some devices got a power leak during S5 that needs a driver workaround to turn it off.

- Firmware:  
Some device firmware does not handle well on runtime suspend which might cause the device to be dropped after system suspend.

- Driver:  
Some IHV does not support runtime suspend in Linux driver that blocks CPU PC 10.

- OS:  
It’s a trade-off that some OS does not aggressively improve power consumption so that might be stabler on problematic hardware.

It’s also a challenge to keep the power consumption experience after shipping when hardware vendors do not cover enough Linux verification for each firmware update. I can just share a bit overview of this topic in this short post. Welcome to leave your message if you would like to know more about the detail.

BTW, Canonical(Ubuntu) is hiring (https://grnh.se/caf5ff9a1). Join us if you would like to make Linux better!

[1] https://www.energystar.gov/  
[2] https://en.wikipedia.org/wiki/Energy-related_products  
[3] https://snapcraft.io/energy-tools  
[4] https://git.launchpad.net/plainbox-provider-checkbox/tree/units/power-management/jobs.pxu  
[5] https://www.kernel.org/doc/Documentation/usb/power-management.txt  
[6] https://www.kernel.org/doc/html/latest/power/pci.html  
[7] https://github.com/linrunner/TLP  
