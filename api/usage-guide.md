---
description: >-
  A guide for developers to access a FMADIO using the web based API. Examples
  are provided for all endpoints.
---

# Examples

## FMADIO API

The examples show how to use the different parameters for the single endpoint. 

**Note**: Replace the IP 1.1.1.1 with the host IP of your FMADIO device.

### Status

```text
curl -u fmadio:100g http://1.1.1.1/sysmaster/status
```

### CaptureList

```text
curl -u fmadio:100g http://1.1.1.1/stream/list
```

### Capture Split By Filesize

```text
curl -u fmadio:100g "http://1.1.1.1/stream/ssize?StreamName=stream_test_001&StreamView=split_1GB&"
```

### Capture Split By Time

```text
curl -u fmadio:100g "http://1.1.1.1/stream/stime?StreamName=stream_test_001&StreamView=split_1sec&"
```

### Single

**StreamName** only:

```text
curl -u fmadio:100g http://1.1.1.1/pcap/single?StreamName=stream_test_001
```

**StreamName** and **FilterBPF**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterBPF=tcp"
```

**StreamName** and **Compression**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/single?StreamName=stream_test_001&Compression=fast"
```

**StreamName**, **Start,** **Stop** and **FilterRE**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/single?StreamName=stream_test_001&" -G --data-urlencode "FilterRE=/login/i" 
```

**StreamName**, **Compression** and **FilterBPF**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/single?StreamName=stream_test_001&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

### SplitTime

**StreamName**, **Start** and **Stop**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&"
```

**StreamName**, **Start,** **Stop** and **FilterBPF**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&" -G --data-urlencode "FilterBPF=tcp" 
```

**StreamName**, **Start**, **Stop** and **FilterPort**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/splittime?StreamName=stream_test_001&Start=1530498788000000000&Stop=1530498789000000000&FilterPort=0"
```

## V1 API

### 



