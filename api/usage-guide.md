---
description: >-
  A guide for developers to access a FMADIO using the web based API. Examples
  are provided for all endpoints.
---

# Usage Guide

## FMADIO API

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

**StreamName**, **Compression** and **FilterBPF**

```text
curl -u fmadio:100g "http://1.1.1.1/pcap/single?StreamName=stream_test_001&Compression=fast&" -G --data-urlencode "FilterBPF=tcp"
```

### SplitTime



## V1 API

### PCAP Download



