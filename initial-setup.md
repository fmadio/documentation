---
description: configuring pcap2json
---

# Initial Setup

pcap2json is a high speed utility that converts PCAP data into JSON flow snapshots. This allows highspeed 10Gbps-100Gbps of capture data size be reduced by x10 into JSON records which can be ingested by an Elastic Stack cluster.

pcap2json pushes data directly to the Elastics Stack cluster, typically over HTTP and port 9200. There is no load balancer, LogStash, Reddis or other system between FMADIO pcap2json and the ES Cluster. as shown below.

![High Level Overview of PCAP2JSON](.gitbook/assets/image%20%2892%29.png)

## PCAP2JSON Plugin

pcap2json is created using FMADIO Packet Capture plugin architecture. It requires the plugin file to load the plugin as follows using the command

```bash
sudo plugin_reload.lua   fmadio_pcap2json_basic_20181121_1428.tcz
```

Example load operation shown below

```bash
fmadio@fmadio20-049:/mnt/store0/pcap2json$ sudo plugin_reload.lua   fmadio_pcap2json_basic_20181121_1428.tcz
Loading Plugin [fmadio_pcap2json_basic_20181121_1428.tcz]
MD5: 351efd347cdd127fccf8375822ac4da0  fmadio_pcap2json_basic_20181121_1428.tcz
reloading pcap2json  [basic]
-----------------------------------------------
Updated:
   basic                          -> basic
   300                            -> 306
   Wed Nov 21 14:28:59 2018       -> Wed Nov 21 14:28:59 2018
-----------------------------------------------
done 0.088614Sec 0.001477Min
fmadio@fmadio20-049:/mnt/store0/pcap2json$

```

When loading plugin the first time on a new system, a reboot may be required to fully load 

Verify the plugin is loaded by checking the version number as follows

```bash
fmadio@fmadio100v2-228U:$ cat /opt/fmadio/version.fmadio_pcap2json_basic
306
Sun May 23 15:43:11 2021
basic
a58461d5ae1cdf78010e476d4d1138da
fmadio@fmadio100v2-228U:$

```

Here we see pugin is at version 306

## PCAP2JSON Config 

Next setup the configuration file located in

```bash
/opt/fmadio/etc/pcap2json.lua
```

Example config file is shown below, if no pcap2json.lua file is in the directory please create one with vi or nano

```lua
local Config =
{
["General"] =
{
        IsMultiFE       = false,
}
,
["pcap2json"] =
{
        "--flow-samplerate 1e9 ",
        "--flow-index-depth 8",
        "--flow-max 4e6",
}
,
["backend"] =
{
        "--index-name pcap2json_index",

        "--output-buffercnt 1024",
        "--output-stdout",
        "--output-stdout",
--       "--output-espush",

        "--es-bulk-action create",
--        "--es-null",

        "--es-host 192.168.1.100:9200:1e6",
--        "--geoip /mnt/store0/etc/pcap2json_map.geo",
}
,
["stream_cat"] =
{
}
}
return Config

```

The above config is a good start, we have left some of options in \["backend"\] commend out to help with debugging per below..

The most import config setting here is

```bash
        "--es-host 192.168.1.100:9200:1e6",
```

Please replace the IP and Port with the appropriate IP Port address of your ES Cluster. You can add multiple lines as above for each ES Node.

Once complete run the below command to confirm no syntax errors in the configuration file.

```bash
fmadio@fmadio100v2-228U:$ fmadiolua /opt/fmadio/etc/pcap2json.lua
argv fmadiolua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/pcap2json.lua]
done 0.000057Sec 0.000001Min
fmadio@fmadio100v2-228U:$
```

## Test Run

Instead of turning everything on at one time and trying to debug the system, a good way is to start with running pcap2json in "offline" mode using stdout with a small PCAP. 

What this does is run pcap2json and instead of pushing to the ES Cluster, it writes the JSON to a local file. This is good as we can confirm pcap2json is functioning correctly first, then look at confirming pcap2json + ES is working correctly.

First ensure the option, per the pcap2json.lua config file above is set

```bash
        "--output-stdout",
```

Next find a small ish sized capture file, using the command

```bash
sudo stream_dump
```

Output of this command will look similar to this

```bash
madio@fmadio100v2-228U:$ sudo stream_dump
Streams:
 [0000]    [this should be empty]      0GB Chunk(Cnt:       0 Start:       1 End:       0 Comp:0.00) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0
 [0423]        test_20210712_1543     59GB Chunk(Cnt:  244288 Start:  634032 End:  878319 Comp:0.00) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0
 [0424]        blah_20210712_1602     59GB Chunk(Cnt:  244632 Start:  878320 End: 1122951 Comp:0.00) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0
 [0433]   interop17_20210716_0716     15GB Chunk(Cnt:   63848 Start: 8475424 End: 8539271 Comp:0.00) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0

fmadio@fmadio100v2-228U:$ sudo stream_dump
```

In the above example we will use the file named "interop17\_20210716\_0716"

Next move to a temporary directory 

```bash
cd /mnt/store0/tmp2/
```

Then start pcap2json in offline mode with the above capture

```bash
sudo /opt/fmadio/analytics/pcap2json_realtime.lua  --offline interop17_20210716_0716
```

Example output as follows.

```bash
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ sudo /opt/fmadio/analytics/pcap2json_realtime.lua  --offline interop17_20210716_0716
loading filename [/opt/fmadio/analytics/pcap2json_realtime.lua]
Args: 1 --offline
Args: 2 interop17_20210716_0716
OpenCtrl [/opt/fmadio/status/analytics] (fSysAnalytics_t*) Length 1048576B
Cmd[sudo killall pcap2json]
killall: pcap2json: no process killed
Cmd[sudo killall pcap2json_backend]
killall: pcap2json_backend: no process killed
Cmd[sudo mkdir -p /mnt/store0/protocol/pcap2json]
Cmd[sudo chown fmadio.staff /mnt/store0/protocol/pcap2json]
Got BPF Filter []
Got  Filter []
Cmd[sudo /opt/fmadio/bin/pcap2json_backend --uid pcap2json_1627043476265853952 --cpu-flow 7 17 18 19 20 21 22 23  --cpu-pipe 0 12 --cpu-pipe 1 13 --cpu-pipe 2 14 --cpu-pipe 3 15 --cpu-output 1 16   --pipe-count 4  --input-pipe 0 /opt/fmadio/queue/pcap2json_0  --input-pipe 1 /opt/fmadio/queue/pcap2json_1  --input-pipe 2 /opt/fmadio/queue/pcap2json_2  --input-pipe 3 /opt/fmadio/queue/pcap2json_3  --index-name pcap2json_test7 --output-buffercnt 1024 --output-stdout --es-bulk-act
ion create --es-host 192.168.2.176:9200:1e6 --geoip /mnt/store0/etc/pcap2json_map.geo > /mnt/store0/log/pcap2json_backend_20210723_2131  2>&1 &]
Cmd[rm -R /mnt/store0/log/pcap2json_backend.stdout.cur]
Cmd[ln -s /mnt/store0/log/pcap2json_backend_20210723_2131 /mnt/store0/log/pcap2json_backend.stdout.cur]
wait for BE to spin up 0
wait for BE to spin up 1
wait for BE to spin up 2
Cmd[sudo /opt/fmadio/bin/stream_cat --uid pcap2json_1627043476265853952 --shmring --io-priority 90  --cpu 48  --ignore_fcs --pktslice 96   --shmring-stride-offset 4 0  interop17_20210716_0716 | /opt/fmadio/bin/pcap2json --uid pcap2json_1627043476265853952 --cpu-core 56 --cpu-flow 11  60 64 68 72 80 84 88 92 36 40 44 --output-pipe /opt/fmadio/queue/pcap2json_0  --instance-id 0 --instance-max 4  --icmp-overwrite  -v --flow-samplerate 1e9  --flow-index-depth 8 --flow-max 4e6 > /
mnt/store0/log/pcap2json_0_20210723_2131  2>&1 &]
Cmd[sudo /opt/fmadio/bin/stream_cat --uid pcap2json_1627043476265853952 --shmring --io-priority 90  --cpu 50  --ignore_fcs --pktslice 96   --shmring-stride-offset 4 1  interop17_20210716_0716 | /opt/fmadio/bin/pcap2json --uid pcap2json_1627043476265853952 --cpu-core 57 --cpu-flow 11  61 65 69 73 81 85 89 93 37 41 45 --output-pipe /opt/fmadio/queue/pcap2json_1  --instance-id 1 --instance-max 4  --icmp-overwrite  -v --flow-samplerate 1e9  --flow-index-depth 8 --flow-max 4e6 > /
mnt/store0/log/pcap2json_1_20210723_2131  2>&1 &]
stream_cat UID [pcap2json_1627043476265853952]
SHM Ring Output
stream_cat IOPriority set: 90
stream_cat CPU Affinity: 48
stream_cat: PktSlice: 96
.
.
.

```

After it has completed running there will be a file name "stdout" in the directory

```bash
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ ls -alh stdout
-rw-r--r--    1 root     root        1.2G Jul 23 21:31 stdout
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$
```

This is the JSON records PCAP2JSON generated, but written to a file.  Example shown below

```javascript
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ cat stdout | head |jq
{
  "create": {
    "_index": "pcap2json_index",
    "pipeline": null
  }
}
{
  "timestamp": 1497015593000,
  "TS": "13:39:53.000.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "a8c5feb934f0525e7c8aed3aa14e07898129e098",
  "hashFull": "9032af87f6002122a07e88ce402ccf10fde32a43",
  "flowCount": 97963,
  "macSrc": "7c:e2:ca:bd:97:d9",
  "macDst": "00:0e:52:80:00:16",
  "macProto": "IPv4",
  "vlan0": null,
  "mpls0TC": null,
  "ipv4Src": "150.100.29.14",
  "hostSrc": "150.100.29.14",
  "ipv4Dst": "130.128.19.30",
  "hostDst": "130.128.19.30",
  "ipv4Proto": "UDP",
  "ipv4DSCP": null,
  "ipv4Frag": null,
  "portSrc": 10662,
  "portDst": 5004,
  "application": "(UDP  5004)",
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
  "totalPackets": 4791349,
  "totalBytes": 6583313526,
  "totalBits": 52666508208,
  "totalFCS": 0,
  "geoipSrc": {
    "location": [
      139.69,
      35.68999
    ],
    "country_name": "Japan",
    "country_iso_code": "JP",
    "city_name": null,
    "asn": 2907,
    "org": "Research Organization of Information and Systems",
    "isp": "Research Organization of Information and Systems"
  },
  "geoipDst": {
    "location": [
      -115.14099,
      36.12139
    ],
    "country_name": "United States",
    "country_iso_code": "US",
    "city_name": "Las Vegas",
    "asn": null,
    "org": null,
    "isp": null
  },
  "icmpUnreach": null,
  "icmpTimeout": null,
  "icmpOverwrite": null
}
{
  "create": {
    "_index": "pcap2json_index",
    "pipeline": null
  }
}
{
  "timestamp": 1497015593000,
  "TS": "13:39:53.000.000.000",
  "device": "fmadio100v2-228U",
  "hashHalf": "c9d83f7ebe0115357b1c909b88ba8c97cbf1647f",
  "hashFull": "f10e75a146fbbc3f4721835cfab9e217f4608da5",
  "flowCount": 97963,
  "macSrc": "84:78:ac:3b:53:a5",
  "macDst": "cc:e1:d5:92:fa:fb",
  "macProto": "IPv4",
  "vlan0": 2047,
  "mpls0TC": null,
  "ipv4Src": "119.243.74.94",
  "hostSrc": "119.243.74.94",
  "ipv4Dst": "130.128.47.5",

```

If your stdout file looks similar, ready to connect the system to Elastic Stack. If not please check the above steps again until stdout file looks correct.

## Elastic Stack Push

After the Test Run writing to file "stdout" is functioning correctly, next step is to connect and push that JSON data to Elastic Stack.

First is checking the ES version, via CLI as below. We are using Elastic Stack 7.10+ \(7.10.1\)

```javascript
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ curl http://192.168.1.100:9200/
{
  "name" : "fmadio-lts.0",
  "cluster_name" : "fmadio-lts",
  "cluster_uuid" : "yfx5yWaUTUiXhQjZ0I67Ug",
  "version" : {
    "number" : "7.10.1",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "1c34507e66d7db1211f66f3513706fdf548736aa",
    "build_date" : "2020-12-05T01:00:33.671820Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$

```

### Elastic Search Mapping 

Next create a default ES mapping per the following command. Please replace 192.168.1.100/9200 with your ES node address

```javascript
/usr/local/bin/curl -H "Content-Type: application/json"  -XPUT "192.168.1.100:9200/pcap2json_index?pretty" --data-binary "@mapping.json" | jq
```

The default mapping file is located below

{% file src=".gitbook/assets/mapping.json" %}

Example output below

```javascript
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ /usr/local/bin/curl -H "Content-Type: application/json"  -XPUT "192.168.2.147:9200/pcap2json_index?pretty" --data-binary "@mapping.json" | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  4798  100    91  100  4707     31   1628  0:00:02  0:00:02 --:--:--  1659
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "pcap2json_index"
}
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$

```

Next push a small part of stdout file manually to ES as follows. This writes the first 10 lines in the JSON file to the ES cluster

```javascript
 head -n 10 stdout  | /usr/local/bin/curl -H "Content-Type: application/json" -XPOST "192.168.1.100:9200/_bulk/?pretty" --data-binary "@-" | jq
```

The correct output showing indexing has been successful as below

```javascript
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ head -n 10 stdout  | /usr/local/bin/curl -H "Content-Type: application/json" -XPOST "192.168.1.100:9200/_bulk/?pretty" --data-binary "@-" | jq

{
  "took": 71,
  "errors": false,
  "items": [
    {
      "create": {
        "_index": "pcap2json_index",
        "_type": "_doc",
        "_id": "_k1_03oBeu-Jzkwd7A06",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 5000,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "create": {
        "_index": "pcap2json_index",
        "_type": "_doc",
        "_id": "_01_03oBeu-Jzkwd7A06",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 5001,
        "_primary_term": 1,
        "status": 201
      }
    },

```

If the above is functioning correctly, can next push the output to ES

### Elastic Search Offline Mode

Pushing captures to ES in offline mode is good for lab/debug/troubleshooting a system. As its easier to control than the 24/7 running mode

Start by updating the config file /opt/fmadio/etc/pcap2json.lua as follows. The only real difference is "--output-espush" is set instead of "--output-stdout".

Ensure the "--es-host &lt;es node hostname&gt;:&lt;port&gt;:1e6" is set correctly

```javascript
local Config =
{
["General"] =
{
        IsMultiFE       = false,
}
,
["pcap2json"] =
{
        "--flow-samplerate 1e9 ",
        "--flow-index-depth 8",
        "--flow-max 4e6",
}
,
["backend"] =
{
        "--index-name pcap2json_index",

        "--output-buffercnt 1024",
        "--output-espush",

        "--es-bulk-action create",
        "--es-host 192.168.1.100:9200:1e6",
}
,
["stream_cat"] =
{
}
}
return Config
```

Next run the same offline command as the STDOUT version above 

```javascript
sudo /opt/fmadio/analytics/pcap2json_realtime.lua  --offline interop17_20210716_0716
```

The output will look the same, except it will push data to the ES Host now instead of writing to a local file. 

Trouble shooting and debug please see the logfiles [https://docs.fmad.io/fmadio-documentation/v/pcap2json/logfiles](https://docs.fmad.io/fmadio-documentation/v/pcap2json/logfiles)

