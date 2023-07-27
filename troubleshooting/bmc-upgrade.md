# BMC Upgrade

## BMC Version 12.41.13

Upgrading the BMC software is simple process but requires physical access to the system. Physical access is required as the power cables need to be disconnected after BMC upgrade has been completed.

### Step 1)

Retrieve the current BMC version as follows

```
sudo ipmitool bmc info
```

Example output

```
fmadio@fmadio100v2-228U:/opt/fmadio/firmware/bmc$ sudo ipmitool bmc info
Device ID                 : 32
Device Revision           : 1
Firmware Revision         : 12.41
IPMI Version              : 2.0
Manufacturer ID           : 15370
Manufacturer Name         : Unknown (0x3C0A)
Product ID                : 308 (0x0134)
Product Name              : Unknown (0x134)
Device Available          : yes
Provides Device SDRs      : yes
Additional Device Support :
    Sensor Device
    SDR Repository Device
    SEL Device
    FRU Inventory Device
    IPMB Event Receiver
    IPMB Event Generator
    Chassis Device
Aux Firmware Rev Info     :
    0x0b
    0x00
    0x00
    0x00
fmadio@fmadio100v2-228U:/opt/fmadio/firmware/bmc$

```

The key value difference is shown below, the firmware gets upgraded to version 0xd shown below. If the system already shows version 0xd there is no need to upgrade the BMC software

<figure><img src="../.gitbook/assets/image (4) (2) (1).png" alt=""><figcaption></figcaption></figure>

### Step 2)

Run the BMC update  in the following directory

```
cd /opt/fmadio/firmware/bmc/
```

Then run the update program

```
sudo ./flash64.sh
```

Enter Y for preserve configuration settings.

The process will take several minutes to complete

```
fmadio@fmadio100v2-228U:/opt/fmadio/firmware/bmc$ sudo ./flash64.sh
chmod: gigaflash_x64: Read-only file system
chmod: socflash_x64: Read-only file system
gigaflash v1.6.3
Do you want to preserve configuration? (Y/n)
Y
Loading Firmware...
Update Firmware
Wait 90 seconds for BMC Ready...
fmadio@fmadio100v2-228U:/opt/fmadio/firmware/bmc$

```

### Step 3)

Disconnect the AC power from the system. Wait for 1 minute

### Step 4)

Reconnect AC power to the system

### Step 5)

Wait 5minutes for the BMC to fully reboot and host system boot

### Step 6)

Reboot the linux Host server

```
sudo reboot
```

### Step 7)

After host linux system has rebooted, Check BMC version is updated. It should show version 0xD per image below

```
sudo ipmitool bmc info
```

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

### &#x20;Step 8)

Update is complete



## BMC Upgrade Version 12.60.39

BMC upgrade to version 12.60.39 resolves CVEs and makes possible boot without prompt when enabling full disk encryption.

From FW: 8940 the BMC firmware is located in

```
/opt/fmadio/firmware/bmc/
```

As the version jump from the factory installed to this version is very large, all BMC settings are lost durning the upgrade, and need to be set again.

**This includes BMC Network information, meaning BMC network connectivity will be lost during this process**

In addition BMC passwords and other items are also lost. The way to update this is via the FMADIO x86 Host system, where the host is always powered on enabling it to set the BMC Network settings and User passwords directly.

### Step 1)

Start the BMC update process using the following commands on the FMADIO host system



```
cd /opt/fmadio/firmware/bmc
```

```
sudo bash bmc_fw_update_linux.sh
```

Example output shown below, it will take about 5minutes to run. Note the warning, performing a full flash update is OK, enter Y

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Step 2)

After the BMC is flashed and has rebooted confirm the new BMC version is 12.60.39 using the following command

```
sudo ipmitool bmc info
```

The output should look like the following

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Step 3)

Set the new network configuration information.

Set to use static ip per below

```
sudo ipmitool lan set 1 ipsrc static
```

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Set a new IP address, netmask and gateway, replace addresses with the assigned BMC network address

```
sudo ipmitool lan set ipaddr 192.168.2.173
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

```
sudo ipmitool lan set 1 netmask 255.255.255.0
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

```
sudo ipmitool lan set 1 defgw ipaddr 192.168.2.254
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

At this point the network should be reachable via ping, however the username password will be reverted to the default setting.

Confirm the network settings using the command

```
sudo ipmitool lan print
```

Example output is shown below

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

The BMC webpage should be accessiable at this point.

### Step 4)

The BMC update will delete all the settings, these need to be added back

First one is to set the admin password as follows, replacing "secret" with the password chosen

```
sudo ipmitool user set password 2 secret
```

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

After setting this, logging into the BMC using the admin account and above password.

### Step 5)

As all BMC settings are disabled, the first critical setting is a custom FAN profile. Usually this is set at the factory however it needs to be created again after the BMC update

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Then copy the default fan profile as follows

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

The new fan profile is named "fmadio100v2" with the following settings. Ensure all CPU sensor and all FANs are selected

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Click Save

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Finally activate the profile

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Step 6)

BIOS update, go to the Firmware Update page

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Choose the R23 BIOS file provided by the support team, and start the update process

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Select BIOS, then proceed to Flash

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

It will present something like this as it goes thru the process, will take a few minutes only.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Once completed the system needs to be rebooted, either using the BMC or the FMADIO linux command line.

### Step 7)

After BIOS update all settings are lost and need to be set-again, the system will fail to boot also as the BIOS settings have not been configured

Setting the Boot settings as follows

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Advanced -> Trusted computing set the following

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Advacne -> Serial Port Console

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Advanced -> Network Stack&#x20;

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Advanced -> Chipset Configuration

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Advacned -> PCIe Configuration. Please set exactly as shown below

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Advanced -> NVMe Configuration

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Security -> Secure Boot

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Save changes and exit

### Step 8)

The system should boot normally now without any BIOS password prompt.

Finished... horary.



