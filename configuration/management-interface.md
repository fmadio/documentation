# Management Interface

FMADIO Systems have multiple 1G, 10G and 40G management interfaces, depending on the ordered SKU.

Management interfaces are all bridged by default per the following block diagram

![FMADIO Management Interface Architecture](../.gitbook/assets/image%20%2872%29.png)

Using the above configuration allows

* LXC containers full pass-thru IP address \(no NAT\)
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
["MTU"] = 9200,
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

### LACP Link Bonding

**Requires FW:6508+**

LACP or Link Bonding is critical for fail over / redundancy planning. FMADIO Packet Capture devices run on Linux thus we support LCAP/Bonding on the management interfaces.

```lua
/opt/fmadio/etc/network.lua
```

Add a bonded interface "bond0" as follows

```lua
["bond0"] =
{
    ["Mode"]    = "bond",
    ["Address"] = "192.168.1.2",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.1.1",
    ["DNS0"]    = "",
    ["DNS1"]    = "",
    ["Speed"]   = "10g",
    ["TSMode"]  = "nic",
    ["Slave"]  = { "phy0", "phy1" }
},
```

In the above example the "Slave" field contains the list of physical interfaces the bonding runs on. This example is bonding the two 1G RJ45 interfaces on the system. To bond the 10G interfaces on a separate LCAP link \(bond1\), use the following:

```lua
["bond1"] =
{
    ["Mode"]    = "bond",
    ["Address"] = "192.168.1.2",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.1.1",
    ["DNS0"]    = "",
    ["DNS1"]    = "",
    ["Speed"]   = "10g",
    ["TSMode"]  = "nic",
    ["Slave"]  = { "phy10", "phy11" }
},
```

### LACP Bonding Mode

**Requires FW: 6633+**

 By default 802.3ad bonding mode is used, full list of Linux bonding modes can be seen on [kernel.org](https://www.kernel.org/doc/Documentation/networking/bonding.txt). Note "BondMode" specifies the Linux bonding mode to be used.

```lua
["bond1"] =
{
    ["Mode"]    = "bond",
    ["BondMode"] = "active-backup",
    ["Address"] = "192.168.1.2",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.1.1",
    ["DNS0"]    = "",
    ["DNS1"]    = "",
    ["Speed"]   = "10g",
    ["TSMode"]  = "nic",
    ["Slave"]  = { "phy10", "phy11" }
},
```

Line Bonding mode options \(details ripped from kernel.org\)

 **Round-robin** \(balance-rr\)  
  
Transmit network packets in sequential order from the first available network interface \(NIC\) slave through the last. This mode provides load balancing and fault tolerance.  
  
**Active-backup** \(active-backup\)  
  
Only one NIC slave in the bond is active. A different slave becomes active if, and only if, the active slave fails. The single logical bonded interface's MAC address is externally visible on only one NIC \(port\) to avoid distortion in the network switch. This mode provides fault tolerance.  
  
  
**XOR** \(balance-xor\)  
  
Transmit network packets based on a hash of the packet's source and destination. The default algorithm only considers MAC addresses \(layer2\). Newer versions allow selection of additional policies based on IP addresses \(layer2+3\) and TCP/UDP port numbers \(layer3+4\). This selects the same NIC slave for each destination MAC address, IP address, or IP address and port combination, respectively. This mode provides load balancing and fault tolerance.  
  
  
**Broadcast** \(broadcast\)  
  
Transmit network packets on all slave network interfaces. This mode provides fault tolerance.  
  
  
**Default mode**  
**IEEE 802.3ad Dynamic link aggregation** \(802.3ad, LACP\)  
  
Creates aggregation groups that share the same speed and duplex settings. Utilizes all slave network interfaces in the active aggregator group according to the 802.3ad specification. This mode is similar to the XOR mode above and supports the same balancing policies. The link is set up dynamically between two LACP-supporting peers.  
  
  
**Adaptive transmit load balancing** \(balance-tlb\)  
  
Linux bonding driver mode that does not require any special network-switch support. The outgoing network packet traffic is distributed according to the current load \(computed relative to the speed\) on each network interface slave. Incoming traffic is received by one currently designated slave network interface. If this receiving slave fails, another slave takes over the MAC address of the failed receiving slave.  
  
  
**Adaptive load balancing** \(balance-alb\)  
  
includes balance-tlb plus receive load balancing \(rlb\) for IPV4 traffic, and does not require any special network switch support. The receive load balancing is achieved by ARP negotiation. The bonding driver intercepts the ARP Replies sent by the local system on their way out and overwrites the source hardware address with the unique hardware address of one of the NIC slaves in the single logical bonded interface such that different network-peers use different MAC addresses for their network packet traffic.  
  
  
NOTE: PTPv2 and LCAP on the 10G Management interfaces are mutually exclusive.

