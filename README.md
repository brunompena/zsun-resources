
# Introduction

This repository contains all the information needed to update the firmware of Zsun-SD100 to OpenWrt version 22.03.

## Main features

These are the main features of this release when compared to other community releases:
 - Firmware image based on OpenWrt 22.03
	- *the latest and greatest release*
 - Recovery image based on OpenWrt 19.07 (no more bricking!)
	- *the old-stable branch which introduced the new ath79 architecture*
	- *read-only partition with so it can't be damaged*
 - Updated u-boot to accommodate future OpenWrt releases
	- *changed kernel boot offset to the start of the flash memory*
 - Built with reviewed and tweaked community patches
	- *all code has been reviewed for usefulness*
	- *deprecated, non-functional or non-relevant code was removed*

## Credits
None of this would have been possible without the work of many other community members that have contributed throughout several years to have OpenWrt running smoothly on Zsun-SD100.

My patches are heavily based on the community releases from the following members:
 - [Emeryth](https://github.com/Emeryth) : who made the very first OpenWrt port of Chaos Calmer 15.05 to Zsun-SD100 and whose patches are still heavily present on the most current OpenWrt releases
 - [puteulanus](https://github.com/puteulanus) : for its implementation of the brilliant recovery partition concept! (this has saved me **countless** times! Kudos!)

## Disclaimer

These instructions and software are provided "AS IS", without support or warranty of any kind, express or implied.

I take no responsability if you damage your device - proceed at your own risk!

# Update guide

The update process for Zsun-SD100 can be summarized as follows:
 1. Identify the current firmware
 2. Download the required update files
 3. Prepare the device to be updated
 4. Flash the update files
 5. Wait for the update process to complete
 6. Verify recovery image
 7. Update to the latest versions

## Identify current firmware

Login to the Zsun-SD100 and execute the following command:

```
md5sum /proc/mtd
```

Then compare its output with the following table to identify the firmware:

| md5sum | Version |
| ------ | ------- |
| c10cba35a7eea4ddde8534dd0c9a48fa | Original Firmware |
| fb97079eb3bd5feffb080e5be71574ea | Emeryth 15.05.0 Firmware |
| 471167cedc5bdaa9fb56d97f52f662d7 | maurer 17.01.4 Firmware |
| 6ac5a0c1c919bd49d56205c9abd8b51d | puteulanus 17.01.6 Firmware |
| 46cc21d44db073f99efe9ecd6437d25a | puteulanus 18.06.1 Firmware |
| 37e381cbb2c9e7660442e1525b3c3c58 | puteulanus 18.06.1_rec Firmware |

If you cannot find your version in the above table then **STOP! DO NOT CONTINUE!** (unless you really know what you're doing)

## Download update files

Download the corresponding files for your firmware version:

| Version | Update Image Parts |
| ------- | ------------------ |
| Original Firmware (part1) | [zsun-sd100.update.variant1.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part1.bin) |
| Original Firmware (part2) | [zsun-sd100.update.variant1.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part2.bin) |
| Emeryth 15.05.0 Firmware (part1) | [zsun-sd100.update.variant1.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part1.bin) |
| Emeryth 15.05.0 Firmware (part2) | [zsun-sd100.update.variant1.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part2.bin) |
| maurer 17.01.4 Firmware (part1) | [zsun-sd100.update.variant1.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part1.bin) |
| maurer 17.01.4 Firmware (part2) | [zsun-sd100.update.variant1.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part2.bin) |
| puteulanus 17.01.6 Firmware (part1) | [zsun-sd100.update.variant1.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part1.bin) |
| puteulanus 17.01.6 Firmware (part2) | [zsun-sd100.update.variant1.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant1.part2.bin) |
| puteulanus 18.06.1 Firmware (part1) | [zsun-sd100.update.variant2.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant2.part1.bin) |
| puteulanus 18.06.1 Firmware (part2) | [zsun-sd100.update.variant2.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant2.part2.bin) |
| puteulanus 18.06.1_rec Firmware (part1) | [zsun-sd100.update.variant3.part1.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant3.part1.bin) |
| puteulanus 18.06.1_rec Firmware (part2) | [zsun-sd100.update.variant3.part2.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.update.variant3.part2.bin) |

## Prepare device for update

### Copy files to Zsun-SD100

Copy the update image parts to the `/tmp` directory of your Zsun-SD100 using one of the many methods available (tftp, ftp, scp, nc, ...).

Then verify the integrity of your update files according to the table below:

```
md5sum /tmp/zsun-sd100.update.variant?.part?.bin
```

| Update Image Parts | md5sum |
| ------------------ | ------ |
| zsun-sd100.update.variant1.part1.bin | aef8bc50aa08f58e16752716a94ea36f |
| zsun-sd100.update.variant1.part2.bin | 3473e84fb47442a75a8e20b1df482fde |
| zsun-sd100.update.variant2.part1.bin | 5def5e6b5e6f61fb10a784f971329b50 |
| zsun-sd100.update.variant2.part2.bin | bad94959877872b2a3d75d6edd04b66f |
| zsun-sd100.update.variant3.part1.bin | 046a4eec4c7b2db6e5bd2e0c2141d505 |
| zsun-sd100.update.variant3.part2.bin | 75a8421899f881c1b229405b71174735 |

If your files md5sum don't match the ones you see on the table then **STOP! DELETE THE FILES AND START OVER!**

### Remount filesystems as read-only 

Remount the main filesystems as read-only to avoid data corruption:

 - For the *Original Firmware* execute:
```
mount -o remount,ro /dev/root /
```
 - For all others execute:
```
mount -o remount,ro /
mount -o remount,ro /overlay
```

### Backup mtd utility to memory

Copy the mtd utility to memory so it's always available throughout this process:

 - For the *Original Firmware* execute:
```
cp /sbin/mtd_write /tmp
```
 - For all others execute:
```
cp /sbin/mtd /tmp
```

### Change to memory filesystem

The final step before starting to flash is to change to the memory filesystem:

```
cd /tmp
```

## Flash binaries

**<HERE BE DRAGONS\>**

YOU ARE ENTERING THE DANGER ZONE!

I TAKE NO RESPONSIBILITY IF YOU BRICK YOUR DEVICE!

PROCEED AT YOUR OWN RISK!


You are now ready to start flashing your device!
To be on the safer side, you'll start by flashing the kernel (part2), and only then the rootfs (part1).

 - If you're running the *Original Firmware* then execute:
```
./mtd_write write zsun-sd100.update.variant?.part2.bin /dev/mtd3
./mtd_write -r write zsun-sd100.update.variant?.part1.bin /dev/mtd2
```
 - For all others execute:
```
./mtd write zsun-sd100.update.variant?.part2.bin /dev/mtd4
./mtd -r write zsun-sd100.update.variant?.part1.bin /dev/mtd2
```
**</HERE BE DRAGONS\>**

## Wait for update process to complete

If everything goes according to plan your device will go through the following steps:
 1. Automatic reboot after flashing the update image
 2. Boot to the update image
	- *will automatically flash the new u-boot*
	- *will automatically flash the recovery image (overwriting the update kernel)*
	- *will automatically reboot once done*
 3. Boot to the main firmware
	- *will automatically initialize its rootfs_data (overwriting the update rootfs)*

While the update image is running you'll see a WiFi SSID: ***Zsun is Updating! Please wait...***.

Once the LED on your Zsun-SD100 stops flashing you should see a new WiFi SSID: ***OpenWrt***.

The entire process should take about 2min - be patient and wait until it finishes.

## Verify recovery image

Devices like the Zsun-SD100 are very unforgiving when it comes to mistakes, making it very easy to brick the device.

This is why it is **imperative to make sure your recovery image is in good conditions before starting to customize your main firmware**.

### Check recovery image integrity

During the flash of the update image parts you have overwritten all of the data of the operating system that was running at that time, and although precautions were taken to avoid data corruption *there's still a small chance of this happening*.

Check the integrity of the recovery image by running and comparing the output of the following command:
```
md5sum /dev/mtd6
```

| MTD Partition | md5sum |
| ------------- | ------ |
| /dev/mtd6 | b8e5722762408a2b0520a95189e3e4bc |

If your md5sum matches then your recovery image was successfully flashed and you may continue to [Recovery Image](#recovery-image) if you want to test it.

If your md5sum doesn't match the one you see above then your recovery image is corrupted and you must reflash it.

### Reflash recovery image

Reflashing the recovery image is a very straight forward process:
 1. Download the mtd-rw kernel module
 2. Download the recovery image
 3. Flash the recovery image

#### Download the mtd-rw kernel module

The recovery image is stored on a read-only partition, so before flashing it you need to change the partition to read-write.

This is done by installing and loading the mtd-rw kernel module that changes all partitions to read-write.

Download the mtd-rw kernel module for your OpenWrt version:

| Version | mtd-rw kernel module |
| ------- | ------------------ |
| OpenWrt 19.07.0-rc1 | [kmod-mtd-rw_4.14.151+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/kmod-mtd-rw_4.14.151+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.0-rc2 | [kmod-mtd-rw_4.14.156+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc2/kmod-mtd-rw_4.14.156+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.0 | [kmod-mtd-rw_4.14.162+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.0/kmod-mtd-rw_4.14.162+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.1 | [kmod-mtd-rw_4.14.167+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.1/kmod-mtd-rw_4.14.167+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.2 | [kmod-mtd-rw_4.14.171+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.2/kmod-mtd-rw_4.14.171+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.3 | [kmod-mtd-rw_4.14.180+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.3/kmod-mtd-rw_4.14.180+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.4 | [kmod-mtd-rw_4.14.195+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.4/kmod-mtd-rw_4.14.195+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.5 | [kmod-mtd-rw_4.14.209+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.5/kmod-mtd-rw_4.14.209+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.6 | [kmod-mtd-rw_4.14.215+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.6/kmod-mtd-rw_4.14.215+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.7 | [kmod-mtd-rw_4.14.221+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.7/kmod-mtd-rw_4.14.221+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 19.07.8 | [kmod-mtd-rw_4.14.241+git-20160214-1_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/19.07.8/kmod-mtd-rw_4.14.241+git-20160214-1_mips_24kc.ipk) |
| OpenWrt 21.02.0-rc1 | [kmod-mtd-rw_5.4.111+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.0-rc1/kmod-mtd-rw_5.4.111+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.0-rc2 | [kmod-mtd-rw_5.4.119+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.0-rc2/kmod-mtd-rw_5.4.119+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.0-rc3 | [kmod-mtd-rw_5.4.124+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.0-rc3/kmod-mtd-rw_5.4.124+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.0-rc4 | [kmod-mtd-rw_5.4.137+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.0-rc4/kmod-mtd-rw_5.4.137+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.0 | [kmod-mtd-rw_5.4.143+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.0/kmod-mtd-rw_5.4.143+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.1 | [kmod-mtd-rw_5.4.154+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.1/kmod-mtd-rw_5.4.154+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.2 | [kmod-mtd-rw_5.4.179+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.2/kmod-mtd-rw_5.4.179+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.3 | [kmod-mtd-rw_5.4.188+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.3/kmod-mtd-rw_5.4.188+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.4 | [kmod-mtd-rw_5.4.215+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.4/kmod-mtd-rw_5.4.215+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 21.02.5 | [kmod-mtd-rw_5.4.215+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/21.02.5/kmod-mtd-rw_5.4.215+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc1 | [kmod-mtd-rw_5.10.111+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc1/kmod-mtd-rw_5.10.111+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc2 | [kmod-mtd-rw_5.10.116+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc2/kmod-mtd-rw_5.10.116+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc3 | [kmod-mtd-rw_5.10.116+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc3/kmod-mtd-rw_5.10.116+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc4 | [kmod-mtd-rw_5.10.120+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc4/kmod-mtd-rw_5.10.120+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc5 | [kmod-mtd-rw_5.10.127+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc5/kmod-mtd-rw_5.10.127+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0-rc6 | [kmod-mtd-rw_5.10.134+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0-rc6/kmod-mtd-rw_5.10.134+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.0 | [kmod-mtd-rw_5.10.138+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.0/kmod-mtd-rw_5.10.138+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.1 | [kmod-mtd-rw_5.10.146+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.1/kmod-mtd-rw_5.10.146+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.2 | [kmod-mtd-rw_5.10.146+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.2/kmod-mtd-rw_5.10.146+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.3 | [kmod-mtd-rw_5.10.161+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.3/kmod-mtd-rw_5.10.161+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.4 | [kmod-mtd-rw_5.10.176+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.4/kmod-mtd-rw_5.10.176+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.5 | [kmod-mtd-rw_5.10.176+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.5/kmod-mtd-rw_5.10.176+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.6 | [kmod-mtd-rw_5.10.201+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.6/kmod-mtd-rw_5.10.201+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 22.03.7 | [kmod-mtd-rw_5.10.221+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/22.03.7/kmod-mtd-rw_5.10.221+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.0-rc1 | [kmod-mtd-rw_5.15.114+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.0-rc1/kmod-mtd-rw_5.15.114+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.0-rc2 | [kmod-mtd-rw_5.15.118+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.0-rc2/kmod-mtd-rw_5.15.118+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.0-rc3 | [kmod-mtd-rw_5.15.127+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.0-rc3/kmod-mtd-rw_5.15.127+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.0-rc4 | [kmod-mtd-rw_5.15.132+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.0-rc4/kmod-mtd-rw_5.15.132+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.0 | [kmod-mtd-rw_5.15.134+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.0/kmod-mtd-rw_5.15.134+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.1 | [kmod-mtd-rw_5.15.137+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.1/kmod-mtd-rw_5.15.137+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.2 | [kmod-mtd-rw_5.15.137+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.2/kmod-mtd-rw_5.15.137+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.3 | [kmod-mtd-rw_5.15.150+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.3/kmod-mtd-rw_5.15.150+git-20160214-2_mips_24kc.ipk) |
| OpenWrt 23.05.4 | [kmod-mtd-rw_5.15.162+git-20160214-2_mips_24kc.ipk](https://github.com/brunompena/zsun-resources/releases/download/23.05.4/kmod-mtd-rw_5.15.162+git-20160214-2_mips_24kc.ipk) |

Copy the file to the `/tmp` directory of your Zsun-SD100 and then use the following command to install it:
```
opkg install /tmp/kmod-mtd-rw_*.ipk
```

Finally, load the mtd-rw kernel module to change all partitions to read-write:
```
insmod $(find /lib/modules/ -name mtd-rw.ko) i_want_a_brick=1
```

#### Download the recovery image

Download the recovery image:

| Version | Recovery Image |
| ------- | -------------- |
| Recovery Image 18.06.5 | [zsun-sd100.recovery.bin](https://github.com/brunompena/zsun-resources/releases/download/19.07.0-rc1/zsun-sd100.recovery.bin) |

Then copy it to the `/tmp` directory of your Zsun-SD100 and check its md5sum:
```
md5sum /tmp/zsun-sd100.recovery.bin
```
| Recovery Image | md5sum |
| -------------- | ------ |
| zsun-sd100.recovery.bin | b8e5722762408a2b0520a95189e3e4bc |

If your md5sum doesn't match the one you see on the table then **delete the file and download it again!**

#### Flash the recovery image

You are now ready to reflash your recovery image!

Execute the command:
```
mtd write /tmp/zsun-sd100.recovery.bin recovery
```

Once it finishes go back to the [Check recovery image integrity](#check-recovery-image-integrity) section to verify its integrity once again.

## Flash the latest versions

The update image includes OpenWrt 19.07.0-rc1 as the main firmware and OpenWrt 18.06.5 as the recovery firmware, both of which are already outdated.

You may upgrade to the latest versions found on [Releases](https://github.com/brunompena/zsun-resources/releases) as long as you follow the release instructions to upgrade **without** preserving any settings!

# Recovery Image

You now have a recovery image that you may boot if you ever get locked out of your main firmware.

*The recovery image is stored on a read-only partition so it cannot be (easily/accidently) damaged.*

## Boot to recovery

To boot the recovery image:
 1. Disconnect your device from USB
 2. Eject the SD card but do not remove it (you'll need to push it back in during the initial boot process)
 3. Connect your Zsun to the USB
 4. When the LED flashes for the first time, immediately insert the SD Card<sup>1</sup> to boot into the recovery image

If you successfully triggered the recovery image then after a few seconds you'll see WiFi SSID: ***Zsun Recovery***

<sup>1</sup>*You don't need to insert it completely, just pushing it in - without locking it in place - for about one to two seconds should be enough.*

## Mounting main firmware

A very useful feature of the recovery image is that allows you to mount your main firmware so you may fix any configuraton issues.

This is done using the `mount-firmware` script included on the recovery image.

To mount your main firmware execute:
```
# For Recovery Image 18.06.5:
mount_firmware

# For Recovery Image 19.07.8:
mount-firmware
```

To unmount your main firmware execute:
```
# For Recovery Image 18.06.5:
umount_firmware

# For Recovery Image 19.07.8:
umount-firmware
```

To reset the main firmware (delete all changes done to it) execute:
```
# For Recovery Image 19.07.8:
reset-firmware
```

## Unlocking mtd partitions

All mtd partitions - except firmware - are marked as read-only to avoid accidental damage.

However, it is possible to unlock them if needed by executing the following command while on the **recovery image**:

```
# For Recovery Image 18.06.5:
insmod $(find /lib/modules/ -name mtd-rw.ko) i_want_a_brick=1

# For Recovery Image 19.07.8:
mtd-rw unlock
```

*All partitions will be locked again on the next reboot.*

## Save changes to recovery

The new recovery image 19.07.8 is capable of persisting changes.

To save all changes done to the recovery:
```
recovery-config save
```

To reset all saved changes:
```
recovery-config reset
```

*Please be aware the amount of space available for storing changes is very limited!*

# Build guide

Below you'll find instructions on how to build each of the images you find on this repository.

## Build environment

All binary images available on this repository have been compiled using a clean installation of Ubuntu Server:
  * Ubuntu Server 16.04: used for all the older images up to version 21.02.5
  * Ubuntu Server 18.04: used for image versions between 22.03.0-rc1 and 22.03.7
  * Ubuntu Server 20.04: used for all images from version 23.05.0-rc1 onwards

Prepare your build environment using the instructions below:
  1. Download and install Ubuntu Server on a VM:
     * Ubuntu Server 16.04: [Network Installer](http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-amd64/current/images/netboot/mini.iso)
     * Ubuntu Server 18.04: [Network Installer](http://archive.ubuntu.com/ubuntu/dists/bionic-updates/main/installer-amd64/current/images/netboot/mini.iso)
     * Ubuntu Server 20.04: [Network Installer](http://archive.ubuntu.com/ubuntu/dists/focal-updates/main/installer-amd64/current/legacy-images/netboot/mini.iso)
  2. Once done, install the required build dependencies:
```
sudo apt install build-essential pkg-config python python3-distutils libncurses5-dev libssl-dev zlib1g-dev
sudo apt install gawk gettext unzip subversion git
```
  3. Clone this repository so you have all patches available on your environment:
```
git clone https://github.com/brunompena/zsun-resources.git
```

## Compile images

Follow the below instructions for each image type.

### U-Boot

```
git clone https://github.com/pepe2k/u-boot_mod.git zsun-sd100.u-boot
cd zsun-sd100.u-boot
git checkout 7a540a7
patch -p1 < ../zsun-resources/zsun-sd100.u-boot.u-boot_mod[7a540a7].patch
wget http://downloads.openwrt.org/releases/18.06.5/targets/ar71xx/generic/openwrt-sdk-18.06.5-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz
tar xJvf openwrt-sdk-18.06.5-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz
make zsun_sd100 PATH=${PATH}:$(pwd)/openwrt-sdk-18.06.5-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/bin/
```

### Firmware Image
```
git clone https://github.com/openwrt/openwrt.git zsun-sd100.firmware
cd zsun-sd100.firmware
git checkout v19.07.0-rc1
patch -p1 < ../zsun-resources/zsun-sd100.firmware.ath79[v19.07.0-rc1].patch
chmod +x target/linux/ath79/base-files/etc/rc.button/BTN_0
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
cp ../zsun-resources/zsun-sd100.firmware.ath79[v19.07.0-rc1].config .config
make -j$(nproc)
```

### Recovery Image

```
git clone https://github.com/openwrt/openwrt.git zsun-sd100.recovery
cd zsun-sd100.recovery
git checkout v18.06.5
patch -p1 < ../zsun-resources/zsun-sd100.recovery.ar71xx[v18.06.5].patch
chmod +x target/linux/ar71xx/base-files/etc/rc.button/BTN_0
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
cp ../zsun-resources/zsun-sd100.recovery.ar71xx[v18.06.5].config .config
make -j$(nproc)
```

### Update Image

```
git clone https://github.com/openwrt/archive.git zsun-sd100.update
cd zsun-sd100.update
git checkout v15.05.1
patch -p1 < ../zsun-resources/zsun-sd100.update.ar71xx[v15.05.1].patch
chmod +x target/linux/ar71xx/base-files/etc/rc.button/BTN_0
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
cp ../zsun-resources/zsun-sd100.update.ar71xx[v15.05.1].config .config
make -j$(nproc)
```

## Image Builder

On this repository you can also find patches to add support for Zsun-SD100 to the official OpenWrt Image Builder.

To use these patches, start by customizing the below environment variables:
```
IMAGEBUILDER_VERSION="19.07.0"    # OpenWrt version to build
IMAGEBUILDER_PACKAGES=""          # List of packages to add/remove (see official documentation for details)
```

Then execute the following commands to build your image:
```
IMAGEBUILDER_PATCH="$(pwd)/zsun-resources/zsun-sd100.imagebuilder.ath79[v${IMAGEBUILDER_VERSION}].patch"
IMAGEBUILDER_NAME="openwrt-imagebuilder-${IMAGEBUILDER_VERSION}-ath79-generic.Linux-x86_64"
IMAGEBUILDER_URL="https://downloads.openwrt.org/releases/${IMAGEBUILDER_VERSION}/targets/ath79/generic/${IMAGEBUILDER_NAME}.tar.xz"
IMAGEBUILDER_IMAGE="$(pwd)/${IMAGEBUILDER_NAME}/bin/targets/ath79/generic/openwrt-${IMAGEBUILDER_VERSION}-ath79-generic-zsun_sd100-squashfs-sysupgrade.bin"

wget -qO- "${IMAGEBUILDER_URL}" | tar xJvf -
cd "${IMAGEBUILDER_NAME}"

patch -p1 < "${IMAGEBUILDER_PATCH}"
find files/ -type f -exec chmod 0755 {} \;

make image PROFILE="zsun_sd100" FILES="files/" PACKAGES="${IMAGEBUILDER_PACKAGES}" && \
echo -e "\n\nOpenWrt image was built successfully and can be found at:\n  ${IMAGEBUILDER_IMAGE}\n\n"
```

Once your customized image is built, you can then use any of the standard methods to flash it.
