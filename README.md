# JVS-H411
Repair instruction for IP Camera JVS-H411 (HZD-600DM)

###Steps for reflashing firmware
+ Write firmware file `IPC_flash_up.bin` in root of the sd-card
+ Insert sd-card in the camera and reboot (power off/on)
+ Wait until camera boot up

If all done successfully you can connect to camera via browser by camera IP.

###Telnet connection
For the telnet connection try to use login `root` and password `jvbzd`.

###Edit appfs partition
+ Insert sdcard and restar camera
+ Connect via telnet
+ Create mount point `mkdir /tmp/sdcard`
+ Mount sdcard `mount /dev/mmcblock0p1 /tmp/sdcard`
+ Check appfs mtd partition `cat /proc/mtd`
+ Create appfs image `dd if=/dev/mtdblockN of=/tmp/sdcard/appfs.sqsh`
+ Unmount sdcard `umount /tmp/sdcard`
+ Copy appfs image to the host system
+ Decompress image `unsquashfs appfs.sqsh`
+ Edit what you want
+ Check the squashfs options (compression type, block size) of the original image `unsquashfs -s appfs.sqsh`
+ Create new image `mksquashfs ./squashfs-root ./appfs-new.sqsh -comp TYPE -b SIZE`
+ Copy new image to sdcard
+ Repeat 1-4 steps (mount sdcard)
+ Check new image size `stat /tmp/sdcard/appfs-new.sqsh`
+ Write new appfs image `dd if=/tmp/sdcard/appfs-new.sqsh of=/dev/mtdblockN bs=NEW_IMAGE_SIZE count=1`
+ Umount sdcard
+ Reboot device

###Resources
[Jovision's firmwares](http://down.jovision.com:81/cn/repair/)

[Update source mirror#1](http://updatewt.afdvr.com/)

[Update source mirror#2](http://updatedx.afdvr.com/)

[Update source mirror#3](http://updatehw.afdvr.com/)
