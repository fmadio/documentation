# JSON Field definition

By default pcap2json uses its builtin JSON field definition. This can however be overridden with a custom JSON format and file using 

```text
--flow-template '{ custom JSON format }'
```

option in  /opt/fmadio/etc/pcap2json.lua  Section "backend"

Default JSON format \(2021/7/24

```text
{\"timestamp\":@TIMESTAMP@,\"TS\":@TS@,\"device\":@DEVICE@,\"hashHalf\":@HASH_HALF@,\"hashFull\":@HASH_FULL@,\"flowCount\":@FLOWCNT@,\"macSrc\":@MACSRC@,\"macDst\":@MACDST@,\"macProto\":@MACPROTO@,\"vlan0\":@VLAN0@,\"mpls0TC\":@MPLS0TC@,\"ipv4Src\":@IPV4SRC@,\"hostSrc\":@SRCIP_HOSTNAME@,\"ipv4Dst\":@IPV4DST@,\"hostDst\":@DSTIP_HOSTNAME@,\"ipv4Proto\":@IPV4PROTO@,\"ipv4DSCP\":@IPV4DSCP@,\"ipv4Frag\":@IPV4FRAG@,\"portSrc\":@PORTSRC@,\"portDs
t\":@PORTDST@,\"application\":@APPLICATION@,\"tag0\":@TAG0@,\"tag1\":@TAG1@,\"tag2\":@TAG2@,\"tcpFin\":@TCPFIN@,\"tcpSyn\":@TCPSYN@,\"tcpSynAck\":@TCPSYNACK@,\"tcpSackPerm\":@TCPSYNSACK@,\"tcpRst\":@TCPRST@,\"tcpSack\":@TCPSACK@,\"tcpZeroWindow\":@TCPWINZERO@,\"totalPackets\":@TOTALPKT@,\"totalBytes\":@TOTALBYTE@,\"totalBits\":@TOTALBIT@,\"totalFCS\":@TOTALFCS@,\"geoipSrc\":{\"location\":@SRCIP_LOCATION@,\"country_name\":@SRCIP_COUNTRY@,\"country_iso_code\":@SRCIP_COUNTRY_CODE@,\"city_name\":@SRCIP_CITY@,\"asn\":@SRCIP_ASN@,\"org\":@SRCIP_ORG@,\"isp\":@SRCIP_ISP@},\"geoipDst\":{\"location\":@DSTIP_LOCATION@,\"country_name\":@DSTIP_COUNTRY@,\"country_iso_code\":@DSTIP_COUNTRY_CODE@,\"city_name\":@DSTIP_CITY@,\"asn\":@DSTIP_ASN@,\"org\":@DSTIP_ORG@,\"isp\":@DSTIP_ISP@},\"tcpRTTNet\":@TCP_RTT_NET@,\"tcpRTTApp\":@TCP_RTT_APP@,\"tcpWindowMin\":@TCP_WINDOW_MIN@,\"tcpWindowMax\":@TCP_WINDOW_MAX@,\"tcpWindowMean\":@TCP_WINDOW_MEAN@,\"decapType\":@DECAP_TYPE@,\"decapIPv4Src\":@DECAP_IPV4_SRC@,\"decapIPv4Dst\":@DECAP_IPV4_DST@,\"decapIpv4Proto\":@DECAP_IPV4_PROTO@,\"decapPortSrc\":@DECAP_PORT_SRC@,\"decapPortDst\":@DECAP_PORT_DST@}"
```

Below is a reference for all the fields

### @TIMESTAMP@

Outputs timestamp in MSec epoch time

```javascript
{
  .
  .
  "timestamp": 1497015593700,
  .
  .
}
```

### @TCP\_RTT\_NET@

Outputs the network TCP RTT in milliseconds

```javascript
{
  .
  .
  "tcpRTTNet": 51.583,
  "tcpRTTApp": 118.732,
  .
  .
}
```

### @TCP\_RTT\_APP@

Outputs the Application Request -&gt; Response network round trip in milliseconds

```javascript
{
  .
  .
  "tcpRTTNet": 51.583,
  "tcpRTTApp": 118.732,
  .
  .
}
```

### @TCP\_WINDOW\_MIN@

Outputs the minimum TCP window size in Bytes

NOTE: This does take into account full Window Scaling from the SYN/SYNACK connection. If no SYN/SYNCACK has been processed this filed outputs NULL{

```javascript
{
.
.
  "tcpWindowMin": 1,
  "tcpWindowMax": 4739,
  "tcpWindowMean": 89,
.
.
}
```

### @TCP\_WINDOW\_MAX@

Outputs the maximum TCP window size in Bytes

NOTE: This does take into account full Window Scaling from the SYN/SYNACK connection. If no SYN/SYNCACK has been processed this filed outputs NULL

```javascript
{
.
.
  "tcpWindowMin": 1,
  "tcpWindowMax": 4739,
  "tcpWindowMean": 89,
.
.
}
```

### @TCP\_WINDOW\_MEAN@

Outputs the arithmetic mean TCP window size in Bytes

NOTE: This does take into account full Window Scaling from the SYN/SYNACK connection. If no SYN/SYNCACK has been processed this filed outputs NULL

```javascript
{
.
.
  "tcpWindowMin": 1,
  "tcpWindowMax": 4739,
  "tcpWindowMean": 89,
.
.
}
```

### @DECAP\_TYPE@

If packet de-encapsulation was performance, indicates what kind of encapsulation

| Value | Description                                                                    |
| :--- | :--- |
| ICMP Destination Unreachable | ICMP Code 3 |
| ICMP Time Exceeded | ICMP Code 11 |
| GRE | GRE Tunnel |

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

### @DECAP\_IPV4\_SRC@

For de-encapsulated packets, the IPv4 Source address of the INNER packet

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

### @DECAP\_IPV4\_DST@

For de-encapsulated packets, the IPv4 Destination address of the INNER packer

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

### @DECAP\_IPV4\_PROTO@

For de-encapsulated packets the IPv4 Protocol of the INNER packet

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

### @DECAP\_PORT\_SRC@

For de-encapsulated packets the Source Port of the INNER TCP or UDP packet

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

### @DECAP\_PORT\_DST@

For de-encapsulated packets the Destination Port of the INNER TCP or UDP packet

```javascript
{
  .
  .
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632
  .
  .
}
```

