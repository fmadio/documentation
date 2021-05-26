# RAID5 HDD Replacement

Over the lifetime of a system HDD can fail, both PSU and HDD are the most common hardware failure parts. To replace a HDD is straight forward, follow the steps as below

### 1\) Add in new HDD mount point 

use the command "devblk\_info.lua" as follows to find the un-used HDD serial number

```text
fmadio@fmadio20-049:~$ devblk_info.lua
     sda       ata1   0:0:0:0              SanDisk SDSSDXPS240G 153837400469        os0
     sdd       ata7   6:0:0:0                TOSHIBA MD04ACA400 153DKAT0FSAA       hdd0
     sde       ata8   7:0:0:0                TOSHIBA MD04ACA400 X522KQVMFSAA
     sdc       ata6   5:0:0:0                TOSHIBA MD04ACA400 357FK7NTFSAA       hdd2
     sdb       ata5   4:0:0:0                TOSHIBA MD04ACA400 6523KK34FSAA       hdd3
     sdf 0000:05:00.0   12:1:8:0             SanDisk SDSSDXPS240G 153762402730       ssd0
     sdg 0000:05:00.0   12:1:9:0             SanDisk SDSSDXPS240G 150645401380       ssd1
     sdh 0000:05:00.0   12:1:10:0            SanDisk SDSSDXPS240G 150645401497       ssd2
     sdi 0000:05:00.0   12:1:11:0            SanDisk SDSSDXPS240G 152040401078       ssd3
     sdj 0000:05:00.0   12:1:12:0            SanDisk SDSSDXPS240G 152574400062       ssd7
     sdk 0000:05:00.0   12:1:13:0            SanDisk SDSSDXPS240G 153762402132       ssd5
     sdl 0000:05:00.0   12:1:14:0            SanDisk SDSSDXPS240G 153762402953       ssd6
     sdm 0000:05:00.0   12:1:15:0            SanDisk SDSSDXPS240G 150645401496       ssd4
Total Devices: 13
fmadio@fmadio20-049:~$

```

Notice in the above output, the serial number "X522KQVMFSAA" has no drive assignment, while all other disks have a mount point \(os\*, hdd\*, ssd\*\)

Edit the file /opt/fmadio/etc/disk.lua, before setting is shown below

```text
.
.
["RaidDisk"] =
{
    ["some_other_serial_number"]    = "hdd1",
    ["357FK7NTFSAA"]    = "hdd2",
    ["153DKAT0FSAA"]    = "hdd0",
    ["6523KK34FSAA"]    = "hdd3",
},
.
.

```

Update it using the above new serial number "X522KQVMFSAA", as shown below.

```text
.
.
["RaidDisk"] =
{
    ["X522KQVMFSAA"]    = "hdd1",
    ["357FK7NTFSAA"]    = "hdd2",
    ["153DKAT0FSAA"]    = "hdd0",
    ["6523KK34FSAA"]    = "hdd3",
},
.
.

```

Then run "devblock\_info.lua again to confirm serial numbers all match with mount points. In this case hdd1 now maps to the new HDD serial number

```text
fmadio@fmadio20-049:~$ devblk_info.lua
     sda       ata1   0:0:0:0              SanDisk SDSSDXPS240G 153837400469        os0
     sdd       ata7   6:0:0:0                TOSHIBA MD04ACA400 153DKAT0FSAA       hdd0
     sde       ata8   7:0:0:0                TOSHIBA MD04ACA400 X522KQVMFSAA       hdd1
     sdc       ata6   5:0:0:0                TOSHIBA MD04ACA400 357FK7NTFSAA       hdd2
     sdb       ata5   4:0:0:0                TOSHIBA MD04ACA400 6523KK34FSAA       hdd3
     sdf 0000:05:00.0   12:1:8:0             SanDisk SDSSDXPS240G 153762402730       ssd0
     sdg 0000:05:00.0   12:1:9:0             SanDisk SDSSDXPS240G 150645401380       ssd1
     sdh 0000:05:00.0   12:1:10:0            SanDisk SDSSDXPS240G 150645401497       ssd2
     sdi 0000:05:00.0   12:1:11:0            SanDisk SDSSDXPS240G 152040401078       ssd3
     sdj 0000:05:00.0   12:1:12:0            SanDisk SDSSDXPS240G 152574400062       ssd7
     sdk 0000:05:00.0   12:1:13:0            SanDisk SDSSDXPS240G 153762402132       ssd5
     sdl 0000:05:00.0   12:1:14:0            SanDisk SDSSDXPS240G 153762402953       ssd6
     sdm 0000:05:00.0   12:1:15:0            SanDisk SDSSDXPS240G 150645401496       ssd4


```



### 2\) Reboot the system

Reboot the system so the new HDD drive mappings gets updated. The files in /opt/fmadio/disk/hdd\* should have links for hdd0, hdd1, hdd2, hdd3. 

NOTE: the actual /dev/sd\* value changes psuedo randomly and not important. Its why we map using the disks serial number

```text
fmadio@fmadio20-049:~$ ls -al /opt/fmadio/disk/hdd*
lrwxrwxrwx    1 root     root             8 May 25 15:10 /opt/fmadio/disk/hdd0 -> /dev/sdd
lrwxrwxrwx    1 root     root             8 May 25 15:10 /opt/fmadio/disk/hdd1 -> /dev/sde
lrwxrwxrwx    1 root     root             8 May 25 15:10 /opt/fmadio/disk/hdd2 -> /dev/sdc
lrwxrwxrwx    1 root     root             8 May 25 15:10 /opt/fmadio/disk/hdd3 -> /dev/sdb
fmadio@fmadio20-049:~$
```

### 3\) Start the RAID array

Check if /dev/md0 is has been started, The following has /dev/md0 started, note the md0 partitions

```text
fmadio@fmadio20-049:~$ lsblk
NAME      MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda         8:0    0 223.6G  0 disk
|-sda1      8:1    0  14.9G  0 part  /mnt/sda1
`-sda2      8:2    0 208.7G  0 part  /mnt/store0
sdb         8:16   0   3.7T  0 disk
`-md0       9:0    0  10.9T  0 raid5
  |-md0p1 259:0    0  59.6G  0 md
  `-md0p2 259:1    0  10.9T  0 md
sdc         8:32   0   3.7T  0 disk
`-md0       9:0    0  10.9T  0 raid5
  |-md0p1 259:0    0  59.6G  0 md
  `-md0p2 259:1    0  10.9T  0 md
sdd         8:48   0   3.7T  0 disk
`-md0       9:0    0  10.9T  0 raid5
  |-md0p1 259:0    0  59.6G  0 md
  `-md0p2 259:1    0  10.9T  0 md
sde         8:64   0   3.7T  0 disk
sdf         8:80   1 223.6G  0 disk
sdg         8:96   1 223.6G  0 disk
sdh         8:112  1 223.6G  0 disk
sdi         8:128  1 223.6G  0 disk
sdj         8:144  1 223.6G  0 disk
sdk         8:160  1 223.6G  0 disk
sdl         8:176  1 223.6G  0 disk
sdm         8:192  1 223.6G  0 disk
```

The following shows if /dev/md0 has not been started, notice the md0 partitions are missing

```text
fmadio@fmadio20-049:~$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 223.6G  0 disk
|-sda1   8:1    0  14.9G  0 part /mnt/sda1
`-sda2   8:2    0 208.7G  0 part /mnt/store0
sdb      8:16   0   3.7T  0 disk
sdc      8:32   0   3.7T  0 disk
sdd      8:48   0   3.7T  0 disk
sde      8:64   0   3.7T  0 disk
sdf      8:80   1 223.6G  0 disk
sdg      8:96   1 223.6G  0 disk
sdh      8:112  1 223.6G  0 disk
sdi      8:128  1 223.6G  0 disk
sdj      8:144  1 223.6G  0 disk
sdk      8:160  1 223.6G  0 disk
sdl      8:176  1 223.6G  0 disk
sdm      8:192  1 223.6G  0 disk
fmadio@fmadio20-049:~$
```

To force assembly of the array as follows

```text
$ sudo mdadm --assemble /dev/md0 --force /opt/fmadio/disk/hdd0 /opt/fmadio/disk/hdd1 /opt/fmadio/disk/hdd2 /opt/fmadio/disk/hdd3
mdadm: no RAID superblock on /opt/fmadio/disk/hdd1
mdadm: /opt/fmadio/disk/hdd1 has no superblock - assembly aborted
fmadio@fmadio20-049:~$ 
```

In the above its showing HDD1 is missing \(e.g. the failed drive\). Please remove it \(/opt/fmadio/disk/hdd1\)  from the array and run the assembly command again as follows

```text
fmadio@fmadio20-049:~$ sudo mdadm --assemble /dev/md0 --force /opt/fmadio/disk/hdd0  /opt/fmadio/disk/hdd2 /opt/fmadio/disk/hdd3
mdadm: /dev/md0 has been started with 3 drives (out of 4).
fmadio@fmadio20-049:~$
```

Then get details of the array status with \(mdadm --detail /dev/md0\)

```text
fmadio@fmadio20-049:~$ sudo mdadm --detail /dev/md0
/dev/md0:
        Version : 1.2
  Creation Time : Tue Nov  5 23:31:47 2019
     Raid Level : raid5
     Array Size : 11720662464 (11177.70 GiB 12001.96 GB)
  Used Dev Size : 3906887488 (3725.90 GiB 4000.65 GB)
   Raid Devices : 4
  Total Devices : 3
    Persistence : Superblock is persistent

  Intent Bitmap : Internal

    Update Time : Tue May 25 15:07:01 2021
          State : clean, degraded
 Active Devices : 3
Working Devices : 3
 Failed Devices : 0
  Spare Devices : 0

         Layout : left-symmetric
     Chunk Size : 64K

           Name : fmadio20-049:0  (local to host fmadio20-049)
           UUID : 5f5b2011:dde52c71:88d6fcd4:0930bb91
         Events : 2

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       2       8       48        2      active sync   /dev/sdd
       6       0        0        6      removed
fmadio@fmadio20-049:~$
```

Notice in this case the "State" is "clean, degraded" and one drive is removed

![](.gitbook/assets/image%20%2831%29.png)



### 4\) Add the new drive to the array

Next add the new drive to the array using the \(mdadm --add /dev/md0 \) command passing in the mount point of the new drive. In this case we are adding /opt/fmadio/hdd1

```text
fmadio@fmadio20-049:~$ sudo mdadm --add /dev/md0 /opt/fmadio/disk/hdd1
mdadm: added /opt/fmadio/disk/hdd1
fmadio@fmadio20-049:~$
```

Then check the status of the updated array

```text
fmadio@fmadio20-049:~$ sudo mdadm --detail /dev/md0
/dev/md0:
        Version : 1.2
  Creation Time : Tue Nov  5 23:31:47 2019
     Raid Level : raid5
     Array Size : 11720662464 (11177.70 GiB 12001.96 GB)
  Used Dev Size : 3906887488 (3725.90 GiB 4000.65 GB)
   Raid Devices : 4
  Total Devices : 4
    Persistence : Superblock is persistent

  Intent Bitmap : Internal

    Update Time : Wed May 26 07:14:31 2021
          State : clean, degraded, recovering
 Active Devices : 3
Working Devices : 4
 Failed Devices : 0
  Spare Devices : 1

         Layout : left-symmetric
     Chunk Size : 64K

 Rebuild Status : 0% complete

           Name : fmadio20-049:0  (local to host fmadio20-049)
           UUID : 5f5b2011:dde52c71:88d6fcd4:0930bb91
         Events : 9

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       2       8       48        2      active sync   /dev/sdd
       4       8       64        3      spare rebuilding   /dev/sde
fmadio@fmadio20-049:~$
```

Can see the new state says its "clean, degraded, recovering" and individual disk state is "spare rebuilding"

During  the rebuild, the write back performance from SSD to HDD will be reduced, however capture rate to the SSDs \(1TB-4TB\) is not impacted at all. e.g. full 20Gbps line rate is no problem.

