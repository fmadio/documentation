# PCAP Flow Generation

One the massive benefits of a full line rate PCAP replay feature is, you can generate PCAPs at any speed, upload them to the FMAD Packet Replay Device and then replay them at any speed you required. There are many ways to generate PCAP files for replay, we will use our builtin utility pcap\_genflow ( [https://github.com/fmadio/pcap\_genflow](https://github.com/fmadio/pcap\_genflow) however many other tools such as tcpreplay, iperf3 and others which can output to a PCAP file for upload.

* is to generate the PCAP using various toolchains
* Upload the PCAP into the FMADIO Capture System
* Replay the capture at any speed

### PCAP Generation

Some example using pcap\_genflow as follows

#### 100Gbps @ 64B Packets, TCP , 1 Billion Unique Flows

Generate 1 billion packets, with 1M unique TCP flows using 64B packets @ 100Gbps

```
$ ./pcap_genflow --pktcnt 1e9 --pktsize 64 --flowcnt 1e6 --bps 100e9  > flow_1M_64B_100G.pcap
```

#### 100Gbps @ 1500B packets, TCP, 1 Million Unique Flows

Generate 100M packets, with 1M unique TCP flows using 1500B packets @ 100Gbp

```
$ ./pcap_genflow --pktcnt 100e6 --pktsize 1500 --flowcnt 1e6 --bps 100e9  > flow_1M_1500B_100G.pcap
```

#### 100Gbps @ IMIX Packet size distribution, TCP, 1 Million Unique Flows

Generate 100M packets, with 1M unique TCP flows using IMIX packet size distribution @ 100Gbps

```
$ ./pcap_genflow --pktcnt 100e6 --imix --flowcnt 1e6 --bps 100e9  > flow_1M_IMIX_100G.pcap
```

### Uploading PCAP into FMADIO

Typically when generating PCAP files the output is written to a linux pipe, the FMAD PCAP File upload function always reads PCAPs from stdin. The tool to upload PCAP into the FMAD capture system is stream\_upload. The syntax is

```
fmadio@fmadio100v2-228:$ sudo ./stream_upload --help
Stream Upload V3: Oct  4 2019 20:00:10
Stream Uploader V3
--------------------------
uploader always reads from stdin

stream_upload 

--append-fcs             : this appends an FCS to all packets
--name <stream_name>     : set the uploaded capture name to stream_name
--verbose                : prints basic statis as upload in progress
--slice <slice amount>   : emulate packet slicing
--time-compress <scale>  : how much to scale the input PCAPS timstamp

fmadio@fmadio100v2-228:/mnt/store0/develop_20191003_rc2/stream_upload$
```

Example of uploading the previously generated file flow\_1M\_IMIX\_100G.pcap. PCAP can be scp to the device, or streamed over an SSH connection

```
$ cat flow_1M_IMIX_100G.pcap | sudo stream_upload --name flow_1M_IMIX_100G 
```

Or this can be generated via on the FMADIO device itself, by piping the output of pcap\_genflow directly to stream\_upload

```
$ ./pcap_genflow --pktcnt 100e6 --imix --flowcnt 1e6 --bps 100e9 | sudo stream_upload --name flow_1M_IMIX_100G 
```

### Replay PCAP File

Once the PCAP has been uploaded into the capture system, it can be replayed with various options as discussed [here](https://fmad.io/en.fmadio10-manual.html#tx-replay-man). It should be clear PCAP Generation + Upload + Replay is a powerful tool for any Network Engineer. If you have suggestions or questions feel free to contact us.
