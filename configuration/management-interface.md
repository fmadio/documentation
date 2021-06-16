# Management Interface

FMADIO Systems have multiple 1G, 10G and 40G management interfaces, depending on the ordered SKU.

Management interfaces are all bridged by default per the following block diagram

![](../.gitbook/assets/image%20%2870%29.png)

Using the above configuration allows

* LXC containers full IP address \(without NAT\)
* Bonded management mode for Redundancy \(Hot-Standby\)
* Bonded management mode for Throughput \( LAG \)

Example ifconfig of the system is as follows

```text
fmadio@fmadio20n40v3-363:~$ ifconfig -a

man0      Link encap:Ethernet  HWaddr 18:C0:4D:B4:0E:6C
          inet addr:192.168.2.225  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::1ac0:4dff:feb4:e6c/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2371 errors:0 dropped:0 overruns:0 frame:0
          TX packets:136 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:911945 (890.5 KiB)  TX bytes:24130 (23.5 KiB)


man10     Link encap:Ethernet  HWaddr 18:C0:4D:17:7A:4E
          inet addr:192.168.15.225  Bcast:192.168.15.255  Mask:255.255.255.0
          inet6 addr: fe80::1ac0:4dff:fe17:7a4e/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9200  Metric:1
          RX packets:5227 errors:0 dropped:0 overruns:0 frame:0
          TX packets:14 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:253670 (247.7 KiB)  TX bytes:1068 (1.0 KiB)

phy0      Link encap:Ethernet  HWaddr 18:C0:4D:B4:0E:6C
          inet6 addr: fe80::1ac0:4dff:feb4:e6c/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2372 errors:0 dropped:0 overruns:0 frame:0
          TX packets:151 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:945205 (923.0 KiB)  TX bytes:25260 (24.6 KiB)

phy1      Link encap:Ethernet  HWaddr 18:C0:4D:B4:0E:6D
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

phy10     Link encap:Ethernet  HWaddr 18:C0:4D:17:7A:4E
          inet6 addr: fe80::1ac0:4dff:fe17:7a4e/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9200  Metric:1
          RX packets:5262 errors:0 dropped:35 overruns:0 frame:0
          TX packets:27 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:331398 (323.6 KiB)  TX bytes:2066 (2.0 KiB)

phy11     Link encap:Ethernet  HWaddr 18:C0:4D:17:7A:4F
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

fmadio@fmadio20n40v3-363:~$

```

And bridge settings

```text
fmadio@fmadio20n40v3-363:~$ brctl show man0
bridge name     bridge id               STP enabled     interfaces
man0            8000.18c04db40e6c       no              phy0

fmadio@fmadio20n40v3-363:~$ brctl show man10
bridge name     bridge id               STP enabled     interfaces
man10           8000.18c04d177a4e       no              phy10

```

### Management MTU Setting

By default MTU size is set to 1500B for maximum compatibility. This can be configure for 9200 Jumbo frame support to maximize download throughput. This is done by setting 

```lua
["MTU"] = 9
```

For both man10 and phy10 network interfaces in the network configuration script below.

```text
/opt/fmadio/etc/network.lua
```

This  has to be set on both the man10 and phy10 \(optionally phy11 if used\) interfaces to be fully effective as per below example.

```lua
["man10"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.15.225",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "",
    ["DNS0"]    = "",
    ["DNS1"]    = "",
    ["Speed"]   = "10g",
    ["TSMode"]  = "nic",
    ["MTU"]     = 9200,
}
,
["phy10"] =
{
    ["MTU"]  = 9200,
}

```

