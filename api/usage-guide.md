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

### **TSBegin** and **TSEnd**

```
 curl -u fmadio:xxxx  "http://127.0.0.1/api/v1/pcap/timerange?
  TSBegin=1621772572136996000&
  TSEnd=1621774913584264000"
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
