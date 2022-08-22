# Scratch Disk (BTRFS)

**FW: 7167+**

When using FMADIO Packet capture system for analytics processing SSD resources can be split into Capture devices and Scratch disk space. In scratch disk space 1-16TB of SSD can be mounted as a general purpose file system used to store temporarily/intermediate network packet processing results.

The system should have scratch disks setup and visible on the GUI as follows, if this has not been configured contact support@fmad.io on how to configure

![FMADIO Scratch Disk Network Analytics processing space](<../.gitbook/assets/image (49) (2) (3).png>)

&#x20;**** In the above example there are 2 disks SCR0 and SCR1 enabled for scratch disk these are seen on the file system as

```
fmadio@fmadio20v3-287:~$ ls -al /opt/fmadio/disk/scr*
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr0 -> /dev/nvme2n1
lrwxrwxrwx    1 root     root            12 Jun  6 17:02 /opt/fmadio/disk/scr1 -> /dev/nvme0n1
fmadio@fmadio20v3-287:~$
```

NOTE: the /dev/\* mount point may change from time to time, please use the /opt/fmadio/disk/scr\* path name for all operations.

### Creating RAID0 partition

Start by creating a /dev/md1 RAID0 partition as follows

```
fmadio@fmadio20v3-287:~$ sudo mdadm --create /dev/md1 --force --level=raid0 --raid-devices=2 /opt/fmadio/disk/scr0 /opt/fmadio/disk/scr1
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
fmadio@fmadio20v3-287:~$
```

This creates a /dev/md1 partition as shown with lsblk command. Can see the /dev/md1 device

```
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

```
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

### Create BTRFS Filesystem

The block device /dev/md1 is block level only, it contains no mountable file system. Next create btrfs filesystem on the device as follows

```
fmadio@fmadio20v3-287:~$ sudo mkfs.btrfs /dev/md1
btrfs-progs v5.12.1
See http://btrfs.wiki.kernel.org for more information.

Detected a SSD, turning off metadata duplication.  Mkfs with -m dup if you want to force metadata duplication.
Performing full device TRIM /dev/md1 (894.01GiB) ...
Label:              (null)
UUID:               5432ceac-b3d1-4b1c-8b69-d622542a9184
Node size:          16384
Sector size:        4096
Filesystem size:    894.01GiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         single            8.00MiB
  System:           single            4.00MiB
SSD detected:       yes
Zoned device:       no
Incompat features:  extref, skinny-metadata
Runtime features:
Checksum:           crc32c
Number of devices:  1
Devices:
   ID        SIZE  PATH
    1   894.01GiB  /dev/md1

fmadio@fmadio20v3-287:~$
```

### Mount the BTRFS Filesystem

By default FMADIO Packet Capture systems at boot time mount BTRFS with lzo disk compression. Compression can be enabled or disabled with BTRFS on-the-fly. In this case we will mount it the same as capture system does at boot time.

```
fmadio@fmadio20v3-287:~$ sudo mount -t btrfs -o compress=lzo /dev/md1 /mnt/store1
fmadio@fmadio20v3-287:~$
```

Then check the mount point with lsblk. Below we can see /dev/md1 is mounted on /mnt/store1&#x20;

```
fmadio@fmadio20v3-287:~$ lsblk
NAME    MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sdd       8:48   0   3.7T  0 disk
sdb       8:16   0   3.7T  0 disk
nvme2n1 259:0    0 447.1G  0 disk
`-md1     9:1    0   894G  0 raid0 /mnt/store1
nvme1n1 259:3    0   477G  0 disk
sde       8:64   0   3.7T  0 disk
sdc       8:32   0   3.7T  0 disk
sda       8:0    0 238.5G  0 disk
|-sda2    8:2    0 223.6G  0 part  /mnt/store0
`-sda1    8:1    0  14.9G  0 part  /mnt/sda1
nvme0n1 259:1    0 447.1G  0 disk
`-md1     9:1    0   894G  0 raid0 /mnt/store1
nvme3n1 259:2    0   477G  0 disk
fmadio@fmadio20v3-287:~$

```

### Check BTRFS compression level

Checking the compression level with BTRFS requires calculating the raw storage and the compressed storage.

```
fmadio@fmadio20v3-287:/mnt/store1$ du -h -d 1
3.0G    ./cache
3.0G    .
fmadio@fmadio20v3-287:/mnt/store1$ 


fmadio@fmadio20v3-287:/mnt/store1$ sudo btrfs fi show
Label: none  uuid: 5432ceac-b3d1-4b1c-8b69-d622542a9184
        Total devices 1 FS bytes used 751.07MiB
        devid    1 size 894.01GiB used 2.02GiB path /dev/md1
        
fmadio@fmadio20v3-287:/mnt/store1$

```

In the above example we see /mnt/store1 has 3.0 GB worth of data (using du)

In the above example we see /mnt/store1 has used 751.MiB of actual storage capacity (using btrfs fi show)

Based on the above math (3112MB / 751MB) , the **compression rate is \~ x4.04**&#x20;
