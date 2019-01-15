---
layout: "post"
title: "LVFS and fwupdmgr"
date: "2019-01-15 12:25"
---
[LVFS][](Linux Vendor Firmware Service) is a way to update firmware (e.g. BIOS, host TBT firmware, device TBT firmware ... etc.)
And the tool to do that is fwupdmgr which came from [fwupd](https://github.com/hughsie/fwupd), so this post is to share the experience on using fwupdmgr.


let's take [xps13][] with fwupd 1.0.9-0ubuntu2 as example,
```
$ fwupdmgr get-remotes 
http://paste.ubuntu.com/p/njkmfmcCdQ/

$ fwupdmgr refresh ; #get latest meta
Fetching metadata https://cdn.fwupd.org/downloads/firmware.xml.gz
Downloading…           [***************************************]
Fetching signature https://cdn.fwupd.org/downloads/firmware.xml.gz.asc

# then you can get /var/lib/fwupd/remotes.d/lvfs/metadata.xml.gz
e.g. http://paste.ubuntu.com/p/Ycg6RTWxjd/

$ fwupdmgr get-devices
http://paste.ubuntu.com/p/QS2MKC9cjk/

$ fwupdmgr get-updates
http://paste.ubuntu.com/p/VNXQtQMpbw/
```

In this case,``fwupdmgr get-updates``  get an availiable update "ID: com.dell.tbt4eeb9d07.firmware", which just match "Guid: 4eeb9d07" in ``fwupdmgr get-devices``. 
And ``fwupdmgr get-updates`` refer to /var/lib/fwupd/remotes.d/lvfs/metadata.xml.gz which downloaded by ``fwupdmgr refresh``

So, here are 2 check points.
 1. the device which is targeted to update firmware must present in ``fwupdmgr get-devices`` 
 2. the target firmware should preset in metadata.xml.gz if vendor uploaded it correctly.

Tips:
 - the firmware indexed by metadata.xml.gz can be downloaded manually, then flash it by fwupdmgr.
 - you can put private configuration in /etc/fwupd/remotes.d/ to get private firmware.(might need reboot let daemon include new configuration)
    - remember to check if the associated metadata.xml.gz get from the url in private configuration.
 - current using firmware version can be get by ``fwupdmgr get-devices``  as well.
 - thunderbolt device might can also be update by this way if it shows in ``fwupdmgr get-devices`` after plug it to thunderbolt slot.

[1] https://github.com/hughsie/fwupd/wiki/LVFS-Testing-remote

[LVFS]: https://fwupd.org/
[1]: https://goo.gl/dDi7Do
[xps13]: https://certification.ubuntu.com/desktop/models/?query=9370&category=Laptop&level=Any&vendors=Dell
