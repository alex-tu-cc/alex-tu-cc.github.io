---
layout: "post"
title: "Secureboot in OEM image(Dell)"
date: "2020-11-08 13:00"
---

# Background
 - Stock Ubuntu will trigger interactive process to enroll MOK while user install a DKMS. (refer to [DKMS_SecureBootOn1804][] and [moke_enroll_stock_ubuntu][])
 - OEM image might preload DKMS by request for functions (e.g. Nvidia, iwlwifi..etc).
 - There are 3 stages in [dell-recovery][] which is the fundation of preloaded Dell OEM image.
    - __Stage 1__: Boot from external recovery USB disk.
        - Happens in factory or
        - User doing freshing install by USB disk.
    - __Stage 2__: Boot from local disk recovery partition.
        - Happens in factory or
        - Next boot of user doing fresh installation by USB disk or
        - User execute dell-recovery from local disk manually.
    - __Stage 3__: OOBE (boot from system partition)
        - Happens in user side only.
    - First login.


After 20.04, OEM image enabled secureboot by default after OOBE. So, the scenarios below are captured:
 1. Fresh install to the factory default from USB disk.
    1. Secureboot was disabled like the factory process. (Let's call it  __rc:USB:no-sb__ below)
    2. Secureboot was enabled that user did not disable it before the process. (Let's call it __rc:USB:sb__)
 2. Recovery to the factory default from local storage.
    1. Secureboot was disabled like the factory process.(Let's call it  __rc:local:no-sb__)
    2. Secureboot was enabled that user did not disable it before the process.(Let's call it __rc:local:sb__)

# Challenages
 - The factory preloading does not accept interactive process. 
    - Say, DKMS in the scenario rc:USB:no-sb should be done quietly
 - User might doing the recovery on a machine enabled secureboot already(rc:USB:sb or rc:local:sb) .
    - Which means the interactive MOK enrolling must be done on the case that user replaced internal storage.

# Solution

All the implementation could be found in plaintext form in each 20.04 OEM image.  
Most of them were wrote in bash script to adopt tools in [DKMS_SecureBootOn1804][] to factory process with [dell-recovery][].

These are involved files in recovery partition:
```
.
├── scripts
│   └── chroot-scripts
│       ├── fish
│       │   └── 01-secureboot-prepare-env.sh
│       └── os-post
│           └── 99-secureboot-pre-enroll.sh
└── secureboot
    ├── enable-secureboot
    ├── enroll-key-enable-secureboot
    ├── enroll-key-in-stage21.sh
    ├── loadefi.efi
    ├── load_efi.sh
    ├── MokEnrollKey.efi
    ├── mok_enroll_key.sh
    ├── mok_testkernel_key.sh
    ├── system
    │   ├── enroll-mok-in-stage21
    │   ├── enroll-mok-in-stage21.service
    │   └── oem-config.service.d
    │       └── local.conf
    └── UbuntuSecBoot.efi
```

Let review the operations in each scenario below: 
## rc:USB:no-sb
This environment is just like what it is expected in factory.
The installed DKMS will be signed by a runtime generated MOK.
The MOK will be enrolled to BIOS and secureboot will be enabled in the end of OOBE automatically without the need of user interactive.

In the next boot of OOBE, there is message printed on the left-top coner of screen shows the process of enabling secureboot for reference:
![enabling secure boot](https://i.imgur.com/yuKJ6Tu.png)

## rc:USB:sb
Most of this scenario should happens on user replaced internal storage.
So, it's necessary to create a new MOK and enroll it to BIOS.
Which means we can not prevent user interactive from Stage 2 because the secureboot is enabled before DKMS installation.
The experience of enrolling MOK will same as stock Ubuntu like [DKMS_SecureBootOn1804][] and [moke_enroll_stock_ubuntu][] shared.

BTW, if there's no DKMS needed for target machine, then of cause there will no MOK enrolling needed.

## rc:local:no-sb
Sometime, user could disable secureboot from BIOS then doing dell-recovery.
This case will just go the same path of rc:USB:no-sb.

## rc:local:sb
This is a normal case that user would like to reset system to factory default.
In this case, the MOK is enrolled so we don't need to enroll it again.
The extra operation is to [keep the old MOK](https://github.com/dell/dell-recovery/pull/81) from system so that we don't need to re-generate MOK again.


# Others
After 20.04, the [OEM experience][oem_meta] has been adopted to stock Ubuntu, so user can based on stock Ubuntu experience to enjoy the effort from OEM image without caring about the complixity of scenarios above. Clicking on the installation of 3rd partiy stuff to enjoy OEM experience because of some of them came from DKMS (e.g. nvidia driver, FP driver ..etc.) 


[DKMS_SecureBootOn1804]: https://wiki.canonical.com/IvanHu/DKMS_SecureBootOn1804
[moke_enroll_stock_ubuntu]: https://wiki.ubuntu.com/Dell?action=AttachFile&do=view&target=mok_enroll_stock_ubuntu_2.pdf
[oem_meta]: https://www.phoronix.com/scan.php?page=news_item&px=Ubuntu-20.04-Certified-OEM-Exp
[dell-recovery]: https://github.com/dell/dell-recovery

