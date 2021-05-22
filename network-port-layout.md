# Network Port Layout

Layout of the network ports is as follows. for FMADIO100G v2 1U Capture System \(10G SFP+ management\)

![](.gitbook/assets/image%20%2819%29.png)



**1Gbe Management Ports**

The Lower RJ45 is the default management port

**IPMI Port**

The IPMI Port is used for out of band communication with the system. It allows power on/off and KVM capabilities. Highly recommend connecting this. Default IP address is 192.168.0.93/24

**Management Port**

10G Management Ports use SFP+ Interfaces

**Capture Port**

Capture ports can be configured in the following way

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 10Gbps | SFP+ Transceiver |
| 2 | 1G | SFP Transceiver |

**PPS Connector**

The PPS connector is a 1 Pulse Per Second time synchronization cable. It runs on a 3.3V trigger signal the interface is SMA Coaxial cable

