---
description: >-
  A guide for developers to access a FMADIO using the web based API. Examples
  are provided for all endpoints.
---

# Usage Guide

## FMADIO API

**Note**: Replace the IP 1.1.1.1 with the host IP of your FMADIO device.

### PCAP Download

The examples show how to use the different parameters for the single endpoint. 

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

## V1 API

### PCAP Download



