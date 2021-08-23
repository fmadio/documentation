# Network Configuration \(CLI\)

Modifying the network configuration setting in a restricted Colocation environment can be far easier to achieve via the command line. The first step is SSH into the system, change to the specified directory and view the current network settings, as shown below. 

Specifically:

| interface name | Hardware Port | Description                                                  |
| :--- | :--- | :--- |
| man0 | L1 1G RJ45 | 1G management port |
| bmc | IPMI 1G RJ45 | 1G Out of bands management port |
| man10 | SFP/QSFP 10G/40G | High speed management port |



### Default Settings

| Interface Name | IP Address | Netmask | Gateway | DNS |
| :--- | :--- | :--- | :--- | :--- |
| man0 | 192.168.0.95 | 255.255.255.0 | 192.168.0.1 | 192.168.0.1 |
| bmc | 192.168.0.93 | 255.255.255.0 | 192.168.0.1 |  |
| man10 | 192.168.15.95 | 255.255.255.0 |  |  |

Example configuration is shown below

```text
fmadio@fmadio10-049:/mnt/store0/etc$ cat network.lua
local Config =
{
["man0"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.0.95",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.0.1",
    ["DNS"]     = "192.168.0.1",
},
["man10"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.15.95",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "",
    ["DNS"]     = "",
},
["bmc"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.0.93",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "",
    ["DNS"]     = "",
},
}
return Config
```

After editing the file, save it and confirm no syntax errors, running the following command

```text
fmadiolua /opt/fmadio/etc/network.lua
```

A successful output looks like the following

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$ fmadiolua /opt/fmadio/etc/network.lua
loading filename [/opt/fmadio/etc/network.lua]
done 0.000052Sec 0.000001Min
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$
```

Reboot the system and confirm the IP settings are correct

### Management VLAN

There may be a requirement for management VLAN interfaces on the FMADIO packet capture system. These are simple to add using the standard linux infrastructure

Example is to create a VLAN 123 on the 10G man10 interface. Add the following configuration to 

```text
/opt/fmadio/etc/network.lua
```

Create a new entry names "man10.123" as follows

```lua
["man10.123"] =
{
    ["Mode"]    = "static",
    ["vlan"]    = "123",
    ["Address"] = "192.168.123.2",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "",
    ["DNS0"]    = "",
    ["DNS1"]    = "",
    ["Speed"]   = "10g",
    ["TSMode"]  = "nic",
},

```

The only difference is the addition of  the "vlan" setting and an entry named "man10.123" all other fields are configured as a normal static interface.

After configuring be sure to run the following to confirm the configuration file is formatted correctly. The following is the correct output

```text
fmadio@fmadio20n40v3-364:~$ fmadiolua /opt/fmadio/etc/network.lua
fmad fmadlua Aug 23 2021
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/network.lua]
done 0.000047Sec 0.000001Min
fmadio@fmadio20n40v3-364:~$

```

### Changing BMC/IPMI Network Settings

In addition to the above, if the BMC information has been changed, please run the following command to configure BMC IP address on the system.

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$ sudo ./setup_network.lua  --updatebmc
```

Example output of BMC IP configuration is shown below

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$ sudo ./setup_network.lua  --updatebmc
loading filename [./setup_network.lua]
network config [Mon Jul  5 02:19:44 2021] fmadio40v3
load intel 10G driver
rmmod: can't unload 'ixgbe': Device or resource busy
insmod: can't insert '/opt/fmadio/drivers/current/ixgbe.ko': File exists
load intel 1G driver
rmmod: can't unload 'igb': Device or resource busy
insmod: can't insert '/opt/fmadio/drivers/current/igb.ko': File exists
[0/10] phy0:up phy1:up man10:up   true
/bin/brctl addbr man0
device man0 already exists; can't create bridge with the same name
/bin/brctl addif man0 phy0
device phy0 is already a member of a bridge; can't enslave it to bridge man0.
/bin/brctl addbr man1
device man1 already exists; can't create bridge with the same name
/bin/brctl addif man1 phy1
device phy1 is already a member of a bridge; can't enslave it to bridge man1.
/bin/brctl addbr man10
device man10 already exists; can't create bridge with the same name
/bin/brctl addif man10 phy10
interface phy10 does not exist!
/sbin/ifconfig phy0 up
/sbin/ifconfig phy1 up
/sbin/ifconfig phy10 up
.
.
.
.
BMC Setting: Addr:192.168.2.223 Netmask:255.255.255.0 GW:192.168.2.254
/sbin/ifconfig man0 192.168.2.225 netmask 255.255.255.0
/sbin/ifconfig man10 192.168.15.225 netmask 255.255.255.0
/sbin/ifconfig man10 mtu 9200
/sbin/route add -net default gw 192.168.2.254
route: SIOCADDRT: File exists
echo "" > /etc/resolv.conf
echo "nameserver 192.168.2.254" >> /etc/resolv.conf
/usr/sbin/ipmitool lan set 1 ipaddr 192.168.2.223
Setting LAN IP Address to 192.168.2.223
/usr/sbin/ipmitool lan set 1 netmask 255.255.255.0
Setting LAN Subnet Mask to 255.255.255.0
/usr/sbin/ipmitool lan set 1 defgw ipaddr 192.168.2.254
Setting LAN Default Gateway IP to 192.168.2.254
Network Setup... Done
done 4.552995Sec 0.075883Min
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$

```

Once Complete confirming the BMC IP network settings using the following command

```text
sudo ipmitool lan print
```

Example output looks like the following

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$ sudo ipmitool lan print
Set in Progress         : Set Complete
Auth Type Support       : NONE MD2 MD5 PASSWORD OEM
Auth Type Enable        : Callback : MD5
                        : User     : MD5
                        : Operator : MD5
                        : Admin    : MD5
                        : OEM      : MD5
IP Address Source       : Static Address
IP Address              : 192.168.0.93
Subnet Mask             : 255.255.255.0
MAC Address             : 18:c0:4d:b4:0e:72
SNMP Community String   : AMI
IP Header               : TTL=0x40 Flags=0x40 Precedence=0x00 TOS=0x10
BMC ARP Control         : ARP Responses Enabled, Gratuitous ARP Disabled
Gratituous ARP Intrvl   : 1.0 seconds
Default Gateway IP      : 192.168.0.1
Default Gateway MAC     : 10:da:43:c8:03:ac
Backup Gateway IP       : 0.0.0.0
Backup Gateway MAC      : 00:00:00:00:00:00
802.1q VLAN ID          : Disabled
802.1q VLAN Priority    : 0
RMCP+ Cipher Suites     : 0,1,2,3,6,7,8,11,12,15,16,17
Cipher Suite Priv Max   : caaaaaaaaaaaXXX
                        :     X=Cipher Suite Unused
                        :     c=CALLBACK
                        :     u=USER
                        :     o=OPERATOR
                        :     a=ADMIN
                        :     O=OEM
Bad Password Threshold  : 0
Invalid password disable: no
Attempt Count Reset Int.: 0
User Lockout Interval   : 0
fmadio@fmadio20n40v3-363:/opt/fmadio/bin$
```

After confirming the IP address, BMC IP address is pingable also reachable with IPMITOOL, HTTP and HTTPS.

