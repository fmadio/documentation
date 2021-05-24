# ICMP Overwrite

ICMP Overwrite is a feature enabling ICMP Unreachable \(Type 3\) and TimeExceed \(Type 11\) Packets to overwrite the network flow.

This enables exact tracking of network flows that are rejected by a Router/Firewall 

Enabling the feature set the configuration file in /opt/fmadio/etc/pcap2json.lua as follows

```lua

local Config =
{
["General"] =
{
    IsMultiFE       = true,
    IsICMPOverwrite = true,
}
,
```

Setting IsICMPOverwrite = true enables the feature, by default the feature is disabled.

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

The specific fields are icmpUnreach, icmpTimeout and icmpOverwrite

**icmpUnreach**

* this is the total number of packets counted for this half duplex flow with ICMP Unreachable set

**icmpTimeout**

* total number of packets for this half duplex flow with the ICMP Timeout set

**icmpOverwrite**

* boolean flag indicating this flow is generated from ICMP sourced packets. e.g the IPv4 header which is appened to the ICMP message for rejected packets



