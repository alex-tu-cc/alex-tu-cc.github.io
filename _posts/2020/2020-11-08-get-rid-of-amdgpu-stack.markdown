---
layout: "post"
title: "Get rid of amdgpu stack"
date: "2020-11-08 13:00"
---

# Background
To support cutting edge AMD graphic card, it was necessary to install amdgpu stack released from [AMD website][].
But there's no way to online update the stack while regression is happening. e.g. 
 - The amdgpu dkms in stack build failed with latest kernel.
 - The mesa libraries carried by amdgpu conflict latest mesa libraries.
 - ... etc (anyway it's no way to fix regression if there's no way to online update.)

But it was hard request before 20.04 to include amdgpu stack in preloaded image.
So, this post is to share how we deal with the regressions on 18.04 on the precess rolling kernel from 5.0 OEM to 5.4 generic.



[AMD website]: https://www.amd.com/en/support/kb/faq/amdgpu-installation
