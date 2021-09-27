# Network Port Layout

### FMADIO40G Gen3 1U \(10G management\)

![FMADIO40G Gen3 1U Port Layout](.gitbook/assets/image%20%2852%29.png)

### FMADIO40G Gen3 2U \(10G management\)

![FMADIO40G Gen3 2U Port Layout](.gitbook/assets/image%20%2853%29.png)

### Port Mapping 

### **1Gbe Management Ports**

The default 1G management port is labeled "L1" and closets to the power supply.

This L1 port can bridge the IPMI/BMC port \(single RJ45 connection for both Server and BMC\)

The L2 port can not bridge the IPMI/BMC port

### **IPMI Port**

The IPMI Port is a dedicated segmented network port used for out of band communication with the system. It allows power on/off and KVM capabilities. Highly recommend connecting this. Default IP address is 192.168.0.93/24

### **Management Port**

The Management ports are 1G SFP/ 10G SFP+  \(Capture System\). Or 10G/40G QSFP+ \(Analytics systems\) These can be run in standard, or link bonded / redundant setup.

FMADIO40Gv3 Capture systems, 

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 10G | SFP+                                     |
| 2 | 1G | SFP |

FMADIO40Gv3 Gen3 Analytics Systems

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 40Gbps | QSFP+   |
| 4 | 10Gbps | QSFP+ Breakout Cables    |

### **Capture Port**

Capture ports can be configured in the following way.

| Port Count | Port Speed | Interface |
| :--- | :--- | :--- |
| 2 | 100Gbps | QSFP28 \(FEC or no FEC\) |
| 2 | 40Gbps | QSFP+ |
| 8 | 25Gbps | QSFP28 \(Not released yet\) |
| 8 | 10Gbps | QSFP+ Breakout Cables |

### **PPS Connector**

The PPS connector is a 1 Pulse Per Second time synchronization cable. It runs on a 3.3V trigger signal the interface is SMA Coaxial cable

## Linux Port mapping



