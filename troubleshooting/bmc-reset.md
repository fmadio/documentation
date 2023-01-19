# BMC Reset

All FMADIO Packet Capture systems have an onboard BMC, this allows remote power on/off and HTML5 based KVM utility which is very helpful when troubleshooting issues. In addition the BMC provides temperature voltage, current sensors and many other functions.

The chipset is an ASPEED2500 ( [http://www.aspeedtech.com/server\_ast2500/](http://www.aspeedtech.com/server\_ast2500/) ) which runs its own linux kernel + management software. Logging into the BMC via web interface looks like the following

![FMADIO BMC Interface](<../.gitbook/assets/image (61).png>)

There are times where the BMC after long uptime or other issues becomes unstable and requires a reboot

### BMC Reset via IPMITOOL

The easiest way to reset the BMC is via IPMITOOL. Start by confirming the BMC is up and responding by issuing the command&#x20;

```
ipmitool -U admin -P <password> -H host.ip.address bmc info 
```

Example output is shown below

```
aaron@ingress:~$ ipmitool -U admin -P secret -H 192.168.1.1 bmc info
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
    0x0d
    0x00
    0x00
    0x00
aaron@ingress:~$
```

This shows the BMC is responding correctly, Next issue the BMC cold reset function using the command

```
ipmitool -U admin -P <password> -H host.ip.address bmc reset cold
```

Example run as follows

```
aaron@ingress:~$ ipmitool -U admin -P secret -H 192.168.1.1 bmc reset cold
Sent cold reset command to MC
aaron@ingress:~$
```

It will take a few minutes for the BMC Web interface and IPMITOOL tools to become active again.

**NOTE**: Please power cycle the Host system after BMC reboot to fully clear the error condition

## BMC Reset via SSH

If the above fails, a secondary option is possible using SSH into the BMC directly and issuing a reboot. Please contact support@fmad.io for password

```
$ ssh sysadmin@192.168.1.10
sysadmin@192.168.1.10's password:

sysadmin [~]# uptime
 07:23:13 up 21 days,  7:27,  1 users,  load average: 5.09, 5.03, 5.10
 
sysadmin [~]# reboot

The system is going down for reboot NOW!92509FE (pts/1) (Mon Mar 14 07:23:16
sysadmin [~]#
```

**NOTE**: Please power cycle the Host system after BMC reboot to fully clear the error condition. If a powercycle is not possible (due to BMC), a warm reboot can be run first.

## Power Disconnect

In some cases ipmitool is not responsive, thus the only option is a full power cable removal. Using the "power cycle" option on the BMC only resets the x86 server, it does not reboot the BMC. This is a last resort as physical access to the machine can be difficult in some cases.
