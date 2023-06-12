---
description: >-
  A guide for developers to access a FMADIO using the web based API. Examples
  are provided for all endpoints.
---

# Examples

## FMADIO API

The examples show how to use the different parameters for the uri endpoint.&#x20;

**Note**: Replace the IP 127.0.0.1 with the host IP of your FMADIO device.

### Status

```
curl -u fmadio:100g "http://127.0.0.1/sysmaster/status"
```

### Device Status

```
curl -u fmadio:100g "http://127.0.0.1/sysmaster/stats_summary"
```

### CaptureList

```
curl -u fmadio:100g "http://127.0.0.1/stream/list"
```

### Capture Split By Filesize

```
curl -u fmadio:100g "http://127.0.0.1/stream/ssize?
    StreamName=stream_test_001&
    StreamView=split_1GB&"
```

### Capture Split By Time

```
curl -u fmadio:100g "http://127.0.0.1/stream/stime?
    StreamName=stream_test_001&
    StreamView=split_1sec&"
```

## Legacy Interface&#x20;

### Single

**StreamName** only:

```
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001"
```

**StreamName** and **FilterBPF**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterBPF=tcp"
```

**StreamName** and **Compression**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&Compression=fast"
```

**StreamName** and **FilterRE**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterRE=/login/i" 
```

**StreamName**, **Compression** and **FilterBPF**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

### SplitTime

**StreamName**, **Start** and **Stop**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&"
```

**StreamName**, **Start,** **Stop** and **FilterBPF**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&" -G --data-urlencode "FilterBPF=tcp" 
```

**StreamName**, **Start**, **Stop** and **FilterPort**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&FilterPort=0"
```

### TimeRange

**TSBegin** and **TSEnd**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/timerange?TSBegin=1497329459948411420&TSEnd=1597329469948411420"
```

**TSBegin, TSEnd, TSMode** and **TSMax**

```
curl -u fmadio:100g "http://127.0.0.1/pcap/timerange?TSBegin=1497329459948411420&TSEnd=1597329469948411420&TSMode=nanos&TSMax=1000000"
```

## V1 API

The examples show how to use the different parameters for the uri endpoint.&#x20;

**Note**: Replace the IP 127.0.0.1 with the host IP of your FMADIO device.

## API v1 - Single

### **StreamName** only.

```
curl -u fmadio:xxxxx "http://127.0.0.1/api/v1/pcap/single?
    StreamName=stream_test"
```

### **StreamName** and **FilterBPF**

```
curl -u fmadio:xxxxx "http://127.0.0.1/api/v1/pcap/single?
    StreamName=stream_test_001" 
    -G --data-urlencode "FilterBPF=tcp"
```

### **StreamName** and **Compression**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/single?
    StreamName=stream_test_001&
    Compression=fast"
```

### **StreamName**, **Compression** and **FilterBPF**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/single?
    StreamName=stream_test_001&
    Compression=fast" 
    -G --data-urlencode "FilterBPF=tcp"
```

## API v1 - SplitTime

### **StreamName**, **Start** and **Stop**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/splittime?
    StreamName=stream_test_001&
    Start=1530498788000000000&
    Stop=1530498789000000000"
```

### **StreamName**, **Start,** **Stop** and **FilterBPF**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/splittime?
    StreamName=stream_test_001&
    Start=1530498788000000000&
    Stop=1530498789000000000" 
    -G --data-urlencode "FilterBPF=tcp" 
```

### **StreamName**, **Start,** **Stop, FilterBPF** and **Compression**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/splittime?
    StreamName=stream_test_001&
    Start=1530498788000000000&
    Stop=1530498789000000000&
    Compression=fast" 
    -G --data-urlencode "FilterBPF=tcp" 
```

### **StreamName**, **Start,** **Stop** and **Compression**

```
curl -u fmadio:xxxx"http://127.0.0.1/api/v1/pcap/splittime?
    StreamName=stream_test_001&
    Start=1530498788000000000&
    Stop=1530498789000000000&
    Compression=fast" 
```

## API v1 - TimeRange

The Time Range function is very useful as the FMADIO system will work out which (or multiple) captures to check based on the Epoch Time stamp value.

### **TSBegin** and **TSEnd**&#x20;

#### **Nano second Epoch selection**

```
 curl -u fmadio:xxxx  "http://127.0.0.1/api/v1/pcap/timerange?
  TSBegin=1621772572136996000&
  TSEnd=1621774913584264000"
```

#### Second Epoch time Selection

```
curl -u fmadio:xxxx  "http://127.0.0.1/api/v1/pcap/timerange?
  TSMode=sec&
  TSBegin=1621772572&
  TSEnd=1621774913"
```

### **TSBegin**, **TSEnd** and **TSMax**

```
curl -u fmadio:xxxx  "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1621772572136996000&
    TSEnd=1621774913584264000&
    TSMax=100000"
```

### **TSBegin**, **TSEnd** and **FilterBPF**

```
curl -u fmadio:xxxx  "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1621772572136996000&
    TSEnd=1621774913584264000" 
    -G --data-urlencode "FilterBPF=tcp"  
```

### **TSBegin**, **TSEnd** and **Compression**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1621772572136996000&
    TSEnd=1621774913584264000&
    Compression=fast"
```

### **TSBegin**, **TSEnd**, **FilterBPF** and **Compression**

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1621772572136996000&
    TSEnd=1621774913584264000&
    Compression=fast" 
    -G --data-urlencode "FilterBPF=tcp"
```

### **TSBegin**, **TSEnd**, **FilterBPF** and FilterFrame

#### Frame Filters based on FMADIO Capture system

Filter based on FMADIO Capture port number

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=capture.port==0" 
     | tcpdump -r - -nn 
     | head
```

Filter based on multiple FMADIO Capture port numbers

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=capture.port==0,1,2,3" 
     | tcpdump -r - -nn 
     | head
```

Filter based on exclude FMADIO Capture port numbers

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=capture.port!=0" 
     | tcpdump -r - -nn 
     | head
```

#### Frame filters specific to 7130 (Metamako) hardware footer

Filter for a specific 7130 Device 54932 (any port)

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=a7130.srcdevice==54932" 
     | tcpdump -r - -nn 
     | head
```

Filter for everything except a specific 7130 Device (not device id 54932)

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=a7130.srcdevice!=54932" 
     | tcpdump -r - -nn 
     | head
```

Filter for a specific 7130 Port number 1

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=a7130.srcport==1" 
     | tcpdump -r - -nn 
     | head

```

Filter for multiple 7130 Port numbers 1, 2, 3, 5, 10

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=a7130.srcport==1,2,3,5,10" 
     | tcpdump -r - -nn 
     | head
```

Filter for everything except 7130 Port number 10

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&"
     -G --data-urlencode "FilterFrame=a7130.srcport!=10" 
     | tcpdump -r - -nn 
     | head
```

Filter on  a specific 7130 Port number and use the 7130 Footer Timestamp as the PCAP timestamp. Overriding the current TimeStamp setting

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&TSMode=arista7130"
     -G --data-urlencode "FilterFrame=a7130.srcport!=10" 
     | tcpdump -r - -nn 
     | head
```

#### Frame filters specific to Cisco 3550 (Exablaze) hardware footer

Filter on a specific ingress port of the Cisco 3550, and use the Footer timestamp as the PCAP timestamp.

```
curl "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1658744408270221800&
    TSEnd=1658744501189259300&TSMode=cisco3550"
     -G --data-urlencode "FilterFrame=c3550.srcport==48" 
     | tcpdump -r - -nn 
     | head
```

## Miscellaneous Examples

### &#x20; Encapsulation Debug

Many times the exact packet encapsulation is unclear, the following uses a wireshark filter expression to extract and show the full encapsulation format of the packet. From this a high speed BPF filter can be used to process the data.

In the below example we are using the Wireshark filter "ip.addr == 192.168.1.1" on a historical capture.

```
curl -u fmadio:xxxx "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1666706401000000000&
    TSEnd=1666706401010000000" 
    |  tshark -r - -T fields  -e frame.protocols -e ip.src -e ip.dst 
    -Y "ip.addr == 192.168.1.1"

```

Alternatively running on the currently running capture via SSH on the fmadio box looks like the following. This example filters on any UDP traffic.

```
sudo stream_cat 
    | tshark -r - -T fields  -e frame.protocols -e ip.src -e ip.dst -Y "udp" 
    | head
```

The output looks like the following

```
eth:ethertype:vlan:ethertype:ip:udp:ntp 106.10.186.200  192.168.133.10
eth:ethertype:vlan:ethertype:ip:udp:ntp 106.10.186.201  192.168.133.10
eth:ethertype:vlan:ethertype:ip:udp:ntp 167.172.70.21   192.168.133.10
eth:ethertype:vlan:ethertype:ip:udp:ntp 106.10.186.200  192.168.133.10
eth:ethertype:vlan:ethertype:ip:udp:ntp 106.10.186.201  192.168.133.10
eth:ethertype:vlan:ethertype:ip:udp:ntp 167.172.70.21   192.168.133.10

```

The above output shows there is a single VLAN tag in the packet. Making the equivalent BPF filter

```
vlan and udp
```

With the final BPF filter using a CURL request

```
curl -u fmadio:xxxxx "http://127.0.0.1/api/v1/pcap/timerange?
    TSBegin=1671407102&
    TSEnd=1671407752&
    TSMode=sec&" 
    -G --data-urlencode "FilterBPF=vlan and udp" 
    | tcpdump -r - -n 
    | head
```

Output per below

```
23:47:45.409489 IP 106.10.186.201.123 > 192.168.133.10.123: NTPv4, Server, length 48
23:52:14.407364 IP 167.172.70.21.123 > 192.168.133.10.123: NTPv4, Server, length 48
23:55:42.405072 IP 106.10.186.200.123 > 192.168.133.10.123: NTPv4, Server, length 48

```
