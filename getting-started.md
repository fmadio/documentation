# Getting Started

## Connecting

There are 4 ways to connect to the appliance for initial configuration:

KVM – The appliance has a VGA port for a monitor and USB for keyboard.\
Serial – The serial port can be used with a configuration of 115200, 8, None, 1.\
SSH to the OS – There are 2 management ports preconfigured with IP addresses:

```
man0 (1GbE copper): 192.168.0.95 / 24
man10 (10GbE SFP+): 192.168.15.10 / 24
```

SSH to the IPMI – The IPMI port is preconfigured with IP address:&#x20;

```
bmc (1GbE copper):       192.168.0.93 / 24
```

Example, is setting a secondary IP address on the switch directly connected to the device. Using a 192.168.0.0/24 IP and address. Then ssh from the switch to the system.

## Show Network Status

To show the current network status use the fmadiocli utility, by typing it on the shell after logging in.

```
fmadiocli
```

Example

```
Last login: Fri Jun  9 10:36:05 2023 from 192.168.187.30
  _____                    .___.__      20Gv3
_/ ____\_____  _____     __| _/|__|  ____
\   __\/     \ \__  \   / __ | |  | /  _ \
 |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
 |__| |__|_|  /(____  /\____ | |__| \____/
            \/      \/      \/
============================================
  -+ all your packet are belong to us +-
fmadio@fmadio20v3-287:~$ fmadiocli
fmad fmadlua Jun  7 2023 (/opt/fmadio/bin/fmadiolua --nocal /opt/fmadio/bin/fmadiocli )
Disable cycle calibration
[Fri Jun  9 19:10:13 2023]   _____                    .___.__
[Fri Jun  9 19:10:13 2023] _/ ____\_____  _____     __| _/|__|  ____
[Fri Jun  9 19:10:13 2023] \   __\/     / \__  \   / __ | |  | /  _ \
[Fri Jun  9 19:10:13 2023]  |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
[Fri Jun  9 19:10:13 2023]  |__| |__|_|  /(____  /\____ | |__| \____/
[Fri Jun  9 19:10:13 2023]             \/      \/      \/
[Fri Jun  9 19:10:13 2023] ============================================
[Fri Jun  9 19:10:13 2023]   -+ Packets confiscated by Customs +-
[Fri Jun  9 19:10:13 2023]
[Fri Jun  9 19:10:13 2023]  type '?' for command information
[Fri Jun  9 19:10:13 2023]  type '???' for verbose information
[Fri Jun  9 19:10:13 2023]
[Fri Jun  9 19:10:13 2023] History: 29
[Fri Jun  9 19:10:13 2023] >
```

To show the current network IP and Link status use the command

```
show interface ip
```

```
show interface status
```

### Example as follows

```
[Fri Jun  9 19:11:46 2023] > show interface ip
[Fri Jun  9 19:11:46 2023] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Fri Jun  9 19:11:46 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:11:48 2023] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Fri Jun  9 19:11:48 2023] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Fri Jun  9 19:11:48 2023] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Fri Jun  9 19:11:48 2023] man10      static     192.168.91.50   255.255.255.0
[Fri Jun  9 19:11:48 2023] cap0
[Fri Jun  9 19:11:48 2023] cap1
[Fri Jun  9 19:11:48 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:11:48 2023] >
```

```
[Fri Jun  9 19:12:19 2023] > show interface status
[Fri Jun  9 19:12:19 2023] Port       Description Status          Speed         Transciver    RxPower    TxPower    Temperature FEC              Vendor            Vendor PN
[Fri Jun  9 19:12:19 2023] -----------------------------------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:12:19 2023] man0                  connected       1G
[Fri Jun  9 19:12:19 2023] man10                 connected       10G          10G Base-SR  0.6117 mW  0.1585 mW        34.00 C                  fmadio         SFP-10GSR-85
[Fri Jun  9 19:12:19 2023] cap0                  notconnected    10G                                                           auto-off
[Fri Jun  9 19:12:19 2023] cap1                  connected       10G               10G SR   4.391 mW   3.498 mA       30.640 C auto-off            OEM            SFP-10-SR
[Fri Jun  9 19:12:19 2023] >

```

NOTE: system has no way to check the BMC link up/down status.

## Configure New Network IP

Once logged in, the first task is to set new IP addresses. Type fmadiocli to enter the configuration CLI, then use the following commands

To set the IPv4 Address

```
config interface ip <interface name> <IPv4 address>
Example: config interface ip man0 192.168.187.10
```

To set the IPv4 netmask

```
config interface netmask <interface name> <subnet mask>
Example: config interface netmask man0 255.255.255.0
```

To set the IPv4 Gateway

```
config interface gateway <interface> <IPv4 gateway address>
Example: config interface gateway man0 192.168.187.30
```

To set the IPv4 DNS (optional)

```
config interface <interface> <IPv4 DNS address>
Example: config interface dns man0 1.1.1.1
```

Full documentation is at&#x20;

{% embed url="https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#configure-interfaces" %}

### Example setup process is shown below

Using IPMITOOL serial port to login

```
# ipmitool  -U admin -P ***** -H 192.168.0.93 -I lanplus sol activate
[SOL Session operational.  Use ~? for help]

fmadio20v3
fmadio20v3-287 login: fmadio
Password:
  _____                    .___.__      20Gv3
_/ ____\_____  _____     __| _/|__|  ____
\   __\/     \ \__  \   / __ | |  | /  _ \
 |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
 |__| |__|_|  /(____  /\____ | |__| \____/
            \/      \/      \/
============================================
  -+ all your packet are belong to us +-
fmadio@fmadio20v3-287:~$ fmadiocli
fmad fmadlua Jun  7 2023 (/opt/fmadio/bin/fmadiolua --nocal /opt/fmadio/bin/fmadiocli )
Disable cycle calibration
[Fri Jun  9 19:17:50 2023]   _____                    .___.__
[Fri Jun  9 19:17:50 2023] _/ ____\_____  _____     __| _/|__|  ____
[Fri Jun  9 19:17:50 2023] \   __\/     / \__  \   / __ | |  | /  _ \
[Fri Jun  9 19:17:50 2023]  |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
[Fri Jun  9 19:17:50 2023]  |__| |__|_|  /(____  /\____ | |__| \____/
[Fri Jun  9 19:17:50 2023]             \/      \/      \/
[Fri Jun  9 19:17:50 2023] ============================================
[Fri Jun  9 19:17:50 2023]   -+ Packets confiscated by Customs +-
[Fri Jun  9 19:17:50 2023]
[Fri Jun  9 19:17:50 2023]  type '?' for command information
[Fri Jun  9 19:17:50 2023]  type '???' for verbose information
[Fri Jun  9 19:17:50 2023]
[Fri Jun  9 19:17:50 2023] History: 32
[Fri Jun  9 19:17:53 2023] > 
```

Configure the IPv4 man0 interface

```
[Fri Jun  9 19:18:13 2023] > config interface ip man0 192.168.187.10
[Fri Jun  9 19:18:15 2023] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
[Fri Jun  9 19:18:15 2023] UPDATING: fmad fmadlua Jun  7 2023 (/usr/local/bin/fmadiolua --nocal /opt/fmadio/bin/setup_network.lua --nocal )
[Fri Jun  9 19:18:15 2023] UPDATING: Disable cycle calibration
[Fri Jun  9 19:18:15 2023] UPDATING: network config [Fri Jun  9 19:18:15 2023] fmadio20v3
.
.
.
[Fri Jun  9 19:18:18 2023] UPDATING: Network Setup... Done
[Fri Jun  9 19:18:18 2023] UPDATING: done 3.184404Sec 0.053073Min
[Fri Jun  9 19:18:18 2023] UPDATING: cat /opt/fmadio/etc/hosts >> /etc/hosts
[Fri Jun  9 19:18:18 2023] set interface [man0] ip (192.168.187.10) -> (192.168.187.10)
[Fri Jun  9 19:18:18 2023]
[Fri Jun  9 19:18:18 2023] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Fri Jun  9 19:18:18 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:18:20 2023] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Fri Jun  9 19:18:20 2023] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Fri Jun  9 19:18:20 2023] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Fri Jun  9 19:18:20 2023] man10      static     192.168.91.50   255.255.255.0
[Fri Jun  9 19:18:20 2023] cap0
[Fri Jun  9 19:18:20 2023] cap1
[Fri Jun  9 19:18:20 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:18:39 2023] > 
```

Configure the IPv4 man0 netmask

<pre><code>[Fri Jun  9 19:18:39 2023] > config interface netmask man0 255.255.255.0
[Fri Jun  9 19:18:40 2023] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
[Fri Jun  9 19:18:40 2023] UPDATING: fmad fmadlua Jun  7 2023 (/usr/local/bin/fmadiolua --nocal /opt/fmadio/bin/setup_network.lua --nocal )
[Fri Jun  9 19:18:40 2023] UPDATING: Disable cycle calibration
[Fri Jun  9 19:18:40 2023] UPDATING: network config [Fri Jun  9 19:18:40 2023] fmadio20v3
[Fri Jun  9 19:18:40 2023] UPDATING: load intel 10G driver
<strong>[Fri Jun  9 19:18:41 2023] UPDATING: load intel 1G driver
</strong>[Fri Jun  9 19:18:43 2023] UPDATING: [0/10] phy0:up phy1:up man10:up   true
.
.
.
[Fri Jun  9 19:18:43 2023] UPDATING: Using Custom hosts
[Fri Jun  9 19:18:43 2023] UPDATING: Network Setup... Done
[Fri Jun  9 19:18:43 2023] UPDATING: done 3.177373Sec 0.052956Min
[Fri Jun  9 19:18:43 2023] UPDATING: cat /opt/fmadio/etc/hosts >> /etc/hosts
[Fri Jun  9 19:18:43 2023] set interface [man0] netmask (255.255.255.0) -> (255.255.255.0)
[Fri Jun  9 19:18:43 2023]
[Fri Jun  9 19:18:43 2023] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Fri Jun  9 19:18:44 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:18:46 2023] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Fri Jun  9 19:18:46 2023] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Fri Jun  9 19:18:46 2023] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Fri Jun  9 19:18:46 2023] man10      static     192.168.91.50   255.255.255.0
[Fri Jun  9 19:18:46 2023] cap0
[Fri Jun  9 19:18:46 2023] cap1
[Fri Jun  9 19:18:46 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:19:19 2023] > 
</code></pre>

Configure IPv4 man0 gateway

```
[Fri Jun  9 19:19:32 2023] > config interface gateway man0 192.168.187.30
[Fri Jun  9 19:19:32 2023] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
[Fri Jun  9 19:19:32 2023] UPDATING: fmad fmadlua Jun  7 2023 (/usr/local/bin/fmadiolua --nocal /opt/fmadio/bin/setup_network.lua --nocal )
[Fri Jun  9 19:19:32 2023] UPDATING: Disable cycle calibration
[Fri Jun  9 19:19:32 2023] UPDATING: network config [Fri Jun  9 19:19:32 2023] fmadio20v3
[Fri Jun  9 19:19:32 2023] UPDATING: load intel 10G driver
[Fri Jun  9 19:19:34 2023] UPDATING: load intel 1G driver
[Fri Jun  9 19:19:35 2023] UPDATING: [0/10] phy0:up phy1:up man10:up   true
.
.
.
[Fri Jun  9 19:19:36 2023] UPDATING: Using Custom hosts
[Fri Jun  9 19:19:36 2023] UPDATING: Network Setup... Done
[Fri Jun  9 19:19:36 2023] UPDATING: done 3.457898Sec 0.057632Min
[Fri Jun  9 19:19:36 2023] UPDATING: cat /opt/fmadio/etc/hosts >> /etc/hosts
[Fri Jun  9 19:19:36 2023] set interface [man0] Gateway (192.168.187.30) -> (192.168.187.30)
[Fri Jun  9 19:19:36 2023]
[Fri Jun  9 19:19:36 2023] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Fri Jun  9 19:19:36 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:19:38 2023] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Fri Jun  9 19:19:38 2023] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Fri Jun  9 19:19:38 2023] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Fri Jun  9 19:19:38 2023] man10      static     192.168.91.50   255.255.255.0
[Fri Jun  9 19:19:38 2023] cap0
[Fri Jun  9 19:19:38 2023] cap1
[Fri Jun  9 19:19:38 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:19:53 2023] > 
```

Configure the IPv4 man0 DNS (optional)

<pre><code>[Fri Jun  9 19:19:53 2023] > config interface dns man0 1.1.1.1
[Fri Jun  9 19:19:53 2023] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
[Fri Jun  9 19:19:53 2023] UPDATING: fmad fmadlua Jun  7 2023 (/usr/local/bin/fmadiolua --nocal /opt/fmadio/bin/setup_network.lua --nocal )
[Fri Jun  9 19:19:53 2023] UPDATING: Disable cycle calibration
[Fri Jun  9 19:19:53 2023] UPDATING: network config [Fri Jun  9 19:19:53 2023] fmadio20v3
[Fri Jun  9 19:19:53 2023] UPDATING: load intel 10G driver
[Fri Jun  9 19:19:55 2023] UPDATING: load intel 1G driver
[Fri Jun  9 19:19:56 2023] UPDATING: [0/10] phy0:up phy1:up man10:up   true
[Fri Jun  9 19:19:56 2023] UPDATING: device man0 already exists; can't create bridge with the same name
[Fri Jun  9 19:19:56 2023] UPDATING: device man1 already exists; can't create bridge with the same name
<strong>.
</strong><strong>.
</strong><strong>.
</strong>[Fri Jun  9 19:19:57 2023] UPDATING: echo "nameserver 1.1.1.1" >> /etc/resolv.conf
[Fri Jun  9 19:19:57 2023] UPDATING: Using Custom hosts
[Fri Jun  9 19:19:57 2023] UPDATING: Network Setup... Done
[Fri Jun  9 19:19:57 2023] UPDATING: done 3.181773Sec 0.053030Min
[Fri Jun  9 19:19:57 2023] UPDATING: cat /opt/fmadio/etc/hosts >> /etc/hosts
[Fri Jun  9 19:19:57 2023] set interface [man0] DNS (nil) -> (1.1.1.1)
[Fri Jun  9 19:19:57 2023]
[Fri Jun  9 19:19:57 2023] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Fri Jun  9 19:19:57 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:19:58 2023] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Fri Jun  9 19:19:58 2023] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Fri Jun  9 19:19:58 2023] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Fri Jun  9 19:19:58 2023] man10      static     192.168.91.50   255.255.255.0
[Fri Jun  9 19:19:58 2023] cap0
[Fri Jun  9 19:19:58 2023] cap1
[Fri Jun  9 19:19:58 2023] ---------------------------------------------------------------------------------------------------------------------------------
[Fri Jun  9 19:19:58 2023] >

</code></pre>

