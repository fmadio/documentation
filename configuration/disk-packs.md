# Disk Packs

FMADIO100 Capture system supports multiple data "Disk Packs" per capture system.

The system at boot time detect which "Disk Pack" that is currently installed, and uses that as the currently active config file.&#x20;

## Disk Swap

FMADIO100Gv2 capture system has 10 x NVMe U.2 drive slots, of these 9 of the disks are used for data, and one disk is used for the OS. Below is the location of the OS disk. **The OS disk can not be removed or swapped, must always be inserted at the location below (top left hand corner).**

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>Location of the OS Disk</p></figcaption></figure>

The location of the remaining 9 data disks when installed can be in any order.

#### 1) Disk Swap&#x20;

Power down the system

#### 2) Disk Swap

Remove all 9 data disks from the current chassis and place into the provided storage case.

#### 3) Disk Swap

Insert the new OS disk into the top left slot, then each of the new 9 data disks into any location. Be careful to not use excessive force as it may damage the backplane and device.

#### 4) Disk Swap

Power on the device, it will take 90-120sec to fully boot. Once fully booted check the disks have been recognized by running "ls" in the directory

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

#### 5) Done

System is now ready for capture and operation

## One Time Setup

When a Disk Pack is first delivered, it requires configuration. This is a one time operation that can be done in a lab environment.&#x20;

The steps are as follows

#### &#x20;1) Install the new Disk Pack

Per the above instructions, install the new disk pack hardware. Including OS and 9 data disks.

#### 2) Power on the system

Power on the system, it will have no network connectivity, but serial port and VGA are operational.

#### 3) Configure Management Interface

The system will be behaving oddly, and is partially operational. Connect to the VGA or Serial port and login with the default passwords.

NOTE: SSH network connectivity will not be functional.

#### 4) Setup Management Interface

Manually configure the 1G management interface with ifconfig, example below

```
sudo ifconfig eth0 192.168.2.173
```

<figure><img src="../.gitbook/assets/image (6) (3).png" alt=""><figcaption><p>Manually configuring the 1G interface via HTMl5 BMC </p></figcaption></figure>

The interface may be eth0 or eth1 depending on the config

#### 4) SSH into the system

Once 1G connectivity is established ssh and scp work no problem. SSH into the system

#### 5) Update the udev rules

Update the udev rules with the vendor provided file. Either scp or use a text editor. The file is located in

```
/opt/fmadio/etc/60-persistent-ethernet.rules
```

#### 6) Reboot Done

After copying the file reboot the system. The system should have normal operation.

