# CPU Mapping

**FW: 8325+**

FMADIO Packet Capture system have 16 to 96 CPUs, this can make configuration and allocation of all these CPU resources tedious and error prone.

To assist the configuration file located in

```
/opt/fmadio/etc/time.lua
```

There is a section name CPUMap, an example is shown below

```
["CPUMap"] =
{
    ["Enable"] = true,
    [  0] = "fmadio",                        --  CPU:  0 Core:  0 Thread:0 Node: 0 system
    [  1] = "",                              --  CPU:  1 Core:  1 Thread:0 Node: 0
    [  2] = "fmadio",                        --  CPU:  2 Core:  2 Thread:0 Node: 0 system
    [  3] = "fmadio",                        --  CPU:  3 Core:  3 Thread:0 Node: 0 system
    [  4] = "",                              --  CPU:  4 Core:  4 Thread:0 Node: 0
    [  5] = "",                              --  CPU:  5 Core:  5 Thread:0 Node: 0
    [  6] = "",                              --  CPU:  6 Core:  6 Thread:0 Node: 0
    [  7] = "",                              --  CPU:  7 Core:  7 Thread:0 Node: 0
    [  8] = "",                              --  CPU:  8 Core:  8 Thread:0 Node: 0
    [  9] = "",                              --  CPU:  9 Core:  9 Thread:0 Node: 0
    [ 10] = "",                              --  CPU: 10 Core: 10 Thread:0 Node: 0
    [ 11] = "",                              --  CPU: 11 Core: 11 Thread:0 Node: 0
    [ 12] = "",                              --  CPU: 12 Core: 12 Thread:0 Node: 0
    [ 13] = "",                              --  CPU: 13 Core: 13 Thread:0 Node: 0
    [ 14] = "",                              --  CPU: 14 Core: 14 Thread:0 Node: 0
    [ 15] = "",                              --  CPU: 15 Core: 15 Thread:0 Node: 0
    [ 16] = "",                              --  CPU: 16 Core: 16 Thread:0 Node: 0
    [ 17] = "",                              --  CPU: 17 Core: 17 Thread:0 Node: 0
    [ 18] = "",                              --  CPU: 18 Core: 18 Thread:0 Node: 0
    [ 19] = "",                              --  CPU: 19 Core: 19 Thread:0 Node: 0
    [ 20] = "",                              --  CPU: 20 Core: 20 Thread:0 Node: 0
    [ 21] = "",                              --  CPU: 21 Core: 21 Thread:0 Node: 0
    [ 22] = "",                              --  CPU: 22 Core: 22 Thread:0 Node: 0
    [ 23] = "",                              --  CPU: 23 Core: 23 Thread:0 Node: 0
    [ 24] = "fmadio",                        --  CPU: 24 Core:  0 Thread:0 Node: 1 system
    [ 25] = "fmadio",                        --  CPU: 25 Core:  1 Thread:0 Node: 1 system
    [ 26] = "fmadio",                        --  CPU: 26 Core:  2 Thread:0 Node: 1 system
    [ 27] = "fmadio",                        --  CPU: 27 Core:  3 Thread:0 Node: 1 system
    [ 28] = "fmadio",                        --  CPU: 28 Core:  4 Thread:0 Node: 1 system
    [ 29] = "fmadio",                        --  CPU: 29 Core:  5 Thread:0 Node: 1 system
    [ 30] = "fmadio",                        --  CPU: 30 Core:  6 Thread:0 Node: 1 system
    [ 31] = "fmadio",                        --  CPU: 31 Core:  7 Thread:0 Node: 1 system
    [ 32] = "",                              --  CPU: 32 Core:  8 Thread:0 Node: 1
    [ 33] = "",                              --  CPU: 33 Core:  9 Thread:0 Node: 1
    [ 34] = "",                              --  CPU: 34 Core: 10 Thread:0 Node: 1
    [ 35] = "",                              --  CPU: 35 Core: 11 Thread:0 Node: 1
    [ 36] = "",                              --  CPU: 36 Core: 12 Thread:0 Node: 1
    [ 37] = "",                              --  CPU: 37 Core: 13 Thread:0 Node: 1
    [ 38] = "",                              --  CPU: 38 Core: 14 Thread:0 Node: 1
    [ 39] = "",                              --  CPU: 39 Core: 15 Thread:0 Node: 1
    [ 40] = "",                              --  CPU: 40 Core: 16 Thread:0 Node: 1
    [ 41] = "",                              --  CPU: 41 Core: 17 Thread:0 Node: 1
    [ 42] = "",                              --  CPU: 42 Core: 18 Thread:0 Node: 1
    [ 43] = "",                              --  CPU: 43 Core: 19 Thread:0 Node: 1
    [ 44] = "",                              --  CPU: 44 Core: 20 Thread:0 Node: 1
    [ 45] = "",                              --  CPU: 45 Core: 21 Thread:0 Node: 1
    [ 46] = "",                              --  CPU: 46 Core: 22 Thread:0 Node: 1
    [ 47] = "",                              --  CPU: 47 Core: 23 Thread:0 Node: 1
    [ 48] = "",                              --  CPU: 48 Core:  0 Thread:1 Node: 0
    [ 49] = "",                              --  CPU: 49 Core:  1 Thread:1 Node: 0
    [ 50] = "",                              --  CPU: 50 Core:  2 Thread:1 Node: 0
    [ 51] = "",                              --  CPU: 51 Core:  3 Thread:1 Node: 0
    [ 52] = "",                              --  CPU: 52 Core:  4 Thread:1 Node: 0
    [ 53] = "",                              --  CPU: 53 Core:  5 Thread:1 Node: 0
    [ 54] = "",                              --  CPU: 54 Core:  6 Thread:1 Node: 0
    [ 55] = "",                              --  CPU: 55 Core:  7 Thread:1 Node: 0
    [ 56] = "",                              --  CPU: 56 Core:  8 Thread:1 Node: 0
    [ 57] = "",                              --  CPU: 57 Core:  9 Thread:1 Node: 0
    [ 58] = "",                              --  CPU: 58 Core: 10 Thread:1 Node: 0
    [ 59] = "",                              --  CPU: 59 Core: 11 Thread:1 Node: 0
    [ 60] = "",                              --  CPU: 60 Core: 12 Thread:1 Node: 0
    [ 61] = "",                              --  CPU: 61 Core: 13 Thread:1 Node: 0
    [ 62] = "",                              --  CPU: 62 Core: 14 Thread:1 Node: 0
    [ 63] = "",                              --  CPU: 63 Core: 15 Thread:1 Node: 0
    [ 64] = "",                              --  CPU: 64 Core: 16 Thread:1 Node: 0
    [ 65] = "",                              --  CPU: 65 Core: 17 Thread:1 Node: 0
    [ 66] = "",                              --  CPU: 66 Core: 18 Thread:1 Node: 0
    [ 67] = "",                              --  CPU: 67 Core: 19 Thread:1 Node: 0
    [ 68] = "",                              --  CPU: 68 Core: 20 Thread:1 Node: 0
    [ 69] = "",                              --  CPU: 69 Core: 21 Thread:1 Node: 0
    [ 70] = "",                              --  CPU: 70 Core: 22 Thread:1 Node: 0
    [ 71] = "",                              --  CPU: 71 Core: 23 Thread:1 Node: 0
    [ 72] = "fmadio",                        --  CPU: 72 Core:  0 Thread:1 Node: 1 system
    [ 73] = "",                              --  CPU: 73 Core:  1 Thread:1 Node: 1
    [ 74] = "",                              --  CPU: 74 Core:  2 Thread:1 Node: 1
    [ 75] = "",                              --  CPU: 75 Core:  3 Thread:1 Node: 1
    [ 76] = "fmadio",                        --  CPU: 76 Core:  4 Thread:1 Node: 1 system
    [ 77] = "fmadio",                        --  CPU: 77 Core:  5 Thread:1 Node: 1 system
    [ 78] = "fmadio",                        --  CPU: 78 Core:  6 Thread:1 Node: 1 system
    [ 79] = "fmadio",                        --  CPU: 79 Core:  7 Thread:1 Node: 1 system
    [ 80] = "",                              --  CPU: 80 Core:  8 Thread:1 Node: 1
    [ 81] = "",                              --  CPU: 81 Core:  9 Thread:1 Node: 1
    [ 82] = "",                              --  CPU: 82 Core: 10 Thread:1 Node: 1
    [ 83] = "",                              --  CPU: 83 Core: 11 Thread:1 Node: 1
    [ 84] = "",                              --  CPU: 84 Core: 12 Thread:1 Node: 1
    [ 85] = "",                              --  CPU: 85 Core: 13 Thread:1 Node: 1
    [ 86] = "",                              --  CPU: 86 Core: 14 Thread:1 Node: 1
    [ 87] = "",                              --  CPU: 87 Core: 15 Thread:1 Node: 1
    [ 88] = "",                              --  CPU: 88 Core: 16 Thread:1 Node: 1
    [ 89] = "",                              --  CPU: 89 Core: 17 Thread:1 Node: 1
    [ 90] = "",                              --  CPU: 90 Core: 18 Thread:1 Node: 1
    [ 91] = "",                              --  CPU: 91 Core: 19 Thread:1 Node: 1
    [ 92] = "",                              --  CPU: 92 Core: 20 Thread:1 Node: 1
    [ 93] = "",                              --  CPU: 93 Core: 21 Thread:1 Node: 1
    [ 94] = "",                              --  CPU: 94 Core: 22 Thread:1 Node: 1
    [ 95] = "",                              --  CPU: 95 Core: 23 Thread:1 Node: 1
},

```

CPUS which are dedicated to FMADIO Packet Capture system can not modified, these are marked "System" in the comments section per below

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>FMADIO System CPUs</p></figcaption></figure>

Any change to system CPUs will be overwritten.

### Enable Custom CPU Mapping

By default the system allocates all CPUs to the FMADIO Capture System. To enable a customer CPU Mapping change Enable = true per the below setting

```
["Enable"] = true,
```

After enabling a FW update is required. As the enable re-writes the isolcpu Linux kernel boot parameters.

NOTE: after Enable = true is set, all subsequent firmware updates will modify the isolcpu setting. To return to stock default setting. Set to false, and update the firmware again.

### Assign a CPU

To assign a CPU to a specific LXC container, use the naming convention

```
container.process.instance
```

For example Market Data Gap detector LXC (mdgap) for CME is allocated to CPU 84

```
    .
    .
    [ 77] = "fmadio",                        --  CPU: 77 Core:  5 Thread:1 Node: 1 system
    [ 78] = "fmadio",                        --  CPU: 78 Core:  6 Thread:1 Node: 1 system
    [ 79] = "fmadio",                        --  CPU: 79 Core:  7 Thread:1 Node: 1 system
    [ 80] = "",                              --  CPU: 80 Core:  8 Thread:1 Node: 1
    [ 81] = "",                              --  CPU: 81 Core:  9 Thread:1 Node: 1
    [ 82] = "",                              --  CPU: 82 Core: 10 Thread:1 Node: 1
    [ 83] = "",                              --  CPU: 83 Core: 11 Thread:1 Node: 1
    [ 84] = "mdgap.cme.0",                   --  CPU: 84 Core: 12 Thread:1 Node: 1
    [ 85] = "",                              --  CPU: 85 Core: 13 Thread:1 Node: 1
    [ 86] = "",                              --  CPU: 86 Core: 14 Thread:1 Node: 1
    [ 87] = "",                              --  CPU: 87 Core: 15 Thread:1 Node: 1
    .
    .
```

This also requires re-running the LXC install script to correctly allocate the CPUs in the container.

### Update CPU Mapping

After the configuration files have been updated, a firmware update and reboot cycle are required. Its required as the Linux kernel boot command parameters (isolcpus) gets modified requiring the reboot.

Start by finding the current Firmware binary, in almost all cases this is the last upload firmware

```
fmadio@fmadio20v3-287:~$ cd /mnt/system/firmware/
fmadio@fmadio20v3-287:/mnt/sda1/firmware$ ls -altr
total 2554288
drwxr-xr-x    7 root     root          8192 Jan  1  1970 ../
-rwxr-xr-x    1 root     root     107442176 Oct 13 13:16 fmadio20v3_20221013_1314.tcz
-rwxr-xr-x    1 root     root           820 Oct 13 13:16 fmadio20v3_20221013_1314.syslinux.cfg.analytics
-rwxr-xr-x    1 root     root          1425 Oct 13 13:16 fmadio20v3_20221013_1314.syslinux.cfg
-rwxr-xr-x    1 root     root           868 Oct 13 13:16 fmadio20v3_20221013_1314.sign
-rwxr-xr-x    1 root     root       9884660 Oct 13 13:16 fmadio20v3_20221013_1314.rom.2x1G
-rwxr-xr-x    1 root     root       9868320 Oct 13 13:16 fmadio20v3_20221013_1314.rom.2x10G
-rwxr-xr-x    1 root     root            87 Oct 13 13:16 fmadio20v3_20221013_1314.pre.lua
-rwxr-xr-x    1 root     root           754 Oct 13 13:16 fmadio20v3_20221013_1314.post.lua
-rwxr-xr-x    1 root     root         21356 Oct 13 13:16 fmadio20v3_20221013_1314.mydata
-rwxr-xr-x    1 root     root       4632672 Oct 13 13:16 fmadio20v3_20221013_1314.kernel
-rwxr-xr-x    1 root     root     312308358 Oct 13 13:16 fmadio20v3_20221013_1314.core
-rwxr-xr-x    1 root     root     427477506 Oct 13 13:16 fmadio20v3_20221013_1314.bin
-rwxr-xr-x    1 root     root     427590054 Oct 21 10:28 fmadio20v3_20221021_1026.bin
-rwxr-xr-x    1 root     root     107548672 Oct 21 10:28 fmadio20v3_20221021_1026.tcz
-rwxr-xr-x    1 root     root           820 Oct 21 10:28 fmadio20v3_20221021_1026.syslinux.cfg.analytics
-rwxr-xr-x    1 root     root          1425 Oct 21 10:28 fmadio20v3_20221021_1026.syslinux.cfg
-rwxr-xr-x    1 root     root           868 Oct 21 10:28 fmadio20v3_20221021_1026.sign
-rwxr-xr-x    1 root     root       9884660 Oct 21 10:28 fmadio20v3_20221021_1026.rom.2x1G
-rwxr-xr-x    1 root     root       9868320 Oct 21 10:28 fmadio20v3_20221021_1026.rom.2x10G
-rwxr-xr-x    1 root     root            87 Oct 21 10:28 fmadio20v3_20221021_1026.pre.lua
-rwxr-xr-x    1 root     root           754 Oct 21 10:28 fmadio20v3_20221021_1026.post.lua
-rwxr-xr-x    1 root     root         21356 Oct 21 10:28 fmadio20v3_20221021_1026.mydata
-rwxr-xr-x    1 root     root       4632672 Oct 21 10:28 fmadio20v3_20221021_1026.kernel
-rwxr-xr-x    1 root     root     312308358 Oct 21 10:28 fmadio20v3_20221021_1026.core
-rwxr-xr-x    1 root     root     107548672 Oct 21 11:27 fmadio20v3_20221021_1125.tcz
-rwxr-xr-x    1 root     root           820 Oct 21 11:27 fmadio20v3_20221021_1125.syslinux.cfg.analytics
-rwxr-xr-x    1 root     root          1425 Oct 21 11:27 fmadio20v3_20221021_1125.syslinux.cfg
-rwxr-xr-x    1 root     root           868 Oct 21 11:27 fmadio20v3_20221021_1125.sign
-rwxr-xr-x    1 root     root       9884660 Oct 21 11:27 fmadio20v3_20221021_1125.rom.2x1G
-rwxr-xr-x    1 root     root       9868320 Oct 21 11:27 fmadio20v3_20221021_1125.rom.2x10G
-rwxr-xr-x    1 root     root            87 Oct 21 11:27 fmadio20v3_20221021_1125.pre.lua
-rwxr-xr-x    1 root     root           754 Oct 21 11:27 fmadio20v3_20221021_1125.post.lua
-rwxr-xr-x    1 root     root         21356 Oct 21 11:27 fmadio20v3_20221021_1125.mydata
-rwxr-xr-x    1 root     root       4632672 Oct 21 11:27 fmadio20v3_20221021_1125.kernel
-rwxr-xr-x    1 root     root     312308358 Oct 21 11:27 fmadio20v3_20221021_1125.core
-rwxr-xr-x    1 root     root     427587155 Oct 21 11:27 fmadio20v3_20221021_1125.bin
drwxr-xr-x    2 root     root         40960 Oct 21 11:27 ./
fmadio@fmadio20v3-287:/mnt/sda1/firmware$

```

In this case the latest firmware is named

```
fmadio20v3_20221021_1125.bin
```

Next re-install the firmware as follows. An upload is not required as the firmware is already on the system

```
sudo firmware_install.lua --install fmadio20v3_20221021_1125.bin
```

Example output as follows, note the "Update CPU Map" output

```bash
fmadio@fmadio20v3-287:/mnt/sda1/firmware$ sudo firmware_install.lua --install fmadio20v3_20221021_1125.bin
fmad fmadlua Oct 21 2022 (/usr/local/bin/fmadiolua /opt/fmadio/bin/firmware_install.lua --install fmadio20v3_20221021_1125.bin )
calibrating...
0 : 2095063152           2.0951 cycles/nsec offset:4.937 Mhz
Cycles/Sec 2095063152.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
Install Firmware [fmadio20v3_20221021_1125.bin]
Open fSysCapture_t* [/opt/fmadio/status/capture:0/100]
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
FW [fmadio20v3] System[fmadio20v3]
PreInstall script [/mnt/system//firmware/fmadio20v3_20221021_1125.pre.lua]
fmad fmadlua Oct 21 2022 (/opt/fmadio/bin/fmadiolua /mnt/system//firmware/fmadio20v3_20221021_1125.pre.lua )
calibrating...
0 : 2095065834           2.0951 cycles/nsec offset:4.934 Mhz
Cycles/Sec 2095065834.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
done 0.000043Sec 0.000001Min
PreInstall: pre install script
PreInstall: PREINSTALL_GOOD
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.tcz /mnt/system//tce/optional/fmadio20v3_current.tcz]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.core /mnt/system//boot/fmadio20v3-corepure64.gz]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.kernel /mnt/system//boot/vmlinuz64]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.mydata /mnt/system//tce/mydata.tgz]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.syslinux.cfg /mnt/system//boot/syslinux/syslinux.cfg]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.rom.2x1G /mnt/system//boot/bitstream.rom.2x1G]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.rom.2x10G /mnt/system//boot/bitstream.rom.2x10G]
Bitstream Config [2x10G]
Copy [cp /mnt/system//firmware/fmadio20v3_20221021_1125.rom.2x10G /mnt/system//boot/bitstream.rom]
os[sudo /opt/fmadio/bin/bitstream_update.lua --noreboot --write /mnt/system//boot/bitstream.rom]
fmad fmadlua Oct 21 2022 (/opt/fmadio/bin/fmadiolua /opt/fmadio/bin/bitstream_update.lua --noreboot --write /mnt/system//boot/bitstream.rom )
calibrating...
0 : 2095065974           2.0951 cycles/nsec offset:4.934 Mhz
Cycles/Sec 2095065974.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
cp: '/mnt/system//boot/bitstream.rom' and '/mnt/system//boot/bitstream.rom' are the same file
done 0.004990Sec 0.000083Min
Updating CPU Map
renaming syslinux.cfg files
/mnt/system//firmware/fmadio20v3_20221021_1125.post.lua
fmad fmadlua Oct 21 2022 (/opt/fmadio/bin/fmadiolua /mnt/system//firmware/fmadio20v3_20221021_1125.post.lua )
calibrating...
0 : 2095064668           2.0951 cycles/nsec offset:4.935 Mhz
Cycles/Sec 2095064668.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
PostInstall: post install script
done 0.001261Sec 0.000021Min
PostInstall: POSTINSTALL_GOOD
Firmware Install Complete
done 36.463587Sec 0.607726Min
fmadio@fmadio20v3-287:/mnt/sda1/firmware$

```

Then reboot the system, and the new CPU mapping is reflected.

```
sudo reboot
```
