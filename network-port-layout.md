# Network Port Layout

Layout of the network ports is as follows. for FMADIO100G v2 1U Capture System \(10G SFP+ management\)

![](.gitbook/assets/image%20%2811%29.png)

Layout of the network ports for FMADIO100G v2 1U Analytics System \(40G QSFP+ Management\)

TBD

**1Gbe Management Ports**

Systems shipped Prior to April 2021, default 1G management port is "L2"

System shipped after April 2021 default 1G management port is "L1"

This was made as L1 port can bridge the IPMI/BMC port \(single RJ45 connection for both Server and BMC\), the L2 port can not bridged. Please contact support@fmad.io on how to swap.



**IPMI Port**

The IPMI Port is used for out of band communication with the system. It allows power on/off and KVM capabilities. Highly recommend connecting this. Default IP address is 192.168.0.93/24

**Management Port**

The Management ports can be 10G SFP+ or 40G QSFP+ depending on the system configuration. These can be run in standard, or link bonded / redundant setup.

FMADIO100Gv2 Capture systems, 

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 10G | SFP+  |
| 2 | 1G | SFP |

FMADIO100G Gen2 Analytics Systems

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 40Gbps | QSFP+ |
| 4 | 10Gbps | QSFP+ Breakout Cables |

**Capture Port**

Capture ports can be configured in the following way

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 100Gbps | QSFP28 \(FEC or no FEC\) |
| 2 | 40Gbps | QSFP+ |
| 8 | 25Gbps | QSFP28 \(Not released yet\) |
| 8 | 10Gbps | QSFP+ Breakout Cables |



