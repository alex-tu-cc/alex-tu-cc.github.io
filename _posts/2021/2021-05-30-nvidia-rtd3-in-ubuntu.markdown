---
Title: Nvidia runtime pm (RTD3) in ubuntu
Layout: post
Date: 2021-05-30
Categories: [_posts]
Tags:
  - ubuntu
  - Nvidia
  - runtimepm
  - RTD3
---

# Background

從上一篇 [Nvidia runtime PM](http://alex-tu-cc.github.io/2020/11/Nvidia-runtimepm/). Nvidia 從 r450開始對某些卡支持 runtime PM.
那某些是那些? 可以在 Nvidia 官方driver 自帶的 [supported-gpus.json](https://alex-tu-cc.github.io/get-nvidia-supported-gpus-json/) 看到。
例如裡面指出 DID 0x1E04，有支持 runtimepm:

```
        {
            "devid": "0x1E04",
            "name": "GeForce RTX 2080 Ti",
            "features": [
                "dpycbcr420",
                "dpgsynccompatible",
                "hdmi4k60rgb444",
                "hdmigsynccompatible",
                "geforce",
                "runtimepm",
                "vdpaufeaturesetJ"
            ]
        },
```

另外 runtime pm 在 Nvidia driver 中預設是不開啟的，需要在 load driver 的時後給它 `NVreg_DynamicPowerManagement=0x02`去開啟 runtime PM 功能。
也就是說，如果你想要讓 Nvidia dGPU 跑 runtimepm 的話，你需要問 Nvidia driver 有沒有支持 runtime pm（by _supported-gpus.json_) 再跟 Nvidia Driver 說它可以跑 runtime pm

這聽起來很麻煩，還好 Ubuntu 的 gpu manager ，跟 prime-select 目前幫忙作掉這些事了。這篇就來分享具體是怎麼作的。

# What is prime-select

Ubuntu 提供的 prime-select command 是用來切換要用 Intel rendering 還是 Nvidia rendering。你們在 nvidia-settings 的 UI 上看到的 performance mode，powersaving mode 就是用 prime-select 來切換的。
那個 UI 的選項是 Ubuntu 另外加進去的，所以從官網抓 .run 檔下來安裝是看不到這個選項的。

![nvidia-prime-select-ui]({{ site.baseurl }}/images/2021/nvidia-prime-select-ui.png)

在 Nvidia support runtimepm 之前當 prime-select Nvidia後，dGPU 會一直醒著。
換言之，prime-select intel 就是用 Intel rendering , 那 dGPU 沒事就可以去睡，以達到省電的目地。但 Nvidia 還不支持 runtime pm ，dGPU 怎麼睡? 它是在 prime-select intel 的下次動開機，直接強迫 Nvidia dGPU 不load 任何 driver，而一個 PCI device 沒用 driver 在使用它，自動就會去睡了。

prime-select 之前只有 nvidia 跟 intel 兩個選項。

冰雪聰明的你應該就看出來前面說的 performance mode 就是 `prime-select nvidia`, powersaving mode 就是 `prime-select intel` 對吧?


後來 prime-select 多了 _on-demand_ 的選項, 讓使用者可以自已決定這次的 rendering 是要用 Nvidia 還是 iGPU (intergrated GPU, default), 主要有有些人想要用 Nvidia dGPU 來作其他用途 （e.g. AI/ML）。但當時(Nvidia < r450) 你選了 on-demand ，Nvidia 還是會一直醒著秏電。(可以在背景一直挖礦?) 在 `prime-select on-demand ` 後，使用者如果想要用 dGPU randering，就要另外加環境變數 `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia`。這裡就跟 AMD 很像(`DRI_PRIME=1`)，反正大家都一樣。

### Launch application in Nvidia dGPU rendering.

![launch-in-dgpu.png]({{ site.baseurl }}/images/2021/launch-in-dgpu.png)

### Execute shell in Nvidia dGPU rendering.

```
# on Nvidia machine with dGPU supported runtimepm and done prime-select on-demand
$ DISPLAY=:0 glxinfo | grep "renderer string"
OpenGL renderer string: Mesa Intel(R) Graphics (RKL GT1)

$ DISPLAY=:0 __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep "renderer string"
OpenGL renderer string: GeForce GTX 1650/PCIe/SSE2
```

# What is runtime pm

又叫作 RTD3(runttime D3)，就是 device 不用的的時後，driver 會讓 device 去睡，以達到省電的目地。大部份的 device 均已支持這個功能，AMD dGPU 也很久之前就支持了。要看 Device 有沒有睡可以用 `powertop` 去看。

```
# make sure the driver got the parameter needed for runtime pm.
$ cat /proc/driver/nvidia/params | grep DynamicPowerManagement
DynamicPowerManagement: 2

# check if Nvidia get sleep when it's no in using.
$ sudo powertop

# move to "Device stats" tab
Usage     Device name
0.0%        PCI Device: NVIDIA Corporation Device
```

# What’s the relationship between on-demand mode and runtime pm?

如前述，prime-select on-demand 是你叫它用 Nvidia rendering 它才會用 Nvidia，不然的話就是用 iGPU 畫，跟睡不睡無關。在 Nvidia r450 之後如果 dGPU 有支持 runtimepm ，感覺 dGPU 沒在畫的時後就去睡覺很合理對嗎?但 Nvidia 沒打算讓 runtime pm default enabled，所以你想讓它沒畫的時後去睡，就必需要問 Nvidia driver 有沒有支持 runtime pm（by [supported-gpus.json](https://alex-tu-cc.github.io/get-nvidia-supported-gpus-json/)) 再跟 Nvidia Driver 說它可以跑 runtime pm 。

因為 大部份的 OEM 都需要過 e-star，而且沒用的時後可以睡就去睡也滿合常理，所以目前 Ubuntu 的設計是在選了 on-demand 情況下，能睡就叫 dGPU 去睡。

所以這裡再強調一遍
```
on-demand 不等於 runtime pm, 有支持 runtime pm 的 Nvida dGPU 在 Ubuntu 才會自動去睡
on-demand 不等於 runtime pm, 有支持 runtime pm 的 Nvida dGPU 在 Ubuntu 才會自動去睡
on-demand 不等於 runtime pm, 有支持 runtime pm 的 Nvida dGPU 在 Ubuntu 才會自動去睡
```

很重要，所以說三次!!

# What did Ubuntu do to adopt Nvidia runtime pm?

gpu manager 在每次開機的時後會去看一下 [supported-gpus.json](https://alex-tu-cc.github.io/get-nvidia-supported-gpus-json/) 裡面目前使用的 dGPU 有沒有支持 runtime pm，有的話會生成 /run/nvidia_runtimepm_supported, 詳細的 log 可以在 /var/log/gpu-manager.log 中找到。
然後 prime-select on-demand 的時後會把`NVreg_DynamicPowerManagement=0x02`這個 module parameter 放到 /lib/modprobe.d/nvidia-runtimepm.conf, 然後再重新生成 initrd ，如此就可以達成下次開機load nvidia driver 的時後，enable runtime pm。再加上 on-demand mode 平常是 default iGPU rendering, 所以在這樣的情況下 dGPU 沒在用就可以自已去睡覺。

```
# gpu-manager confirmed dGPU supporting runtime pm
$ grep runtime /var/log/gpu-manager.log 
Is nvidia runtime pm supported for "0x1f99"? yes
Trying to create new file: /run/nvidia_runtimepm_supported
Is nvidia runtime pm enabled for "0x1f99"? yes
Trying to create new file: /run/nvidia_runtimepm_enabled

# the module parameter be generated by prime-select on-demand when target dGPU supported runtime pm
$ cat /lib/modprobe.d/nvidia-runtimepm.conf
options nvidia "NVreg_DynamicPowerManagement=0x02"

# then the module parameter be updated to initrd
$ unmkinitramfs /boot/initrd.img .
$ cat main/usr/lib/modprobe.d/nvidia-runtimepm.conf
options nvidia "NVreg_DynamicPowerManagement=0x02"

# from next boot, the module parameter be used by Nvidia driver
$ cat /proc/driver/nvidia/params | grep DynamicPowerManagement
DynamicPowerManagement: 2
```

詳細source code:
 - gput manager in ubuntu-drivers-common: https://github.com/tseliot/ubuntu-drivers-common.git
 - prime-select: https://github.com/tseliot/nvidia-prime.git

# What else be benefited by Nvidia runtime pm?

最近有很多筆電自帶的 HDMI/DP port 在硬體設計上，直接接到 dGPU 上面，換言之，常 dGPU 睡覺的時後，HDMI/DP port 就失去作用，也就是說 prime-select intel 之後(如前述，沒有任何driver 使用dGPU)，你的 HDMI/DP port 當然就不能用了。
在 Nvidia runtime pm 之後，如果你的 Nvidia dGPU 有支持 runtime pm，你用 prime-select on-demand 後，因為 enable runtime pm 的 Nvidia driver 還在，所以當有人插入 HDMI/DP 的時後，driver 收到 interrupt 會自動醒來，這樣 HDMI/DP 就能正常使用。

所以，如果你的筆電是這種 hardware 設計的，剛好 Nvidia 有把你的 dGPU 放到 [supported-gpus.json](https://alex-tu-cc.github.io/get-nvidia-supported-gpus-json/), 那你裝最新的 nvidia driver (> r450)，就可以同時享有沒插 HDMI/DP 的時後省電，插上就又馬上可以使用。

最後，如果 Nvidia 最後跟 AMD 一樣都 default enable runtim pm for all dGPU，那就不用那麼多 workaround 去 prime-select 了。

# Reference
 - https://download.nvidia.com/XFree86/Linux-x86_64/450.51/README/dynamicpowermanagement.html

// tips: {{ site.baseurl }}


