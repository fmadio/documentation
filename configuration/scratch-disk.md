# Scratch Disk

Scratch Disks are helpful for keeping temporary or persistant data outside of the FMADIO host capture system. Many times the OS disks is unable to be used due to capacity or location.

Scratch Disks can be run as a RAID0 or RAIDx group of drives, allow you the system flexible allocation of the total storage capacity of the system between Capture vs Analytics.

## ScratchDisk Configuration

### Step 1) Assign physical disks

Start by editing the disk configuration file

```
/opt/fmadio/etc/disk.lua
```

Then moving serial numbers from the CacheDisk section to the ScratchDisk section.&#x20;

For example moving a single disk (serial number "22443E9DC54E") from ssd8 -> scr0

Before:

```
  CacheDisk =
    {
        ["22443E9D204F"]        = "ssd0",
        ["22223AD5BFC3"]        = "ssd1",
        ["22443E9D3AFF"]        = "ssd2",
        ["22443E9DC543"]        = "ssd3",
        ["22443E9D2076"]        = "ssd4",
        ["22443E9D3B41"]        = "ssd5",
        ["22443E9D3B65"]        = "ssd6",
        ["22443E9D20A4"]        = "ssd7",
        ["22443E9DC54E"]        = "ssd8",
    }
    ,
    ParDisk =
    {
        ["22443E9D2087"]        = "par0",
    }
    ,
    RaidDisk =
    {
    }
    ,
    ScratchDisk =
    {
    }
    ,
    OSDisk =
    {
    ["50026B7685513F33"] = "os0",
    }
    ,

```

renameing ssd8 to scr0

After:

```
    CacheDisk =
    {
        ["22443E9D204F"]        = "ssd0",
        ["22223AD5BFC3"]        = "ssd1",
        ["22443E9D3AFF"]        = "ssd2",
        ["22443E9DC543"]        = "ssd3",
        ["22443E9D2076"]        = "ssd4",
        ["22443E9D3B41"]        = "ssd5",
        ["22443E9D3B65"]        = "ssd6",
        ["22443E9D20A4"]        = "ssd7",
    }
    ,
    ParDisk =
    {
        ["22443E9D2087"]        = "par0",
    }
    ,
    RaidDisk =
    {
    }
    ,
    ScratchDisk =
    {
        ["22443E9DC54E"]        = "scr0",
    }
    ,
    OSDisk =
    {
    ["50026B7685513F33"] = "os0",
    }
```

Save the file.

This can be repeated for as many disks as you require, please keep the numbering sequential e.g. scr0, scr1, scr2, scr3  etc so the system can map it correctly.

### Step 2) Reboot the system

Rebooting is required as the system needs to rename the mount points in /opt/fmadio/disk

### Step 3) Create RAID partition

Create the RAID0 partition using mdmadm as follows. Change the appropriate fields to create a RAID0 2 disk partition or more.

```
sudo mdadm --create /dev/md1 --force --verbose --level=raid0 --raid-devices=1 /opt/fmadio/disk/scr0
```

Or for a 2 disk Scratch Partition

```
sudo mdadm --create /dev/md1 --force --verbose --level=raid0 --raid-devices=2 /opt/fmadio/disk/scr0  /opt/fmadio/disk/scr1
```

The output will look something like below

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Create RAID0 Scratch Drive</p></figcaption></figure>

### Step 3) Create Filesystem&#x20;

The partition now exists on /dev/md1 we need to create a filesystem next so it can be used in a general purpose way. Run the following command to create an EXT4 based Scratch Disk partition

```
sudo mkfs.ext4 /dev/md1
```

Example output looks like below

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Step 4) Confirm the filesystem mounts correct

Next confirm th file system mounts correctly, by creating a test file on the partition

```
sudo mount /dev/md1 /mnt/store1
```

Then creating a file and confirming, such as below

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Step 5) Confirm automatic mounting

Reboot the system and confirm the ScratchDisk gets mounted automatically on boot to&#x20;

```
/mnt/store1
```

The test file created above should exit, example below

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

## Capture Metadata on ScratchDisk

In some cases its best to locate the Capture Filesystems Metadata onto the scratch disk. A few extra steps in addition to the above steps are required.

### Step 1) Create stream directory

Create a stream directory on the ScratchDisk such as below

```
sudo mkdir stream
```

Example output looks like the following

```
fmadio@fmadio100p3-539:/mnt/store1$ sudo mkdir stream
fmadio@fmadio100p3-539:/mnt/store1$ ls -al stream
total 8
drwxr-xr-x    2 root     root          4096 Sep  7 20:26 ./
drwxr-xr-x    4 root     root          4096 Sep  7 20:26 ../
fmadio@fmadio100p3-539:/mnt/store1$
```

### Step 2) set location of the Capture systems metadata.

Edit the file&#x20;

```
/opt/fmadio/etc/disk.lua
```

Near the end of the file create a one line entry as follows

```
StreamPath = "/mnt/store1/stream/",
```

Example output looks like

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Step 3) Reboot the system

Reboot the system

### Step 4) Confirm stream path is pointing to the correct location

Run the command

```
ls -al /opt/fmadio/
```

Confirm the /opt/fmadio/stream symblic link is pointing to the correct location, example below.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### Step 5) Quick Format

Run a quick format via the GUI to reset the storage partition. This changes the number of drives used for the capture file system.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

The above will take about 5 minutes and reboot 2 times by itself.

### Step 6) Confirm filesystem metadata is on the ScratchDisk

Check the directory listing

```
ls -al /mnt/store1/stream/
```

Example looks as follows

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Step 7) Finished

After this the file system can be moved along with the diskpacks.
