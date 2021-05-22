# Capture Port IP/MAC

#### FW Version: 7130+

By default FMADIO devices capture ports operate without any MAC or IP information. It receives and records any and all ethernet traffic on the wire. Its essentially a black hole high speed data recorder. 

However there are some situations where the Capture interfaces need an IP MAC address, this is for ERSPAN IP targets, and also having the capture ports directly join Mulitcast groups. The follow demonstrates how to setup IP MAC Address,

Using FMAIO DPI Engine we can filter out low bandwidth traffic such as ARP/ICMP requests without any effect on the 100Gbps / 149Mpps packet capture performance. As seen below using a few of the PreCapture filter rules and forwarding a few packets to our ARP/ICMP/IGMP software network stack running on the x86 Server. This allows full ARP and ICMP protocol support on the capture interfaces.

![FMADIO Packet Capture DPI ARP ICMP Network Architecture](../.gitbook/assets/image%20%2828%29.png)

NOTE: Enabling this feature reduces the total number of Pre Capture filter rules, It requires

| Rule Number | Description |
| :--- | :--- |
| 15 | MAC Broadcast ARP Requests  |
| 14 | Capture Port 0 MAC filter |
| 13 | Capture Port 1 MAC Filter |

Depending on the capture port port configuration \(2 ports or 8 ports\), a maximum of 9 pre-capture filter rules may be used.

#### CONFIG

To Configure IP/MAC information for the capture ports. edit the files below, by adding 2 new sections \["cap0"\] and \["cap1"\] i the configuration file

```bash
/opt/fmadio/etc/network.lua
```

Edit the file such that it look like the following, adding in your own MAC and IP addresses.  There is no Netmask or Gateway associated with the capture ports.

```lua
fmadio@fmadio20v2-149:~$ cat /opt/fmadio/etc/network.lua
-- auto generated on Wed Dec 26 20:13:20 2018
local Config =
{
.
.
.
.
.

["cap0"] =
{
      ["MAC"]     = "00:11:22:33:44:55",
      ["Address"] = "192.168.15.170",
},
["cap1"] =
{
      ["MAC"]     = "00:66:77:88:99:aa",
      ["Address"] = "192.168.15.171",
},

}
return Config
```

Save the file and reboot the system. 

The capture interfaces MAC/IP/IGMP is only enabled when there is an active capture. Please start a capture then try arping the interface first

```bash
fmadio@fmadio100v2-228U:~$ sudo arping -I man10 192.168.15.170
ARPING to 192.168.15.170 from 192.168.15.175 via man10
Unicast reply from 192.168.15.170 [00:11:22:33:44:55] 0.097ms
Unicast reply from 192.168.15.170 [00:11:22:33:44:55] 0.044ms
Unicast reply from 192.168.15.170 [00:11:22:33:44:55] 0.041ms
Unicast reply from 192.168.15.170 [00:11:22:33:44:55] 0.040ms
Unicast reply from 192.168.15.170 [00:11:22:33:44:55] 0.040ms
^CSent 5 probe(s) (1 broadcast(s))
Received 5 reply (0 request(s), 0 broadcast(s))
fmadio@fmadio100v2-228U:~$

```

If that is successful then check ICMP ping is functioning correctly also

```bash
fmadio@fmadio100v2-228U:~$ ping 192.168.15.170
PING 192.168.15.170 (192.168.15.170): 56 data bytes
64 bytes from 192.168.15.170: seq=1 ttl=64 time=40.726 ms
64 bytes from 192.168.15.170: seq=2 ttl=64 time=0.069 ms
64 bytes from 192.168.15.170: seq=3 ttl=64 time=0.075 ms
64 bytes from 192.168.15.170: seq=4 ttl=64 time=0.097 ms
64 bytes from 192.168.15.170: seq=5 ttl=64 time=0.089 ms
^C
--- 192.168.15.170 ping statistics ---
6 packets transmitted, 5 packets received, 16% packet loss
round-trip min/avg/max = 0.069/8.211/40.726 ms
fmadio@fmadio100v2-228U:~$

```

Any problems please check the log file 

```bash
/mnt/store0/log/fnic_ping.cur
```

It may provide more information on why ARP or ICMP is not responding correctly

