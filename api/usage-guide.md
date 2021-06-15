---
description: >-
  A guide for developers to access a FMADIO using the web based API. Examples
  are provided for all endpoints.
---

# Examples

## FMADIO API

The examples show how to use the different parameters for the uri endpoint. 

**Note**: Replace the IP 127.0.0.1 with the host IP of your FMADIO device.

### Status

```text
curl -u fmadio:100g "http://127.0.0.1/sysmaster/status"
```

### Device Status

```text
curl -u fmadio:100g "http://127.0.0.1/sysmaster/stats_summary"
```

### CaptureList

```text
curl -u fmadio:100g "http://127.0.0.1/stream/list"
```

### Capture Split By Filesize

```text
curl -u fmadio:100g "http://127.0.0.1/stream/ssize?StreamName=stream_test_001&StreamView=split_1GB&"
```

### Capture Split By Time

```text
curl -u fmadio:100g "http://127.0.0.1/stream/stime?StreamName=stream_test_001&StreamView=split_1sec&"
```

### Single

**StreamName** only:

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001"
```

**StreamName** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterBPF=tcp"
```

**StreamName** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&Compression=fast"
```

**StreamName** and **FilterRE**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterRE=/login/i" 
```

**StreamName**, **Compression** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/single?StreamName=stream_test_001&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

### SplitTime

**StreamName**, **Start** and **Stop**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&"
```

**StreamName**, **Start,** **Stop** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&" -G --data-urlencode "FilterBPF=tcp" 
```

**StreamName**, **Start**, **Stop** and **FilterPort**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&FilterPort=0"
```

### TimeRange

**TSBegin** and **TSEnd**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/timerange?TSBegin=1497329459948411420&TSEnd=1597329469948411420"
```

**TSBegin, TSEnd, TSMode** and **TSMax**

```text
curl -u fmadio:100g "http://127.0.0.1/pcap/timerange?TSBegin=1497329459948411420&TSEnd=1597329469948411420&TSMode=nanos&TSMax=1000000"
```

## V1 API

The examples show how to use the different parameters for the uri endpoint. 

**Note**: Replace the IP 127.0.0.1 with the host IP of your FMADIO device.

### Single

**StreamName** only.

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/single?StreamName=stream_test"
```

**StreamName** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterBPF=tcp"
```

**StreamName** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/single?StreamName=stream_test_001&Compression=fast"
```

**StreamName**, **Compression** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/single?StreamName=stream_test_001&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

### SplitTime

**StreamName**, **Start** and **Stop**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&"
```

**StreamName**, **Start,** **Stop** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&" -G --data-urlencode "FilterBPF=tcp" 
```

**StreamName**, **Start,** **Stop, FilterBPF** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&Compression=fast&" -G --data-urlencode "FilterBPF=tcp" 
```

**StreamName**, **Start,** **Stop** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&Compression=fast&" 
```

### TimeRange

**TSBegin** and **TSEnd**

```text
 curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000"
```

**TSBegin**, **TSEnd** and **TSMax**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000&TSMax=100000"
```

**TSBegin**, **TSEnd** and **FilterBPF**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000&" -G --data-urlencode "FilterBPF=tcp"  
```

**TSBegin**, **TSEnd** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000&Compression=fast"
```

**TSBegin**, **TSEnd**, **FilterBPF** and **Compression**

```text
curl -u fmadio:100g "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

