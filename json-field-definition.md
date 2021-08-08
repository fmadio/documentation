# JSON Fields

By default pcap2json uses its builtin JSON field definition. This can however be overridden with a custom JSON format and file using 

```text
--flow-template '{ custom JSON format }'
```

option in  /opt/fmadio/etc/pcap2json.lua  Section "backend"

Default JSON format \(2021/7/24\)

```javascript
{
\"timestamp\":@TIMESTAMP@,
\"TS\":@TS@,
\"device\":@DEVICE@,
\"hashHalf\":@HASH_HALF@,
\"hashFull\":@HASH_FULL@,
\"flowCount\":@FLOWCNT@,
\"macSrc\":@MACSRC@,
\"macDst\":@MACDST@,
\"macProto\":@MACPROTO@,
\"vlan0\":@VLAN0@,
\"mpls0TC\":@MPLS0TC@,
\"ipv4Src\":@IPV4SRC@,
\"hostSrc\":@SRCIP_HOSTNAME@,
\"ipv4Dst\":@IPV4DST@,
\"hostDst\":@DSTIP_HOSTNAME@,
\"ipv4Proto\":@IPV4PROTO@,
\"ipv4DSCP\":@IPV4DSCP@,
\"ipv4Frag\":@IPV4FRAG@,
\"portSrc\":@PORTSRC@,
\"portDst\":@PORTDST@,
\"application\":@APPLICATION@,
\"tag0\":@TAG0@,
\"tag1\":@TAG1@,
\"tag2\":@TAG2@,
\"tcpFin\":@TCPFIN@,
\"tcpSyn\":@TCPSYN@,
\"tcpSynAck\":@TCPSYNACK@,
\"tcpSackPerm\":@TCPSYNSACK@,
\"tcpRst\":@TCPRST@,
\"tcpSack\":@TCPSACK@,
\"tcpZeroWindow\":@TCPWINZERO@,
\"totalPackets\":@TOTALPKT@,
\"totalBytes\":@TOTALBYTE@,
\"totalBits\":@TOTALBIT@,
\"totalFCS\":@TOTALFCS@,
\"geoipSrc\":
{
    \"location\":@SRCIP_LOCATION@,
    \"country_name\":@SRCIP_COUNTRY@,
    \"country_iso_code\":@SRCIP_COUNTRY_CODE@,
    \"city_name\":@SRCIP_CITY@,
    \"asn\":@SRCIP_ASN@,
    \"org\":@SRCIP_ORG@,
    \"isp\":@SRCIP_ISP@
},
\"geoipDst\":
{
    \"location\":@DSTIP_LOCATION@,
    \"country_name\":@DSTIP_COUNTRY@,
    \"country_iso_code\":@DSTIP_COUNTRY_CODE@,
    \"city_name\":@DSTIP_CITY@,
    \"asn\":@DSTIP_ASN@,
    \"org\":@DSTIP_ORG@,
    \"isp\":@DSTIP_ISP@
},
\"tcpRTTNet\":@TCP_RTT_NET@,
\"tcpRTTApp\":@TCP_RTT_APP@,
\"tcpWindowMin\":@TCP_WINDOW_MIN@,
\"tcpWindowMax\":@TCP_WINDOW_MAX@,
\"tcpWindowMean\":@TCP_WINDOW_MEAN@,
\"decapType\":@DECAP_TYPE@,
\"decapIPv4Src\":@DECAP_IPV4_SRC@,
\"decapIPv4Dst\":@DECAP_IPV4_DST@,
\"decapIpv4Proto\":@DECAP_IPV4_PROTO@,
\"decapPortSrc\":@DECAP_PORT_SRC@,
\"decapPortDst\":@DECAP_PORT_DST@
}"
```

### Example JSON Flow

```javascript
{
  "timestamp": 1497015593700,
  "TS": "13:39:53.700.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "810a2feb5dcdb1f602f913c7de2e90b789e635e9",
  "hashFull": "a7cbb9296d6c642b34ef295a902a9fda8484a178",
  "flowCount": 36365,
  "macSrc": "7c:e2:ca:bd:97:d9",
  "macDst": "00:0e:52:80:00:16",
  "macProto": "IPv4",
  "vlan0": null,
  "mpls0TC": null,
  "ipv4Src": "202.17.220.140",
  "hostSrc": "202.17.220.140",
  "ipv4Dst": "208.67.222.222",
  "hostDst": "208.67.222.222",
  "ipv4Proto": "ICMP",
  "ipv4DSCP": null,
  "ipv4Frag": null,
  "portSrc": null,
  "portDst": null,
  "application": "(ICMP     0)",
  "tag0": null,
  "tag1": null,
  "tag2": null,
  "tcpFin": null,
  "tcpSyn": null,
  "tcpSynAck": null,
  "tcpSackPerm": null,
  "tcpRst": null,
  "tcpSack": null,
  "tcpZeroWindow": null,
  "totalPackets": 1,
  "totalBytes": 74,
  "totalBits": 592,
  "totalFCS": 0,
  "geoipSrc": {
    "location": [
      139.69,
      35.68999
    ],
    "country_name": "Japan",
    "country_iso_code": "JP",
    "city_name": null,
    "asn": null,
    "org": null,
    "isp": "Research Organization of Information and Systems"
  },
  "geoipDst": {
    "location": [
      -74.4774,
      41.45349
    ],
    "country_name": "United States",
    "country_iso_code": "US",
    "city_name": "Middletown",
    "asn": 36692,
    "org": "OPENDNS",
    "isp": "Cisco OpenDNS"
  },
  "tcpRTTNet": null,
  "tcpRTTApp": null,
  "tcpWindowMin": null,
  "tcpWindowMax": null,
  "tcpWindowMean": null,
  "decapType": "ICMP Destination Unreachable",
  "decapIPv4Src": "208.67.222.222",
  "decapIPv4Dst": "202.17.220.140",
  "decapIpv4Proto": "UDP",
  "decapPortSrc": 53,
  "decapPortDst": 64632

```

## Reference

The following is a reference for all the fields

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

Outputs the network TCP RTT in milliseconds.

Calculation is Half Duplex

Unit is Milliseconds

| Use Case | Calculation | Description |
| :--- | :--- | :--- |
| SYN/SYN-ACK | SYN-ACK.TS - SYN.TS | Initial Handshake RTT calculation, when Flow direction is connect\(\) |
| SYN-ACK/ACK | ACK.TS - SYN-ACK.TS | Initial TCP Handshake RTT when the Flow direction is accept\(\). \(not released yet\) |
| TCP Segment | ACK.TS - Push.TS | When the expected Ack Sequence number matches, the corresponding TCP Segment Push. Indicates TCP Peer has acknowledge receiving of the payload. This requires TCP ACK packet to be a 0 byte acknowledge only. Please note DACK  and related TCP options may skew this result.  [https://en.wikipedia.org/wiki/TCP\_delayed\_acknowledgment](https://en.wikipedia.org/wiki/TCP_delayed_acknowledgment) |

**Case SYN/SYN-ACK**, tcpRTTNet = TS1 - TS0 with respect to the Half Duplex Flow

![TCP RTT SYN/SYN-ACK](.gitbook/assets/image%20%28103%29.png)

**Case SYN-ACK/ACK**, tcpRTTNet = TS2 - T1 with respect to the Half Duplex Flow

![TCP RTT SYN-ACK/ACK](.gitbook/assets/image%20%28102%29.png)

**Cast TCP Segment Push/ACK**   

tcpRTTNet = TS101 - TS100

tcpRTTNet = TS201 - TS200

![TCP Push RTT Half Duplex](.gitbook/assets/image%20%28100%29.png)

![TCP Push RTT Half Duplex](.gitbook/assets/image%20%28101%29.png)

Example JSON output

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

Calculation is Half Duplex

Unit is Milliseconds

|  |
| :--- |


![](.gitbook/assets/image%20%2897%29.png)

Time different between between application Push to Push. e.g. HTTP Get -&gt; 200 OK Response

tcpRTTApp = TS1 - TS0

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

Calculation is Half Duplex

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

### @TCP\_WINDOW\_MAX@

Outputs the maximum TCP window size in Bytes

Calculation is Half Duplex

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

Calculation is Half Duplex

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

### @DECAP\_IPV4\_DSCP@

Version:608+

| This is the INNER IPv4 DSCP field following table shows the current enumeration. If an unknown value is found the text output is a hexadecimal string literal. |
| :--- |


Value \(In Hex\)

| JSON String |  |
| :--- | :--- |
| 0x2e | EF |
| 0x0a | AF11 |
| 0x0c | AF12 |
| 0x0e | AF13 |
| 0x12 | AF21 |
| 0x14 | AF22 |
| 0x16 | AF23 |
| 0x1a | AF31 |
| 0x1c | AF32 |
| 0x1e | AF33 |
| 0x22 | AF41 |
| 0x24 | AF42 |
| 0x26 | AF43 |
| 0x08 | CS1 |
| 0x10 | CS2 |
| 0x18 | CS3 |
| 0x20 | CS4 |
| 0x28 | CS5 |
| 0x30 | CS6 |
| 0x38 | CS7 |
| default | 0xXX  \(hexadecimal print\) |

Example JSON

```javascript
{
    .
    .
    .
     "decapType": "ICMP Time Exceeded",
     "decapIPv4Src": "130.128.197.9",
     "decapIPv4Dst": "111.87.220.242",
     "decapIpv4Proto": "ICMP",
     "decapIpv4DSCP": "AF13",
     "decapPortSrc": 0,
     "decapPortDst": 0
     .
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

### @DECAP\_SRCIP\_HOSTNAME@

Uses the GEO IP Mapping to search a a textual description of the De-Encapsulated source IPv4 address. If a mapping is not found, the standard IPV4 numeric address is used.

```javascript
{
  .
  .
  "decapIPv4Src": "45.0.192.14",
  "decapIPv4Dst": "10.1.119.196",
  "decapHostSrc": "45.0.192.14",
  "decapHostDst": "Reserved Private",
  .
  .
}
```

### @DECAP\_DSTIP\_HOSTNAME@

See @DECAP\_SRCIP\_HOSTNAME@  for description

### @DECAP\_SRCIP\_LOCATION@

De-encapsulated Source IP GeoIP Location. This includes the Longitude / Latitude value

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_COUNTRY@

De-Encapsulated Source IP GeoIP Country

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_COUNTRY\_CODE@

De-Encapsulated Source IP GeoIP 2 letter country code

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_CITY@

De-Encapsulated Source IP GeoIP City

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_ASN@

De-Encapsulated Source IP ASN

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_ORG@

De-Encapsulated Source IP Organization

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_SRCIP\_ISP@

De-Encapsulated Source IP ISP Name

```javascript
"decapGeoipSrc": {
    "location": [
      -51.6571,
      -27.4297
    ],
    "country_name": "Brazil",
    "country_iso_code": "BR",
    "city_name": "Capinzal",
    "asn": 53062,
    "org": "GGNET TELECOMUNICACOES LTDA",
    "isp": "Ggnet Telecomunicacoes Ltda"
  },

```

### @DECAP\_DSTIP\_HOSTNAME@



