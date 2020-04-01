# Accu-Chek Solo RC scatter file

Read whole story here: https://medium.com/@sebastianmichalowski/internals-of-accu-chek-solo-remote-controller-a85d871e9a96

Device is using Android 4.2.2 Jellybean build JDQ39

## Accessing ADB

1. Use SPFlash to dump System partition.
2. Edit default.prop to enable ADB.

```
persist.service.adb.enable=1
persist.service.debuggable=1
persist.sys.usb.config=mtp,adb
```

3. Download firmware onto device using Fastboot.
4. Restart and have your console ready (you have just a few seconds before Roche software will boot up and take over USB so no more ADB)
5. Disable automatic bootup of pump software on device so your ADB shell will not be terminated:

```
./adb shell "mount -o remount,rw /system; mv /system/app/CustBootServiceSigned.apk /system/app/CustBootServiceSigned.apk.bak"
```

## Interesting services

All available as system apps under ```/system/app```

```
com.accu_chek.solo_m.rcapp.application.bootservice
com.accu_chek.solo_m.rcapp.application.continua
com.accu_chek.solo_m.rcapp.application.emwrservice
com.accu_chek.solo_m.rcapp.application.reminder
com.accu_chek.solo_m.rcapp.application.solompumpservice
com.accu_chek.solo_m.rcapp.presentation
```

## Setting correct time

Useful if you mess something with RTC.

```
./adb shell date -s `date +%Y%m%d.%H%M%S`
```

## Fastboot response

```
C:\Users\sim\Desktop\platform-tools>fastboot getvar all
(bootloader)    partition-size:fat: 515c0000
(bootloader)    partition-type:fat: fat
(bootloader)    partition-size:userdata: 32000000
(bootloader)    partition-type:userdata: ext4
(bootloader)    partition-size:cache: 17800000
(bootloader)    partition-type:cache: ext4
(bootloader)    partition-size:system: 28a00000
(bootloader)    partition-type:system: ext4
(bootloader)    partition-size:expdb: a00000
(bootloader)    partition-type:expdb: raw data
(bootloader)    partition-size:logo: 300000
(bootloader)    partition-type:logo: raw data
(bootloader)    partition-size:misc: 80000
(bootloader)    partition-type:misc: raw data
(bootloader)    partition-size:sec_ro: 40000
(bootloader)    partition-type:sec_ro: raw data
(bootloader)    partition-size:recovery: 600000
(bootloader)    partition-type:recovery: raw data
(bootloader)    partition-size:boot: 600000
(bootloader)    partition-type:boot: raw data
(bootloader)    partition-size:uboot: 60000
(bootloader)    partition-type:uboot: raw data
(bootloader)    partition-size:seccfg: 20000
(bootloader)    partition-type:seccfg: raw data
(bootloader)    partition-size:protect_s: a00000
(bootloader)    partition-type:protect_s: ext4
(bootloader)    partition-size:protect_f: a00000
(bootloader)    partition-type:protect_f: ext4
(bootloader)    partition-size:nvram: 500000
(bootloader)    partition-type:nvram: raw data
(bootloader)    partition-size:pro_info: 300000
(bootloader)    partition-type:pro_info: raw data
(bootloader)    partition-size:ebr1: 80000
(bootloader)    partition-type:ebr1: raw data
(bootloader)    partition-size:mbr: 80000
(bootloader)    partition-type:mbr: raw data
(bootloader)    partition-size:preloader: c00000
(bootloader)    partition-type:preloader: raw data
(bootloader)    kernel: lk
(bootloader)    product: ALTEK72_WE_JB3
(bootloader)    version: 0.5
all: Done!!
Finished. Total time: 0.124s
```
