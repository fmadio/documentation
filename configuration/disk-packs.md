# Disk Packs

FMADIO100 Capture system supports multiple data "Disk Packs" per capture system.

The system at boot time detect which "Disk Pack" that is currently installed. It does this by checking the first Data disks serial number against all disk configurations. When it finds a match, that disk configuration will be used as the currently active disk configuration file.

### Setup

Initial setup requires the "disk.boot" configuration file to be copied in the directory

```
/opt/fmadio/etc/
```

Latest version can be downloaded from our GitHub repo linked here

[https://github.com/fmadio/public/blob/master/etc/disk.boot](broken-reference)

### Adding Disk Packs

Next step is to add diskpack configurations in the config directory

```
/opt/fmadio/etc/
```

Each disk pack configuration file is named

```
/opt/fmadio/etc/disk_0.lua
/opt/fmadio/etc/disk_1.lua
/opt/fmadio/etc/disk_2.lua
/opt/fmadio/etc/disk_3.lua
.
.
.
/opt/fmadio/etc/disk_49.lua
```

There is no hard limit to number of Disk Packs, the numbers do not need to be sequential with a soft limit currently set to 50.

Each disk configuration file is a standard disk.lua config, an example of 100G Gen2 configuration file is below. It lists the serial numbers and mount point for each of the disks in the system

```lua
return {
["RaidDisk"] =
{
},
["RaidDevice"] =
{
},
["CacheDisk"] =
{
    ["S463NF0M405144R"]   = "ssd0",
    ["S463NF0M405016N"]   = "ssd2",
    ["S463NFJ0T707659"]    = "ssd3",
    ["S463NFJ0T707658"]    = "ssd4",
    ["S463NFJ0T707662"]    = "ssd5",
    ["S463NFJ0T707655"]    = "ssd6",
    ["S463NFJ0T707793"]    = "ssd7",
},
["ScratchDisk"] =
{
},
["ParDisk"] =
{
    ["S463NFJ0T707660"]    = "par0",
},
["HotDisk"] =
{
},
["OSDisk"] =
{
    ["1949256760C6"]    = "os0",
},
IndexDisk = "ssd",
CacheLevel = "full",
RaidLevel = "raid0",
["NFSDisk"] =
{ua
},
["CIFSDisk"] =
{
},
}

```

This is then replicated with each disk packs serial numbers for each disk\_X.lua configuration file

### Hardware Install

FMADIO100Gv2 capture system has 10 x NVMe U.2 drive slots, of these 9 of the disks are used for data, and one disk is used for the OS. Below is the location of the **OS disk. The OS disk can not be removed or swapped.**

The location of the remaining 9 data disks when installed can be in any order.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Location of the OS Disk</p></figcaption></figure>

#### 1) Install Setup

Power down the system

#### 2) Install Setup

Remove all 9 data disks from the current chassis and place into the provided storage case.

#### 3) Install Setup

Insert each of the new 9 disks into any location. Be careful to not use excessive force as it may damage the backplane and device.

#### 4) Install Setup

Power on the device, it will take 90-120sec to fully boot. Once fully booted check the disks have been recogined by running "ls" in the directory

```
/opt/fmadio/disk/
```

Correct listing will look like below, ssd0 - ssd7 and par0 are mapped to a physical end point. If any disk is missing, please reseat the disk and reboot the system.

```
fmadio@fmadio100v2-228U:~$ ls -al /opt/fmadio/disk/
total 0
drwxr-xr-x    2 root     root           380 Sep 29 12:23 ./
drwxr-xr-x   13 fmadio   staff          460 Sep 29 12:23 ../
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 par0 -> /dev/nvme9n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd0 -> /dev/nvme5n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd0_capture -> /dev/nvme5n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd1 -> /dev/nvme6n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd1_capture -> /dev/nvme6n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd2 -> /dev/nvme7n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd2_capture -> /dev/nvme7n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd3 -> /dev/nvme8n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd3_capture -> /dev/nvme8n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd4 -> /dev/nvme4n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd4_capture -> /dev/nvme4n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd5 -> /dev/nvme2n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd5_capture -> /dev/nvme2n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd6 -> /dev/nvme3n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd6_capture -> /dev/nvme3n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd7 -> /dev/nvme1n1
lrwxrwxrwx    1 root     root            12 Sep 29 12:23 ssd7_capture -> /dev/nvme1n1
fmadio@fmadio100v2-228U:~$

```

### 5) System ready&#x20;

System is now ready for capture and operation
