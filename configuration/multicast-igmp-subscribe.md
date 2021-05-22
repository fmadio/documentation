# Multicast IGMP Subscribe

#### FW Version: 7130+

There are many cases where the FMADIO Packet Capture system should subscribe to Multicast traffic via the capture Port.



![](../.gitbook/assets/image%20%2821%29.png)

In the above example each capture port is connected to 2 separate switches \(e.g. Market Data Feed A and B\) such that each capture port receives the a direct Market data feed, instead of relying on SPAN/Mirror sessions in a passive setup.

To configure start with enabling the capture ports IP/MAC address as per

{% page-ref page="capture-port-ip-mac.md" %}

Next is configuring the IGMP groups and port configuration. Example configuration for the OPRA Options market data feed is as follows. Edit or create the file

```text
/opt/fmadio/etc/igmp.lua
```

Example igmp.lua file is as follows.

```lua
local Port0 = {}
table.insert(Port0, { Group = "233.43.202.1", Port = 11101, VLAN = nil }) --  "OPRA_001",
table.insert(Port0, { Group = "233.43.202.2", Port = 11102, VLAN = nil }) --  "OPRA_002",
table.insert(Port0, { Group = "233.43.202.3", Port = 11103, VLAN = nil }) --  "OPRA_003",
table.insert(Port0, { Group = "233.43.202.4", Port = 11104, VLAN = nil }) --  "OPRA_004",
table.insert(Port0, { Group = "233.43.202.5", Port = 11105, VLAN = nil }) --  "OPRA_005",
table.insert(Port0, { Group = "233.43.202.6", Port = 11106, VLAN = nil }) --  "OPRA_006",
table.insert(Port0, { Group = "233.43.202.7", Port = 11107, VLAN = nil }) --  "OPRA_007",
table.insert(Port0, { Group = "233.43.202.8", Port = 11108, VLAN = nil }) --  "OPRA_008",
table.insert(Port0, { Group = "233.43.202.9", Port = 11109, VLAN = nil }) --  "OPRA_009",
table.insert(Port0, { Group = "233.43.202.10", Port = 11110, VLAN = nil }) --  "OPRA_010",
table.insert(Port0, { Group = "233.43.202.11", Port = 11111, VLAN = nil }) --  "OPRA_011",
table.insert(Port0, { Group = "233.43.202.12", Port = 11112, VLAN = nil }) --  "OPRA_012",
table.insert(Port0, { Group = "233.43.202.13", Port = 11113, VLAN = nil }) --  "OPRA_013",
table.insert(Port0, { Group = "233.43.202.14", Port = 11114, VLAN = nil }) --  "OPRA_014",
table.insert(Port0, { Group = "233.43.202.15", Port = 11115, VLAN = nil }) --  "OPRA_015",
table.insert(Port0, { Group = "233.43.202.16", Port = 11116, VLAN = nil }) --  "OPRA_016",
table.insert(Port0, { Group = "233.43.202.17", Port = 11117, VLAN = nil }) --  "OPRA_017",
table.insert(Port0, { Group = "233.43.202.18", Port = 11118, VLAN = nil }) --  "OPRA_018",
table.insert(Port0, { Group = "233.43.202.19", Port = 11119, VLAN = nil }) --  "OPRA_019",
table.insert(Port0, { Group = "233.43.202.20", Port = 11120, VLAN = nil }) --  "OPRA_020",
table.insert(Port0, { Group = "233.43.202.21", Port = 11121, VLAN = nil }) --  "OPRA_021",
table.insert(Port0, { Group = "233.43.202.22", Port = 11122, VLAN = nil }) --  "OPRA_022",
table.insert(Port0, { Group = "233.43.202.23", Port = 11123, VLAN = nil }) --  "OPRA_023",
table.insert(Port0, { Group = "233.43.202.24", Port = 11124, VLAN = nil }) --  "OPRA_024",
table.insert(Port0, { Group = "233.43.202.129", Port = 16101, VLAN = nil }) --  "OPRA_025",
table.insert(Port0, { Group = "233.43.202.130", Port = 16102, VLAN = nil }) --  "OPRA_026",
table.insert(Port0, { Group = "233.43.202.131", Port = 16103, VLAN = nil }) --  "OPRA_027",
table.insert(Port0, { Group = "233.43.202.132", Port = 16104, VLAN = nil }) --  "OPRA_028",
table.insert(Port0, { Group = "233.43.202.133", Port = 16105, VLAN = nil }) --  "OPRA_029",
table.insert(Port0, { Group = "233.43.202.134", Port = 16106, VLAN = nil }) --  "OPRA_030",
table.insert(Port0, { Group = "233.43.202.135", Port = 16107, VLAN = nil }) --  "OPRA_031",
table.insert(Port0, { Group = "233.43.202.136", Port = 16108, VLAN = nil }) --  "OPRA_032",
table.insert(Port0, { Group = "233.43.202.137", Port = 16109, VLAN = nil }) --  "OPRA_033",
table.insert(Port0, { Group = "233.43.202.138", Port = 16110, VLAN = nil }) --  "OPRA_034",
table.insert(Port0, { Group = "233.43.202.139", Port = 16111, VLAN = nil }) --  "OPRA_035",
table.insert(Port0, { Group = "233.43.202.140", Port = 16112, VLAN = nil }) --  "OPRA_036",
table.insert(Port0, { Group = "233.43.202.141", Port = 16113, VLAN = nil }) --  "OPRA_037",
table.insert(Port0, { Group = "233.43.202.142", Port = 16114, VLAN = nil }) --  "OPRA_038",
table.insert(Port0, { Group = "233.43.202.143", Port = 16115, VLAN = nil }) --  "OPRA_039",
table.insert(Port0, { Group = "233.43.202.144", Port = 16116, VLAN = nil }) --  "OPRA_040",
table.insert(Port0, { Group = "233.43.202.145", Port = 16117, VLAN = nil }) --  "OPRA_041",
table.insert(Port0, { Group = "233.43.202.146", Port = 16118, VLAN = nil }) --  "OPRA_042",
table.insert(Port0, { Group = "233.43.202.147", Port = 16119, VLAN = nil }) --  "OPRA_043",
table.insert(Port0, { Group = "233.43.202.148", Port = 16120, VLAN = nil }) --  "OPRA_044",
table.insert(Port0, { Group = "233.43.202.149", Port = 16121, VLAN = nil }) --  "OPRA_045",
table.insert(Port0, { Group = "233.43.202.150", Port = 16122, VLAN = nil }) --  "OPRA_046",
table.insert(Port0, { Group = "233.43.202.151", Port = 16123, VLAN = nil }) --  "OPRA_047",
table.insert(Port0, { Group = "233.43.202.152", Port = 16124, VLAN = nil }) --  "OPRA_048",

local Port1 = {}
table.insert(Port1, { Group = "233.43.202.33", Port = 12101, VLAN = nil }) --  "OPRA_001",
table.insert(Port1, { Group = "233.43.202.34", Port = 12102, VLAN = nil }) --  "OPRA_002",
table.insert(Port1, { Group = "233.43.202.35", Port = 12103, VLAN = nil }) --  "OPRA_003",
table.insert(Port1, { Group = "233.43.202.36", Port = 12104, VLAN = nil }) --  "OPRA_004",
table.insert(Port1, { Group = "233.43.202.37", Port = 12105, VLAN = nil }) --  "OPRA_005",
table.insert(Port1, { Group = "233.43.202.38", Port = 12106, VLAN = nil }) --  "OPRA_006",
table.insert(Port1, { Group = "233.43.202.39", Port = 12107, VLAN = nil }) --  "OPRA_007",
table.insert(Port1, { Group = "233.43.202.40", Port = 12108, VLAN = nil }) --  "OPRA_008",
table.insert(Port1, { Group = "233.43.202.41", Port = 12109, VLAN = nil }) --  "OPRA_009",
table.insert(Port1, { Group = "233.43.202.42", Port = 12110, VLAN = nil }) --  "OPRA_010",
table.insert(Port1, { Group = "233.43.202.43", Port = 12111, VLAN = nil }) --  "OPRA_011",
table.insert(Port1, { Group = "233.43.202.44", Port = 12112, VLAN = nil }) --  "OPRA_012",
table.insert(Port1, { Group = "233.43.202.45", Port = 12113, VLAN = nil }) --  "OPRA_013",
table.insert(Port1, { Group = "233.43.202.46", Port = 12114, VLAN = nil }) --  "OPRA_014",
table.insert(Port1, { Group = "233.43.202.47", Port = 12115, VLAN = nil }) --  "OPRA_015",
table.insert(Port1, { Group = "233.43.202.48", Port = 12116, VLAN = nil }) --  "OPRA_016",
table.insert(Port1, { Group = "233.43.202.49", Port = 12117, VLAN = nil }) --  "OPRA_017",
table.insert(Port1, { Group = "233.43.202.50", Port = 12118, VLAN = nil }) --  "OPRA_018",
table.insert(Port1, { Group = "233.43.202.51", Port = 12119, VLAN = nil }) --  "OPRA_019",
table.insert(Port1, { Group = "233.43.202.52", Port = 12120, VLAN = nil }) --  "OPRA_020",
table.insert(Port1, { Group = "233.43.202.53", Port = 12121, VLAN = nil }) --  "OPRA_021",
table.insert(Port1, { Group = "233.43.202.54", Port = 12122, VLAN = nil }) --  "OPRA_022",
table.insert(Port1, { Group = "233.43.202.55", Port = 12123, VLAN = nil }) --  "OPRA_023",
table.insert(Port1, { Group = "233.43.202.56", Port = 12124, VLAN = nil }) --  "OPRA_024",
table.insert(Port1, { Group = "233.43.202.161", Port = 17101, VLAN = nil }) --  "OPRA_025",
table.insert(Port1, { Group = "233.43.202.162", Port = 17102, VLAN = nil }) --  "OPRA_026",
table.insert(Port1, { Group = "233.43.202.163", Port = 17103, VLAN = nil }) --  "OPRA_027",
table.insert(Port1, { Group = "233.43.202.164", Port = 17104, VLAN = nil }) --  "OPRA_028",
table.insert(Port1, { Group = "233.43.202.165", Port = 17105, VLAN = nil }) --  "OPRA_029",
table.insert(Port1, { Group = "233.43.202.166", Port = 17106, VLAN = nil }) --  "OPRA_030",
table.insert(Port1, { Group = "233.43.202.167", Port = 17107, VLAN = nil }) --  "OPRA_031",
table.insert(Port1, { Group = "233.43.202.168", Port = 17108, VLAN = nil }) --  "OPRA_032",
table.insert(Port1, { Group = "233.43.202.169", Port = 17109, VLAN = nil }) --  "OPRA_033",
table.insert(Port1, { Group = "233.43.202.170", Port = 17110, VLAN = nil }) --  "OPRA_034",
table.insert(Port1, { Group = "233.43.202.171", Port = 17111, VLAN = nil }) --  "OPRA_035",
table.insert(Port1, { Group = "233.43.202.172", Port = 17112, VLAN = nil }) --  "OPRA_036",
table.insert(Port1, { Group = "233.43.202.173", Port = 17113, VLAN = nil }) --  "OPRA_037",
table.insert(Port1, { Group = "233.43.202.174", Port = 17114, VLAN = nil }) --  "OPRA_038",
table.insert(Port1, { Group = "233.43.202.175", Port = 17115, VLAN = nil }) --  "OPRA_039",
table.insert(Port1, { Group = "233.43.202.176", Port = 17116, VLAN = nil }) --  "OPRA_040",
table.insert(Port1, { Group = "233.43.202.177", Port = 17117, VLAN = nil }) --  "OPRA_041",
table.insert(Port1, { Group = "233.43.202.178", Port = 17118, VLAN = nil }) --  "OPRA_042",
table.insert(Port1, { Group = "233.43.202.179", Port = 17119, VLAN = nil }) --  "OPRA_043",
table.insert(Port1, { Group = "233.43.202.180", Port = 17120, VLAN = nil }) --  "OPRA_044",
table.insert(Port1, { Group = "233.43.202.181", Port = 17121, VLAN = nil }) --  "OPRA_045",
table.insert(Port1, { Group = "233.43.202.182", Port = 17122, VLAN = nil }) --  "OPRA_046",
table.insert(Port1, { Group = "233.43.202.183", Port = 17123, VLAN = nil }) --  "OPRA_047",
table.insert(Port1, { Group = "233.43.202.184", Port = 17124, VLAN = nil }) --  "OPRA_048",

return { Port0, Port1, Interval = 30e9, }

```

Note each Capture port \(Port0/Port1\) is subscribing to a different MC Group. Also the interval for broadcasting the joins by default is 60sec. In the above example this is reduced to 30sec \(Interval = 30e9  is in nanoseconds\)

After editing the file confirm the syntax is correct by running the command as follows

```bash
fmadio@fmadio100v2-228U:~$ fmadiolua /mnt/store0/etc/igmp.lua
fmad fmadlua May 22 2021
calibrating...
0 : 2095078768           2.0951 cycles/nsec offset:4.921 Mhz
Cycles/Sec 2095078768.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv fmadiolua
failed to open self? [fmadiolua]
loading filename [/mnt/store0/etc/igmp.lua]
done 0.000352Sec 0.000006Min
fmadio@fmadio100v2-228U:~$

```

There should not be any syntax errors or error statements. 

Changes to this file requires the capture to be restarted. Please stop the current capture, then restart it. After the restart the system will issue the IGMP joins based on the above.

Troubleshooting logfile can be seen in /mnt/store0/log/fnic\_ping.cur

