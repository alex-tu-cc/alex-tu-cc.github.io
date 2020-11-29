#---
#title: oem-experience
#subtitle:
#layout: post
#date: 2020-12-20
#categories: [_posts]
#tags:
#  - oem 
#---

# Background
Even it's already very easy to run Ubuntu on any X86 machine, it sometimes still struggling when some device (e.g. wifi, bt ..etc.) failed to work.
And also even we can just pick one of [Ubuntu certified machines][Ubuntu certification], but there still different software combinations.
For example, there are [severial kernels][kernels in focal] and [Nvidia driver versions][Nvidia driver versions in focal] in Focal archive.
The potential combinations could be {number of kernels in focal} X {number of nvidia driver versions in focal}.

And sometimes we still need DKMS for cutting-edge devices or functions before the needed patches be merged back to upstream.
Then, the combinations will be extended to {number of kernels in focal} X {number of nvidia driver versions in focal} X {DKMS on archive}.

Of cause people can just dig out the software combination from the Ubuntu certification report like what my previous post "[buy-a-certified-ubuntu-machine][]" said.  But it would be convenient if we have a single stable combination verified on target machine and people can get it easily.
(Let's call it OEM experience)

## OEM experience
Linux ecosystem offer you the flexibility to build your favor system.
So does the system built for [Ubuntu certification][]. 
 
As I mentioned in previous posts that users can get OEM experience through [oem meta][oem_meta].
It is not a restriction but trying to deliver the best verified stable status for you.  

## OEM meta
After Focal, the OEM meta package is used to turn your system to the environment passed [Ubuntu certification][].
The environment might include OEM kernel, 3rd party drivers (e.g. Fingerprint), the best setting for power consumption ...etc

The OEM meta will be installed automatically during Ubuntu installation if you agreed to install 3rd party software.

# Example
Let's take one certified machine as an example.
[ To be continue...]

[oem_meta]: https://www.phoronix.com/scan.php?page=news_item&px=Ubuntu-20.04-Certified-OEM-Exp
[oem certification]: https://ubuntu.com/blog/people-and-processes-behind-ubuntu-certified-devices
[kernels in focal]: https://qa.debian.org/madison.php?package=linux-oem-osp1+linux-oem+linux-image-oem-20.04+linux-oem-20.04-edge+linux-image-generic+linux-image-generic-hwe-20.04&table=ubuntu&a=&c=&s=focal+focal-proposed#
[kernels in focal]: https://qa.debian.org/madison.php?package=linux-oem-osp1+linux-oem+linux-image-oem-20.04+linux-oem-20.04-edge+linux-image-generic+linux-image-generic-hwe-20.04&table=ubuntu&a=&c=&s=focal+focal-proposed#
[Nvidia driver versions in focal]: https://paste.ubuntu.com/p/c7rrkxjjnZ/
[Ubuntu certification]: https://certification.ubuntu.com/
[buy-a-certified-ubuntu-machine]: http://alex-tu-cc.github.io/2019/07/buy-a-certified-ubuntu-machine/
