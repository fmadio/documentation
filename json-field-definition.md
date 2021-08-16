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

## TCP Connection 

PCAP2JSON can calculate RTT and Byte accurate TCP Window information following is a description of each field. TCP Window sizes are real sizes in bytes that are scaled correctly using the SYN/SYNACK negotiated window scaling factor

### @TCP\_MODE@

This field indicates which side of the tcp connection the Half Duplex flow is on. Its particularly useful when used in combination with tcpRTTNetHalf to monitor the response time of the server or the WAN latency of a client.

| Value                   | Description |
| :--- | :--- |
| server | The server side of a half duplex flow. Calculated by the half duplex flow that receives the TCP SYN |
| client | TheClient side of a half duplex flow. Calculated by the flow which receives the TCP SYNACK |

#### tcpMode = server

Server highlighted in Aqua

![tcpMode = server](.gitbook/assets/image%20%28114%29.png)

#### tcpMode = client

Client highlighted in Aqua

![tcpMode = client](.gitbook/assets/image%20%28110%29.png)

### @TCP\_RTT\_NET\_FULL@

Outputs the full TCP round trip time in milliseconds. . This is a full duplex value, both sides of the TCP connection have the same value. This value approximates an ICMP ping between the two hosts.

Calculation is Full Duplex

Unit is Milliseconds

Show below, the aqua colored paths are used in the calculation. Total caluclation is

tcpNetRTTFull = P2 - P0

![tcpRTTNetFull latency calculation](.gitbook/assets/image%20%28113%29.png)

Example JSON output, note field  tcpRTTNetFull, in this example the Full RTT round trip is 856msec.

Both half duplex hash have the exact same tcpRTTNetFull value.

```javascript
{
  "timestamp": 1497015595700,
  "TS": "13:39:55.700.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "4811607328d46ec497c1add34da011649a73b611",
  "hashFull": "d9cd496276ea9c4a4f2f072e4201560239023bd9",
  "flowCount": 44437,
  "macSrc": "84:78:ac:3b:53:a5",
  "macDst": "68:05:ca:0a:b9:15",
  "macProto": "IPv4",
  "vlan0": 2048,
  "ipv4Src": "212.92.115.17",
  "hostSrc": "212.92.115.17",
  "ipv4Dst": "130.128.48.12",
  "hostDst": "130.128.48.12",
  "ipv4Proto": "TCP",
  "portSrc": 61025,
  "portDst": 3389,
  "application": "(TCP  3389)",
  "tcpRst": 1,
  "tcpMode": "client",
  "tcpRTTNetFull": 856.3,
  "tcpRTTNetHalf": 000.2,
  "totalPackets": 5,
  "totalBytes": 1365,
  "totalBits": 10920,
  "totalFCS": 0,
  "geoipSrcLocation": [
    4.89949,
    52.3824
  ],
  "geoipSrcCountry_name": "Netherlands",
  "geoipSrcCountry_iso_code": "NL",
  "geoipSrcAsn": 43350,
  "geoipSrcOrg": "NForce Entertainment B.V.",
  "geoipSrcIsp": "NFOrce Entertainment B.V.",
  "geoipDstLocation": [
    -115.14099,
    36.12139
  ],
  "geoipDstCountry_name": "United States",
  "geoipDstCountry_iso_code": "US",
  "geoipDstCity_name": "Las Vegas"
}

{
  "timestamp": 1497015595700,
  "TS": "13:39:55.700.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "adf86ea334da1be65391e97c4a635c2c50bfcc01",
  "hashFull": "d9cd496276ea9c4a4f2f072e4201560239023bd9",
  "flowCount": 44437,
  "macSrc": "68:05:ca:0a:b9:15",
  "macDst": "00:00:5e:00:01:31",
  "macProto": "IPv4",
  "vlan0": 2048,
  "ipv4Src": "130.128.48.12",
  "hostSrc": "130.128.48.12",
  "ipv4Dst": "212.92.115.17",
  "hostDst": "212.92.115.17",
  "ipv4Proto": "TCP",
  "portSrc": 3389,
  "portDst": 61025,
  "application": "(TCP  3389)",
  "tcpMode": "server",
  "tcpRTTNetFull": 856.3,
  "tcpRTTNetPartial": 856.1,
  "totalPackets": 5,
  "totalBytes": 1838,
  "totalBits": 14704,
  "totalFCS": 0,
  "geoipSrcLocation": [
    -115.14099,
    36.12139
  ],
  "geoipSrcCountry_name": "United States",
  "geoipSrcCountry_iso_code": "US",
  "geoipSrcCity_name": "Las Vegas",
  "geoipDstLocation": [
    4.89949,
    52.3824
  ],
  "geoipDstCountry_name": "Netherlands",
  "geoipDstCountry_iso_code": "NL",
  "geoipDstAsn": 43350,
  "geoipDstOrg": "NForce Entertainment B.V.",
  "geoipDstIsp": "NFOrce Entertainment B.V."
}

```

### @TCP\_RTT\_NET\_PARTIAL@

This calculates the partial RTT value. Note depending on where the capture device is located indicates which side of the RTT it shows. This value is used in conjunction with @TCP\_MODE@ to monitor Server response time or Client speed of light latency.

#### tcpMode = server

In this mode tcpRTTNetPartial is the round trip between request to connect to the server, and the services accept\(\) of the connection. Shown below in aqua

tcpRTTNetPartial = P1 - P0

![tcpRTTNetPartial \(server mode\)](.gitbook/assets/image%20%28112%29.png)

### tcpMode = client

In the  client mode, tcpRTTNetPartial is  \(usually\) the speed of light network latency between the Server \(e.g. HTTP web server\) and the Client a desktop / mobile  end user. Its not strictly the case, this is the typical use case as FMADIO Capture device is usually co-located with the Web Server.

tcpRTTNetPartial = P2 - P1



![tcpRTTNetPartial \(client mode\)](.gitbook/assets/image%20%28111%29.png)

Example JSON below. tcpRTTPartial is different for each half duplex flow.

```javascript
{
  "timestamp": 1497015595700,
  "TS": "13:39:55.700.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "4811607328d46ec497c1add34da011649a73b611",
  "hashFull": "d9cd496276ea9c4a4f2f072e4201560239023bd9",
  "flowCount": 44437,
  "macSrc": "84:78:ac:3b:53:a5",
  "macDst": "68:05:ca:0a:b9:15",
  "macProto": "IPv4",
  "vlan0": 2048,
  "ipv4Src": "212.92.115.17",
  "hostSrc": "212.92.115.17",
  "ipv4Dst": "130.128.48.12",
  "hostDst": "130.128.48.12",
  "ipv4Proto": "TCP",
  "portSrc": 61025,
  "portDst": 3389,
  "application": "(TCP  3389)",
  "tcpRst": 1,
  "tcpMode": "client",
  "tcpRTTNetFull": 856.3,
  "tcpRTTNetPartial": 000.2,
  "totalPackets": 5,
  "totalBytes": 1365,
  "totalBits": 10920,
  "totalFCS": 0,
  "geoipSrcLocation": [
    4.89949,
    52.3824
  ],
  "geoipSrcCountry_name": "Netherlands",
  "geoipSrcCountry_iso_code": "NL",
  "geoipSrcAsn": 43350,
  "geoipSrcOrg": "NForce Entertainment B.V.",
  "geoipSrcIsp": "NFOrce Entertainment B.V.",
  "geoipDstLocation": [
    -115.14099,
    36.12139
  ],
  "geoipDstCountry_name": "United States",
  "geoipDstCountry_iso_code": "US",
  "geoipDstCity_name": "Las Vegas"
}

{
  "timestamp": 1497015595700,
  "TS": "13:39:55.700.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "adf86ea334da1be65391e97c4a635c2c50bfcc01",
  "hashFull": "d9cd496276ea9c4a4f2f072e4201560239023bd9",
  "flowCount": 44437,
  "macSrc": "68:05:ca:0a:b9:15",
  "macDst": "00:00:5e:00:01:31",
  "macProto": "IPv4",
  "vlan0": 2048,
  "ipv4Src": "130.128.48.12",
  "hostSrc": "130.128.48.12",
  "ipv4Dst": "212.92.115.17",
  "hostDst": "212.92.115.17",
  "ipv4Proto": "TCP",
  "portSrc": 3389,
  "portDst": 61025,
  "application": "(TCP  3389)",
  "tcpMode": "server",
  "tcpRTTNetFull": 856.3,
  "tcpRTTNetPartial": 856.1,
  "totalPackets": 5,
  "totalBytes": 1838,
  "totalBits": 14704,
  "totalFCS": 0,
  "geoipSrcLocation": [
    -115.14099,
    36.12139
  ],
  "geoipSrcCountry_name": "United States",
  "geoipSrcCountry_iso_code": "US",
  "geoipSrcCity_name": "Las Vegas",
  "geoipDstLocation": [
    4.89949,
    52.3824
  ],
  "geoipDstCountry_name": "Netherlands",
  "geoipDstCountry_iso_code": "NL",
  "geoipDstAsn": 43350,
  "geoipDstOrg": "NForce Entertainment B.V.",
  "geoipDstIsp": "NFOrce Entertainment B.V."
}

```

### @TCP\_RTT\_APP@

Outputs the Application Request -&gt; Response network round trip in milliseconds

Calculation is Half Duplex

Unit is Milliseconds

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

## De-Encapsulation

PCAP2JSON will de-encapsulation some packets and payloads. Following is a description of each field

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

See @DECAP\_SRCIP\_HOSTNAME@  for description 

### @DECAP\_DSTIP\_LOCATION@

See @DECAP\_SRCIP\_LOCATION@  for description 

### @DECAP\_DSTIP\_COUNTRY@

See @DECAP\_SRCIP\_COUNTRY@  for description 

### @DECAP\_DSTIP\_COUNTRY\_CODE@

See @@DECAP\_DSTIP\_COUNTRY\_CODE@

### @DECAP\_DSTIP\_CITY@

See @DECAP\_SRCIP\_CITY@  for description 

### @DECAP\_DSTIP\_ASN@

See @DECAP\_SRCIP\_ASN@  for description 

### @DECAP\_DSTIP\_ORG@

See @DECAP\_SRCIP\_ORG@  for description 

### @DECAP\_DSTIP\_ISP@

See @DECAP\_SRCIP\_ISP@  for description 

