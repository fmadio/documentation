# Replay & Capture (Slow)

FMADIO can replay PCAP traffic at low speed while simultaneously capturing at full line rate. This can be very helpful for debugging or other troubleshooting and does not require changing the FPGA firmware from Capture to Replay mode. This Replay mode uses PIO (Programmed IO) who's bandwidth is quite low, and is not suitable for full high bandwidth packet relay. Usage is as follows

### FMADIO20v3

Example command on the 20G Packet Capture system

```
/opt/fmadio/bin/stream_generate_f20 --replay
```

Example below pipes a PCAP previously captured on the device back down the the capture interfaces.

```
fmadio@fmadio20v2-149:~$ sudo stream_cat --ignore_fcs test64_20191004_1151 | sudo stream_generate_f20 --replay
Packet Gen: Sep 22 2019 01:05:06
stream_cat ioqueue: 4
SetAffinity: CPU 30 Index:16
StartChunkID: 8994
PCAP Nano
Replay Pkts:         1 Drop:         0 Total:0.000GB 0.396Gbps
Replay Pkts:    649312 Drop:         0 Total:0.042GB 0.332Gbps
Replay Pkts:   1299210 Drop:         0 Total:0.083GB 0.333Gbps
Replay Pkts:   1949573 Drop:         0 Total:0.125GB 0.333Gbps
Replay Pkts:   2599309 Drop:         0 Total:0.166GB 0.333Gbps
Replay Pkts:   3249180 Drop:         0 Total:0.208GB 0.333Gbps
Replay Pkts:   3899671 Drop:         0 Total:0.250GB 0.333Gbps
Replay Pkts:   4549914 Drop:         0 Total:0.291GB 0.333Gbps
Replay Pkts:   5199871 Drop:         0 Total:0.333GB 0.333Gbps
Replay Pkts:   5828751 Drop:         0 Total:0.373GB 0.332Gbps
Replay Pkts:   6478494 Drop:         0 Total:0.415GB 0.332Gbps
Replay Pkts:   7128289 Drop:         0 Total:0.456GB 0.332Gbps
Replay Pkts:   7777780 Drop:         0 Total:0.498GB 0.332Gbps
Replay Pkts:   8425123 Drop:         0 Total:0.539GB 0.332Gbps
Replay Pkts:   9074288 Drop:         0 Total:0.581GB 0.332Gbps
Replay Pkts:   9723647 Drop:         0 Total:0.622GB 0.332Gbps
Replay Pkts:  10379903 Drop:         0 Total:0.664GB 0.332Gbps
Replay Pkts:  11029807 Drop:         0 Total:0.706GB 0.332Gbps
Replay Pkts:  11679917 Drop:         0 Total:0.748GB 0.332Gbps
Replay Pkts:  12327988 Drop:         0 Total:0.789GB 0.332Gbps
Replay Pkts:  12977595 Drop:         0 Total:0.831GB 0.332Gbps
Replay Pkts:  13629543 Drop:         0 Total:0.872GB 0.332Gbps
Replay Pkts:  14285072 Drop:         0 Total:0.914GB 0.332Gbps
Replay Pkts:  14928107 Drop:         0 Total:0.955GB 0.332Gbps
Replay Pkts:  15575152 Drop:         0 Total:0.997GB 0.332Gbps
Replay Pkts:  16222518 Drop:         0 Total:1.038GB 0.332Gbps
Replay Pkts:  16871634 Drop:         0 Total:1.080GB 0.332Gbps
Replay Pkts:  17521499 Drop:         0 Total:1.121GB 0.332Gbps
Replay Pkts:  18177284 Drop:         0 Total:1.163GB 0.332Gbps
Replay Pkts:  18823901 Drop:         0 Total:1.205GB 0.332Gbps
Replay Pkts:  19472977 Drop:         0 Total:1.246GB 0.332Gbps
packet stream end
SUCCESS
STDIN Read fail: 0
Replay Pkts:  20000000 Drop:         0 Total:1.280GB 0.322Gbps
fmadio@fmadio20v2-149:~$

```

### FMADIO100v2

For SKUs FMADIO100v2 and FMADIO40v3

```
PCAP linux stdin pipe |  sudo /opt/fmadio/bin/stream_generate_f100 --replay_pio
```

You can pipe a PCAP from a local file system or use stream\_cat to pipe from the capture system. Example below pipes from a previous captures.

```
fmadio@fmadio100v2-228:$ sudo stream_cat --ignore_fcs testk9k_b_20200727_0106 | sudo ./stream_generate_f100  --replay_pio

Packet Gen: Jul 27 2020 00:24:51
map 0x7f73115e0000
0x7f7308c4a000
StartChunk: 3856856
PCAP Nano
Replay Pkts:         1 Drop:         0 Total:0.000GB 0.579Gbps
Replay Pkts:     40225 Drop:         0 Total:0.056GB 0.451Gbps
Replay Pkts:     80542 Drop:         0 Total:0.113GB 0.451Gbps
Replay Pkts:    120730 Drop:         0 Total:0.169GB 0.451Gbps
Replay Pkts:    160842 Drop:         0 Total:0.225GB 0.450Gbps
Replay Pkts:    200879 Drop:         0 Total:0.281GB 0.450Gbps
Replay Pkts:    240992 Drop:         0 Total:0.337GB 0.450Gbps
Replay Pkts:    281222 Drop:         0 Total:0.394GB 0.450Gbps
Replay Pkts:    321452 Drop:         0 Total:0.450GB 0.450Gbps
Replay Pkts:    361580 Drop:         0 Total:0.506GB 0.450Gbps
Replay Pkts:    401815 Drop:         0 Total:0.563GB 0.450Gbps
Replay Pkts:    442053 Drop:         0 Total:0.619GB 0.450Gbps
Replay Pkts:    482276 Drop:         0 Total:0.675GB 0.450Gbps
Replay Pkts:    522380 Drop:         0 Total:0.731GB 0.450Gbps
Replay Pkts:    562478 Drop:         0 Total:0.787GB 0.450Gbps
Replay Pkts:    602678 Drop:         0 Total:0.844GB 0.450Gbps
Replay Pkts:    642979 Drop:         0 Total:0.900GB 0.450Gbps
Replay Pkts:    683165 Drop:         0 Total:0.956GB 0.450Gbps
Replay Pkts:    723334 Drop:         0 Total:1.013GB 0.450Gbps
Replay Pkts:    763528 Drop:         0 Total:1.069GB 0.450Gbps
Replay Pkts:    803801 Drop:         0 Total:1.125GB 0.450Gbps
Replay Pkts:    843917 Drop:         0 Total:1.181GB 0.450Gbps
Replay Pkts:    884163 Drop:         0 Total:1.238GB 0.450Gbps
Replay Pkts:    924157 Drop:         0 Total:1.294GB 0.450Gbps
.
.
.
.
```

The following example pipes a PCAP on the localfile system (/mnt/store0/tmp/imix.pcap) to replay the traffic

```
fmadio@fmadio100v2-228$ cat imix10.pcap | sudo stream_generate_f100  --replay_pio

Packet Gen: Jul 28 2020 01:05:12
SetAffinity: CPU 1 Index:18
PCAP Nano
Replay Pkts:         1 Drop:         0 Total:0.000GB 0.506Gbps
Replay Pkts:    137652 Drop:         0 Total:0.049GB 0.389Gbps
Replay Pkts:    273955 Drop:         0 Total:0.097GB 0.390Gbps
Replay Pkts:    411187 Drop:         0 Total:0.146GB 0.391Gbps
Replay Pkts:    548794 Drop:         0 Total:0.196GB 0.391Gbps
Replay Pkts:    686374 Drop:         0 Total:0.244GB 0.391Gbps
Replay Pkts:    822256 Drop:         0 Total:0.293GB 0.390Gbps
Replay Pkts:    955237 Drop:         0 Total:0.340GB 0.389Gbps
Replay Pkts:   1092409 Drop:         0 Total:0.389GB 0.389Gbps
Replay Pkts:   1227813 Drop:         0 Total:0.437GB 0.389Gbps
Replay Pkts:   1361926 Drop:         0 Total:0.485GB 0.388Gbps
Replay Pkts:   1498830 Drop:         0 Total:0.534GB 0.388Gbps
Replay Pkts:   1634867 Drop:         0 Total:0.582GB 0.388Gbps
Replay Pkts:   1771895 Drop:         0 Total:0.631GB 0.388Gbps
Replay Pkts:   1907879 Drop:         0 Total:0.680GB 0.388Gbps
Replay Pkts:   2039344 Drop:         0 Total:0.726GB 0.387Gbps
Replay Pkts:   2176207 Drop:         0 Total:0.775GB 0.388Gbps
Replay Pkts:   2310531 Drop:         0 Total:0.823GB 0.387Gbps
Replay Pkts:   2446486 Drop:         0 Total:0.871GB 0.387Gbps
Replay Pkts:   2583970 Drop:         0 Total:0.921GB 0.388Gbps
Replay Pkts:   2722008 Drop:         0 Total:0.970GB 0.388Gbps
Replay Pkts:   2859852 Drop:         0 Total:1.019GB 0.388Gbps
Replay Pkts:   2997241 Drop:         0 Total:1.068GB 0.388Gbps
Replay Pkts:   3134724 Drop:         0 Total:1.117GB 0.388Gbps
.
.
.
.
```
