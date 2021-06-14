# Scratch Disk \(EXT4\)

When using FMADIO Packet capture system for analytics processing SSD resources can be split into Capture devices and Scratch disk space. In scratch disk space 1-16TB of SSD can be mounted as a general purpose file system used to store temporarily/intermediate network packet processing results.

The system should have scratch disks setup and visible on the GUI as follows, if this has not been configured contact support@fmad.io on how to configure

![FMADIO Scratch Disk Network Analytics processing space](../.gitbook/assets/image%20%2849%29%20%281%29.png)

In the above example there are 2 disks SCR0 and SCR1 enabled for scratch disk these are seen on the file system as

```text
fmadio@fmadio20v3-287:~$ ls -al /opt/fmadio/disk/scr*
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr0 -> /dev/nvme2n1
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr1 -> /dev/nvme0n1
fmadio@fmadio20v3-287:~$
```

NOTE: the /dev/\* mount point may change from time to time, please use the /opt/fmadio/disk/scr\* path name for all operations.


### Specifcying Sratch Disks 

By default all SSD are specified are dedicated to capture. This is specified in the configuration file 


```text
/opt/fmadio/etc/disk.lua
```

Capture disks are specified here
```text
CacheDisk =
{
	["S462NF0MA04379A"] = "ssd0",
	["S462NF0MA05134B"] = "ssd1",
	["S5JXNG0N108745Y"] = "ssd2",
	["S5JXNG0N108746D"] = "ssd3",
}
```

In the above example we have 4 x SSD for capture. To convert half to capture and half to scratch disk modify as follows

```text
CacheDisk =
{
	["S462NF0MA04379A"] = "ssd0",
	["S462NF0MA05134B"] = "ssd1",
}
,
ScratchDisk =
{
	["S5JXNG0N108745Y"] = "scr0",
	["S5JXNG0N108746D"] = "scr1",
}

```

This is assigning the SSD Serial numbers to mount point /opt/fmadio/disk/scr0 and /opt/fmadio/disk/scr1. The actual Serial numbers for each system will be different, the mount point (scr0/scr1) is the same.

After updating confirm there are no syntax errors in the config file by running fmadiolua /opt/fmadio/etc/disk.lua as follows

```text
fmadio@fmadio20n40v3-363:~$ fmadiolua /opt/fmadio/etc/disk.lua
fmad fmadlua Jun 13 2021
calibrating...
0 : 2992970378           2.9930 cycles/nsec offset:7.030 Mhz
Cycles/Sec 2992970378.0000 Std:       0 cycle std(  0.00000000) Target:3.00 Ghz
argv fmadiolua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/disk.lua]
done 0.000038Sec 0.000001Min
fmadio@fmadio20n40v3-363:~$
```

Output as above shows correctly formated file. Output per below shows configuration file with a syntax error  (line 30 has some incorrect formatting)

```text
fmadio@fmadio20n40v3-363:~$ fmadiolua /opt/fmadio/etc/disk.lua
fmad fmadlua Jun 13 2021
calibrating...
0 : 2992970368           2.9930 cycles/nsec offset:7.030 Mhz
Cycles/Sec 2992970368.0000 Std:       0 cycle std(  0.00000000) Target:3.00 Ghz
argv fmadiolua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/disk.lua]
load status: 3 0
/opt/fmadio/etc/disk.lua:30: '}' expected (to close '{' at line 2) near 'ScratchDisk'
fmadio@fmadio20n40v3-363:~$
```

After confirming the configuriation file syntax is correct, reboot the system. The mount points scr0 and scr1 should be visibile

```text
fmadio@fmadio20n40v3-363:~$ ls -al /opt/fmadio/disk/scr*
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr0 -> /dev/nvme2n1
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr1 -> /dev/nvme0n1
fmadio@fmadio20n40v3-363:~$
```

### Creating RAID0 partition

Start by creating a /dev/md1 RAID0 partition as follows

```text
fmadio@fmadio20v3-287:~$ sudo mdadm --create /dev/md1 --force --level=raid0 --raid-devices=2 /opt/fmadio/disk/scr0 /opt/fmadio/disk/scr1
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
fmadio@fmadio20v3-287:~$
```

This creates a /dev/md1 partition as shown with lsblk command. Can see the /dev/md1 device

```text
fmadio@fmadio20v3-287:~$ lsblk
NAME    MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sdd       8:48   0   3.7T  0 disk
sdb       8:16   0   3.7T  0 disk
nvme2n1 259:0    0 447.1G  0 disk
`-md1     9:1    0   894G  0 raid0
nvme1n1 259:3    0   477G  0 disk
sde       8:64   0   3.7T  0 disk
sdc       8:32   0   3.7T  0 disk
sda       8:0    0 238.5G  0 disk
|-sda2    8:2    0 223.6G  0 part  /mnt/store0
`-sda1    8:1    0  14.9G  0 part  /mnt/sda1
nvme0n1 259:1    0 447.1G  0 disk
`-md1     9:1    0   894G  0 raid0
nvme3n1 259:2    0   477G  0 disk
fmadio@fmadio20v3-287:~$
```

More detail via the mdadm --detail command

```text
fmadio@fmadio20v3-287:~$ sudo mdadm --detail /dev/md1
/dev/md1:
        Version : 1.2
  Creation Time : Sun Jun  6 17:43:12 2021
     Raid Level : raid0
     Array Size : 937440256 (894.01 GiB 959.94 GB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Sun Jun  6 17:43:12 2021
          State : clean
 Active Devices : 2
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 0

     Chunk Size : 512K

           Name : fmadio20v3-287:1  (local to host fmadio20v3-287)
           UUID : 06234ac8:694ae295:3659e4fc:b59a7d55
         Events : 0

    Number   Major   Minor   RaidDevice State
       0     259        0        0      active sync   /dev/nvme2n1
       1     259        1        1      active sync   /dev/nvme0n1
fmadio@fmadio20v3-287:~$
```

### Create EXT4 Filesystem

The block device /dev/md1 is block level only, it contains no mountable file system. Next create btrfs filesystem on the device as follows

```text
fmadio@fmadio20v3-287:~$ sudo mkfs.ext4 /dev/md1

< update me >

fmadio@fmadio20v3-287:~$
```

### Mount the Scratch Filesystem

By default FMADIO Packet Capture systems at boot time will start the RAID0 partition and mount /dev/md1 Scratch disk to /mnt/store1 

If it fails to mount, please issue the following command

```text
fmadio@fmadio20v3-287:~$ sudo mount -t ext4 /dev/md1 /mnt/store1
fmadio@fmadio20v3-287:~$
```
