# Global Time PTPv2

FMADIO capture systems support PTPv2 using the linux PTP open source project. ( [http://linuxptp.sourceforge.net/](http://linuxptp.sourceforge.net/) )  running only on the 10G management interface. 1G links are supported using 1G RJ45 SFP transceiver.

Enabling PTPv2 via GUI is as follows

![PTPv2 Time Selection](<../.gitbook/assets/image (109).png>)

In addition the 10G Bridged networking config must be disabled. As LinuxPTP must run directly on the bare metal NIC. Please edit the file

```
/opt/fmadio/etc/60-persistent-ethernet.rules
```

Change the "phy10" values as shown below to "man10"

![Before Bare Metal NIC](<../.gitbook/assets/image (107).png>)

Changed to "man10" as follows

![Enable Bare metal 10G Interface](<../.gitbook/assets/image (108).png>)

System must be rebooted for the above to take effect.

## PTPv2 VLAN Interface

In some environments PTPv2 is run on its own dedicated VLAN interface. To enable this update the configuration file

```
/opt/fmadio/etc/time.lua
```

Specifically setting the VLANID as follows, in the below example its set to VLANID "123"

```lua
["PTP"] =
{
    ["Master0"] = "",
    ["Master1"] = "",
    ["Master2"] = "",
    ["Master3"] = "",
    ["UpdateRate"] = "",
    ["VLANID"] = "123",
    ["Interface"] = "man10",
},

```

After editing save and confirm the file syntax is correct, ensure there are no syntax or parse errors. The  correct output is shown below.

```
fmadio@fmadio20n40v3-364:~$ fmadiolua /opt/fmadio/etc/time.lua
fmad fmadlua Aug 10 2021
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/time.lua]
done 0.000051Sec 0.000001Min
fmadio@fmadio20n40v3-364:~$

```

Either reboot the system, or kill all ptp processes (e.g. sudo killall ptp4l; sudo killall phc2sys)

On reboot a man10.123  VLAN interface that matches the set VLANID should be populated as follows

```
fmadio@fmadio20n40v3-364:~$ ifconfig man10.123
man10.123 Link encap:Ethernet  HWaddr 18:C0:4D:17:7A:4E
          BROADCAST MULTICAST  MTU:9200  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

fmadio@fmadio20n40v3-364:~$

```

And the linux ptp daemo should have binded  to the newly created instance as follows

```
fmadio@fmadio20n40v3-364:~$ ps aux |grep ptp4l
64727 root     /opt/fmadio/bin/ptp4l -4 -H -i man10.123 -m -s
fmadio@fmadio20n40v3-364:~$
```

And the PHC interface also

```
fmadio@fmadio20n40v3-364:~$ ps aux |grep phc2sys
64729 root     /opt/fmadio/bin/phc2sys -s man10.123 -c CLOCK_REALTIME -w -m -P 0.7 -I 0.3
fmadio@fmadio20n40v3-364:~$

```

## PTPv2 on Second Port

The above config assumes PTPv2 is running on the primary 10G management port. There is not requirement for using the primary port, instead the 2nd 10G management port can also be used. Below are the differences required

1\) in the udev rules phy10 -> phy11 -> man11

Use phy11 interface

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Rename to man11

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

2\) Set the IP address of man11

man11 needs to be configured with an IP address, either editing  network config file directly

```
/opt/fmadio/etc/network.lua
```

or using the fmadiocli command line utility



3\) Specify man11 in the ptp config per below

```
["PTP"] =
{
    ["Master0"] = "",
    ["Master1"] = "",
    ["Master2"] = "",
    ["Master3"] = "",
    ["UpdateRate"] = "",
    ["VLANID"] = nil,
    ["Interface"] = "man11",
},
```

## Custom LinuxPTP Config

FMADIO system use the standard LinuxPTP project for time synchronization

{% embed url="http://linuxptp.sourceforge.net" %}
Linux PTP Configuration
{% endembed %}

As such the PTP configuration is highly customizable. For example Broadcast environments use a non standard domain and update rate. FMADIO support this by using a custom LinuxPTP configuration file.

Custom config file can be located below

```
/opt/fmadio/etc/ptp4l.conf
```

If this file is present the system will use it when starting the application ptp4l.&#x20;

After creating or editing the file, need to stop ptp4l as follows

```
// Some code
sudo killall ptp4l
sudo killall phc2sys
```

The system will then automatically start it on a 60sec timer.&#x20;

Logfiles for debugging can be found in&#x20;

```
// Some code
/mnt/store0/log/ptp_ptp4l.log
/mnt/store0/log/ptp_phc2sys.log
```

## PPS Time Synchronization

FMADIO Devices have a PPS input connect that expect 1 1PPS 50ohm 5V signal. The start of second boundary occurs on the rising edge of the PPS clock. Required hold time is 100usec or more.

Enabling of PPS is automatic, when the system detects PPS input, it automatically selects PPS to discipline the timestamp clock.

