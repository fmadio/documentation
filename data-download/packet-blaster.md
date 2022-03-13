# Packet Blaster

All Gen2+ FMADIO devices have a built in packet blaster / Layer 2 packet generator. This allows a single system to be entirely self contained for unit and system testing. In addition FMADIO devices can also load test network devices such as switching and firewalls, checking for physical layer links and measuring network path latency.

Packet blaster is a layer 2 (Ethernet level) packet generator that runs at full line rate @ 64B to 9218 Jumbo sized packets. Generation is performed entirely on the FPGA Capture card thus up to full 100Gbps @ 64B 148Mpps packets can be generated without generation variance. Packet generation and capture can run simultaneously, thus verification the capture device is operating correctly is achieved.

The payload of each packet is a per physical port MAC Address followed by a 32bit incrementally increasing sequence number. This sequence number is used later post capture to ensure data of all packets has been captured correctly without error. An example packet is shown in Wireshark below.

![Packet Blaster](<../.gitbook/assets/image (127) (1).png>)

In the above wireshark picture, you can see 2 different MAC address 11:11:11:11:11:11 (Physical Port 1) and 22:22:22:22:22 (Physical Port 2). The payload is a 32bit little endian sequence numbers

```
0x11111111 (Per Physical Port MAC Address)
0x11111111 (Per Physical Port MAC Address)
0x11111111 (Per Physical Port MAC Address)
0x11111111 (Per Physical Port MAC Address)
0x11111111 (Per Physical Port MAC Address)

0xca560dc7 (Data payload Seq Number + 0)
0xca560dc8 (Data payload Seq Number + 1)
0xca560dc9 (Data payload Seq Number + 2)
0xca560dca (Data payload Seq Number + 3)
0xca560dcb (Data payload Seq Number + 4)
0xca560dcc (Data payload Seq Number + 5)
0xca560dce (Data payload Seq Number + 5)
0xca560dcf (Data payload Seq Number + 6)
0xca560dd0 (Data payload Seq Number + 7)
0xca560dd1 (Data payload Seq Number + 8)
0x313e49f0 (Frame Check Sequence)
```

This sequence number and MAC address allow the analysis software to not only check the total number of packets captured by the device, but also check every byte of the payload has been captured without error. As the analysis software knows exactly what the packet payload data should be via the sequence number.

## Operation

Packet blaster is operated only by the CLI interface, each FMAD SKU has a slightly different operation

### FMADIO100v2

```
fmadio@fmadio100v2-228:$ sudo ./stream_generate_f100 --blaster --help
Packet Gen: Oct  3 2019 18:44:18
stream_generate_f100 --blaster ::: FMADIO 100G Packet Blaster :::

Command line options:
  --pktsize      <packet size>    : Size of each packet (64-9218) Default 64B
  --pktcnt       <packet count>   : Total number of packets to generate. Scientific notation accepted
  --gbps         <data rate>      : Data rate to generate at, e.g. 50.0 Gbps (Default is 100G line rate)
  --port-enable  <port mask>      : Which ports to enable 0 is disable, 1 enable (Default is single port 01)

fmadio@fmadio100v2-228:$

```

Example operation, generate 1 billion 64B packets on a single 100G port at full line rate

```
fmadio@fmadio100v2-228:$ sudo ./stream_generate_f100 --blaster --pktcnt 1e9 --pktsize 64 --port-enable 01
Packet Gen: Oct  3 2019 18:44:18
PktCnt: 1000000000
PktSize: 64
PortEnable: 0 1
Generate: PktSize:64 PktCnt:1000.000M Gbps:100.000000
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
GenConfig(0, 1,  1, 1, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 0, 0x0, 0x4,  1, 0, 1, 0x0, 0x4)
Config Length: 40
TargetPkt Time: 51.200001ns per packet
DataPktTime   : 51.200001
Add extra     : 0.000000 ns
Pad Cycles    : 0
Final Wait    : 1
PortMode: 0 1
update 191 35712 0.000000 : 03010122
update 14212540 909614080 72.734941 : 05000134
update 28424336 1819170048 145.459036 : 05000134
update 42635968 2728713536 218.187497 : 05000134
update 56842067 3637903488 290.999828 : 05000134
update 71052937 4547399168 363.629380 : 05000134
update 85262420 5456806464 436.391969 : 03010122
update 99468647 6366004608 509.219176 : 03010122
.
.
.
.
```

## Data Verification

Payload data verification on the device is achieved by "linux-cat-ing" a capture down a linux pipe to the builtin utility capinfos2. The syntax looks as follows

### FMADIO100v2

```
fmadio@fmadio100v2-228:$ sudo stream_cat --ignore_fcs <capture name>| capinfos2  -v --seq --with-fcs --disable-portid
No PortID
PCAP nano
Port:4 new seq: af000000 Packets: 0 Length:  68
0.00GB    0.000 Gbps    0.000 Mpps
0.33GB    2.584 Gbps    3.845 Mpps
0.74GB    3.295 Gbps    4.903 Mpps
1.09GB    2.831 Gbps    4.213 Mpps
1.40GB    2.487 Gbps    3.701 Mpps
1.78GB    3.034 Gbps    4.514 Mpps
2.15GB    2.932 Gbps    4.363 Mpps
2.50GB    2.816 Gbps    4.191 Mpps
2.82GB    2.556 Gbps    3.804 Mpps
3.23GB    3.260 Gbps    4.852 Mpps
3.59GB    2.893 Gbps    4.305 Mpps
3.90GB    2.487 Gbps    3.701 Mpps
4.26GB    2.877 Gbps    4.281 Mpps
4.56GB    2.362 Gbps    3.514 Mpps
4.86GB    2.439 Gbps    3.629 Mpps
5.16GB    2.385 Gbps    3.549 Mpps
5.47GB    2.434 Gbps    3.622 Mpps
5.87GB    3.221 Gbps    4.793 Mpps
6.17GB    2.406 Gbps    3.581 Mpps
6.59GB    3.306 Gbps    4.920 Mpps
6.95GB    2.859 Gbps    4.254 Mpps
7.29GB    2.705 Gbps    4.025 Mpps
7.63GB    2.773 Gbps    4.126 Mpps
7.99GB    2.862 Gbps    4.259 Mpps
8.35GB    2.883 Gbps    4.290 Mpps
packet stream end
SUCCESS
8.40GB    0.321 Gbps    0.478 Mpps
Total Packets  : 100000000
TotalBytes     : 6800000000
TotalPackets   : 100000000
PayloadCRC     : 4791a6a7add60780
ErrorSeq       : 0
ErrorPktSize   : 0
LastByte       : 0x0e5e0fff
SeqStart       : 0x00000000 0x00000000 0x00000000 0x00000000 : 0xaf000000
SeqEnd         : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x0e5e0fff
PacketCnt      : 0 0 0 0
TimeOrder      : 0
CRCFail        : 0
TotalPCAPTime  : 0 ns
Bandwidth      : 77.273 Gbps
Packet Rate    : 142.045 Mpps

Complete
fmadio@fmadio100v2-228:$

```

