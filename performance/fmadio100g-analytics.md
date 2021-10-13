# FMADIO100G Analytics

Performance baselines

## FW:7003 Ver:623

### Flows 500K @ 50Gbps Slice:128B

Result:

* Data rate 59.97Gbps
* Packet rate 21.164Mpps 

Flow generation

```
 ./pcap_genflow --pktcnt 1e9 --flowcnt 500e3 --imix --pktslice 128 --bps 50e9 | sudo stream_upload --name pcap2json500K_50G
```

Capture size

```
 [1100] pcap2json500K_50G_20211013_1151     99GB Chunk(Cnt:  407008 Start: 2091208 End: 2498215 Comp:0.00) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0
```

Backend performance

```
[20211013_115609] ltrace              :  144 | [Wed Oct 13 11:56:09 2021]:   0.00 usec/msg       0 Msg/sec Ingress[Inflight:   12 Read:0.000 Fwd:0.000] Flow[Snapshot:       0        0/Snap    0.000M/sec      nan Gbps      nan Mpps | Read:0.060 Process:0.140 Merge:0.137 Output:0.000 JSONDump:0.000 MACTop:0.000 GeoIP:0.000 Send:0.000 ] Push[ESSock:   0 ESCnt:    0 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB      0.000 Kdps    0.000 Gbps] signal:false
[20211013_115614] ltrace              :  144 | [Wed Oct 13 11:56:14 2021]:   0.00 usec/msg       0 Msg/sec Ingress[Inflight:   16 Read:0.000 Fwd:0.000] Flow[Snapshot:       0        0/Snap    0.000M/sec    0.000 Gbps    0.000 Mpps | Read:0.057 Process:0.179 Merge:0.129 Output:0.000 JSONDump:0.000 MACTop:0.000 GeoIP:0.000 Send:0.000 ] Push[ESSock:   0 ESCnt: 1208 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    254.374 Kdps    0.249 Gbps] signal:false
[20211013_115619] ltrace              :  144 | [Wed Oct 13 11:56:19 2021]:   1.67 usec/msg  599864 Msg/sec Ingress[Inflight:   14 Read:0.000 Fwd:0.000] Flow[Snapshot:       6   500000/Snap    0.600M/sec   52.565 Gbps   18.544 Mpps | Read:0.031 Process:0.128 Merge:0.071 Output:0.214 JSONDump:0.099 MACTop:0.000 GeoIP:0.033 Send:0.026 ] Push[ESSock:   0 ESCnt: 3920 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    570.824 Kdps    0.560 Gbps] signal:false
[20211013_115624] ltrace              :  144 | [Wed Oct 13 11:56:24 2021]:   1.67 usec/msg  599762 Msg/sec Ingress[Inflight:   14 Read:0.000 Fwd:0.000] Flow[Snapshot:      12   500000/Snap    0.600M/sec   59.976 Gbps   21.158 Mpps | Read:0.023 Process:0.103 Merge:0.050 Output:0.288 JSONDump:0.134 MACTop:0.000 GeoIP:0.044 Send:0.036 ] Push[ESSock:   0 ESCnt: 6739 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    593.258 Kdps    0.581 Gbps] signal:false
[20211013_115629] ltrace              :  144 | [Wed Oct 13 11:56:29 2021]:   1.67 usec/msg  599888 Msg/sec Ingress[Inflight:   16 Read:0.000 Fwd:0.000] Flow[Snapshot:      18   500000/Snap    0.600M/sec   59.989 Gbps   21.163 Mpps | Read:0.019 Process:0.094 Merge:0.040 Output:0.470 JSONDump:0.218 MACTop:0.000 GeoIP:0.070 Send:0.061 ] Push[ESSock:   0 ESCnt: 9530 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    587.428 Kdps    0.576 Gbps] signal:false
[20211013_115634] ltrace              :  144 | [Wed Oct 13 11:56:34 2021]:   1.67 usec/msg  599909 Msg/sec Ingress[Inflight:   15 Read:0.000 Fwd:0.000] Flow[Snapshot:      24   500000/Snap    0.600M/sec   59.991 Gbps   21.163 Mpps | Read:0.016 Process:0.085 Merge:0.033 Output:0.190 JSONDump:0.088 MACTop:0.000 GeoIP:0.028 Send:0.024 ] Push[ESSock:   0 ESCnt:12352 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    594.023 Kdps    0.582 Gbps] signal:false
[20211013_115639] ltrace              :  144 | [Wed Oct 13 11:56:39 2021]:   2.00 usec/msg  499921 Msg/sec Ingress[Inflight:   62 Read:0.000 Fwd:0.000] Flow[Snapshot:      29   500000/Snap    0.500M/sec   49.992 Gbps   17.636 Mpps | Read:0.015 Process:0.084 Merge:0.030 Output:0.442 JSONDump:0.205 MACTop:0.000 GeoIP:0.066 Send:0.056 ] Push[ESSock:   0 ESCnt:15100 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    578.489 Kdps    0.567 Gbps] signal:false
[20211013_115644] ltrace              :  144 | [Wed Oct 13 11:56:44 2021]:   1.67 usec/msg  599250 Msg/sec Ingress[Inflight:   62 Read:0.000 Fwd:0.000] Flow[Snapshot:      35   500000/Snap    0.599M/sec   59.925 Gbps   21.140 Mpps | Read:0.014 Process:0.081 Merge:0.028 Output:0.417 JSONDump:0.194 MACTop:0.000 GeoIP:0.061 Send:0.054 ] Push[ESSock:   0 ESCnt:17900 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    588.757 Kdps    0.577 Gbps] signal:false
[20211013_115649] ltrace              :  144 | [Wed Oct 13 11:56:49 2021]:   1.67 usec/msg  599788 Msg/sec Ingress[Inflight:   14 Read:0.000 Fwd:0.000] Flow[Snapshot:      41   500000/Snap    0.600M/sec   59.978 Gbps   21.159 Mpps | Read:0.013 Process:0.077 Merge:0.025 Output:0.345 JSONDump:0.161 MACTop:0.000 GeoIP:0.051 Send:0.045 ] Push[ESSock:   0 ESCnt:20671 ESErr:0 ESTimeout:    0 ESPending:     0.0 MB    583.141 Kdps    0.572 Gbps] signal:false

```
