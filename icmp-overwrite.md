# De-Encapsulation

PCAP2JSON can do basic packet de-encapsulation. Specifically ICMP and IPv4 GRE, these fields are populated in the decap\* JSON fields. Please note the de-encapsulation is include in the unique half duplex flow calculation.

Example output 

```javascript
{
  "timestamp": 1497015595000,
  "TS": "13:39:55.000.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "34f43ffa0506879662d2396fc6d01af2b91eb458",
  "hashFull": "093d8f99124181484a2b49eb6ca61b6e1373f251",
  "flowCount": 311918,
  "macSrc": "7c:e2:ca:bd:97:d9",
  "macDst": "00:0e:52:80:00:16",
  "macProto": "IPv4",
  "vlan0": null,
  "mpls0TC": null,
  "ipv4Src": "216.229.0.179",
  "hostSrc": "216.229.0.179",
  "ipv4Dst": "130.128.255.183",
  "hostDst": "130.128.255.183",
  "ipv4Proto": "UDP",
  "ipv4DSCP": null,
  "ipv4Frag": null,
  "portSrc": 123,
  "portDst": 20508,
  "application": "(UDP   123)",
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
  "totalPackets": 10,
  "totalBytes": 1140,
  "totalBits": 9120,
  "totalFCS": 0,
  "geoipSrc": {
    "location": [
      -96.6461,
      40.78419
    ],
    "country_name": "United States",
    "country_iso_code": "US",
    "city_name": "Lincoln",
    "asn": 7806,
    "org": "ASN7806",
    "isp": "Binary Net"
  },
  "geoipDst": {
    "location": [
      -115.1406,
      36.12089
    ],
    "country_name": "United States",
    "country_iso_code": "US",
    "city_name": "Las Vegas",
    "asn": null,
    "org": null,
    "isp": null
  },
  "icmpUnreach": 10,
  "icmpTimeout": null,
  "icmpOverwrite": true
}

```



