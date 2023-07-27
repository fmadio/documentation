# Getting Packets

FMADIO LXC system high level architecture is shown below.

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption><p>FMADIO LXC Architecture</p></figcaption></figure>

This provides a full lossless filtered version of data directly into the LXC container for processing.&#x20;

Backpressure is provided thru the LXC Ring structure, allowing the application to consume data as fast or slow as possible without dropping anything.



## Sending Data to LXC Offline

During development its typically easiest to send data into the LXC container in a one time replay operation. This allows sending the same data repeatedly to the application allow the engineers to debug and test the code.

In this example we will use the application "fmadio2pcap" which converts the LXC ring into a standard PCAP formatted stream of data.

Its recommended to start the data consumer aka LXC application first, then start the replay.

### Consumer - fmadio2pcap tcpdump

Run the following on the LXC container to generate TCPDUMP output thru the lxc ring named "general"

```
/opt/fmadio/platform/fmadio2pcap  -i /opt/fmadio/queue/lxc_ring_general  | tcpdump  -r - -nn
```

It will look similar to the following on startup, it stops because no packets have been sent to the ring.

```
root@fmadio40v3-440-centos7:/opt/fmadio/platform/fmadio2pcap# ./fmadio2pcap  -i /opt/fmadio/queue/lxc_ring_general  | tcpdump  -r - -nn
fmadio2pcap
FMAD Ring [/opt/fmadio/queue/lxc_ring_general]
Ring size   : 12595200 12595200 16777216
Ring Version:      100      100
Ring Depth  :      400      400
Ring Mask   :      3ff      3ff
RING: Put:1612f 12f
RING: Get:1612f 12f
```

### Producer - stream\_cat sending to LXC ring

On the FMADIO Host system find a capture you want to replay. In this example we are using capture named "inetsample\_20230615\_1513" filename can be found using "stream\_dump"

The command is as follows

```
sudo stream_cat --ring /opt/fmadio/queue/lxc_ring_general  --ring-filter-bpf /opt/fmadio/queue/lxc_ring_general "icmp" -v inetsample_20230615_1513
```

Example output of the command shown below

```
fmadio@fmadio40v3-440:/mnt/store0/tmp2$ sudo stream_cat --ring /opt/fmadio/queue/lxc_ring_general  --ring-filter-bpf /opt/fmadio/queue/lxc_ring_general "icmp" -v inetsample_20230615_1513
Create FMAD Ring:0 [/opt/fmadio/queue/lxc_ring_general]
found ring: [/opt/fmadio/queue/lxc_ring_general] id:0
FMAD Ring:0 [/opt/fmadio/queue/lxc_ring_general] FilterBPF[icmp]
RING[/opt/fmadio/queue/lxc_ring_general                ] 00 : CPU:   0 FilterBPF:[icmp] FilterFrame:[(null)]
StartChunkID: 766030
StartChunk: 766030 Offset: 0 Stride: 1
StartChunk: 766030
RING[/opt/fmadio/queue/lxc_ring_general                ] Size   : 12595200 16777216
RING[/opt/fmadio/queue/lxc_ring_general                ] Version:      100      100
RING[/opt/fmadio/queue/lxc_ring_general                ] Put:54a 14a 0x7f30c8cc1000
RING[/opt/fmadio/queue/lxc_ring_general                ] Get:54a 14a 0x7f30c8cc2000
RING[/opt/fmadio/queue/lxc_ring_general                ] thread:0
RING[/opt/fmadio/queue/lxc_ring_general                ] worker thread start
0M Offset:    0GB ChunkID:766030 TS:00:00:00.000.000.000 | Pending  31590 MB 0.000Gbps 0.000Mpps CPUIdle:0.000 CPUFetch:0.595 CPUSend:0.000
2M Offset:    2GB ChunkID:776263 TS:00:00:00.000.000.000 | Pending  29032 MB 21.164Gbps 2.311Mpps CPUIdle:0.000 CPUFetch:0.955 CPUSend:0.000
4M Offset:    4GB ChunkID:786596 TS:00:00:00.000.000.000 | Pending  26449 MB 21.374Gbps 2.300Mpps CPUIdle:0.000 CPUFetch:0.957 CPUSend:0.000
6M Offset:    7GB ChunkID:796886 TS:00:00:00.000.000.000 | Pending  23876 MB 21.283Gbps 2.318Mpps CPUIdle:0.000 CPUFetch:0.956 CPUSend:0.000
.
.
.

```

And on the LXC client side the output will show ICMP packets printed in tcpdump format as shown below

```
root@fmadio40v3-440-centos7:/opt/fmadio/platform/fmadio2pcap# ./fmadio2pcap  -i /opt/fmadio/queue/lxc_ring_general  | tcpdump  -r - -nn | head -n 1000
fmadio2pcap
FMAD Ring [/opt/fmadio/queue/lxc_ring_general]
Ring size   : 12595200 12595200 16777216
Ring Version:      100      100
Ring Depth  :      400      400
Ring Mask   :      3ff      3ff
RING: Put:29c1 1c1
RING: Get:29c1 1c1
reading from file -, link-type EN10MB (Ethernet)
13:39:53.729815 IP 130.128.35.9 > 210.175.175.21: ICMP echo request, id 4494, seq 1, length 64
13:39:53.731604 IP 210.175.175.21 > 130.128.35.9: ICMP echo reply, id 4494, seq 1, length 64
13:39:53.732577 IP 45.0.191.213 > 8.8.8.8: ICMP echo request, id 13542, seq 25606, length 40
13:39:53.733466 IP 202.17.221.191 > 216.58.200.196: ICMP host 202.17.221.191 unreachable, length 123
13:39:53.733501 IP 202.17.221.191 > 216.58.200.196: ICMP host 202.17.221.191 unreachable, length 123
13:39:53.733641 IP 8.8.8.8 > 45.0.191.213: ICMP echo reply, id 13542, seq 25606, length 40
13:39:53.734071 IP 130.128.255.176 > 103.235.46.39: ICMP echo request, id 65301, seq 2, length 64
13:39:53.744961 IP 130.128.255.81 > 210.175.175.21: ICMP echo request, id 4498, seq 1, length 64
13:39:53.746434 IP 210.175.175.21 > 130.128.255.81: ICMP echo reply, id 4498, seq 1, length 64
13:39:53.762392 IP 12.133.183.36 > 45.0.191.123: ICMP echo reply, id 46807, seq 6777, length 28
13:39:53.770100 IP 45.0.252.242 > 8.8.8.8: ICMP echo request, id 6172, seq 0, length 64
13:39:53.771160 IP 8.8.8.8 > 45.0.252.242: ICMP echo reply, id 6172, seq 0, length 64
13:39:53.775954 IP 130.128.5.7 > 172.21.32.22: ICMP echo request, id 63039, seq 32022, length 40
13:39:53.776217 IP 130.128.5.7 > 172.21.32.21: ICMP echo request, id 63039, seq 32278, length 40
13:39:53.776490 IP 130.128.5.7 > 172.21.32.20: ICMP echo request, id 63039, seq 32534, length 40
13:39:53.776777 IP 130.128.5.7 > 172.21.32.19: ICMP echo request, id 63039, seq 32790, length 40
13:39:53.790153 IP 45.0.191.229 > 8.8.8.8: ICMP echo request, id 34054, seq 39636, length 44
13:39:53.800539 IP 45.0.191.122 > 8.8.8.8: ICMP echo request, id 8265, seq 44218, length 64
13:39:53.801850 IP 8.8.8.8 > 45.0.191.122: ICMP echo reply, id 8265, seq 44218, length 64
.
.
.


```

## Custom Application&#x20;

For a custom application to directly ingest data, the following reference code is provided

Primary header file with all functions and strcutures inline

[https://github.com/fmadio/platform/blob/main/include/fmadio\_packet.h](https://github.com/fmadio/platform/blob/main/include/fmadio\_packet.h)

Core example code to retreive packets

[https://github.com/fmadio/platform/blob/main/fmadio2pcap/main.c#L160-L185](https://github.com/fmadio/platform/blob/main/fmadio2pcap/main.c#L160-L185)

Core loop snippet, this converts from LXC Ring into standard PCAP Packet format

```c
while (!s_Exit)
{
	u64 TS;
	PCAPPacket_t* Pkt	= (PCAPPacket_t*)PktBuffer;

	// fetch packet from ring without blocking
	int ret = FMADPacket_RecvV1(s_RING, false, &TS, &Pkt->LengthWire, &Pkt->LengthCapture, NULL, Pkt + 1);

	// if it has valid data
	if (ret > 0)
	{
		// convert 64b epoch into sec/subsec for pcap
		Pkt->Sec 			= TS / (u64)1e9;
		Pkt->NSec 			= TS % (u64)1e9;

		// write PCAP header and payload 	
		fwrite(PktBuffer, 1, sizeof(PCAPPacket_t) + Pkt->LengthCapture, FPCAP); 

		// general stats
		TotalPkt 	+= 1;
		TotalByte 	+= ret;
	}	

	// end of stream
	if (ret < 0) break;
}
```
