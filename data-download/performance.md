# Performance

Download performance examples using the CLI interface to extract PCAP data to downstream systems.

Please use "stream\_dump" to list all captures which can be used with 

```text
sudo stream_cat -v  <capture name> |  .... 
```

the output of stream\_cat is a standard PCAP via the stdout pipe

NOTE: throughput is heavily dependent on the packet size mix of the capture. Lager packet sizes get a better over all throughput

### Manual NFS Writes \(10G Management\)

Commands to write data to an NFS share over a 10G management interface ~ 5Gbps throughput

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 > /mnt/remote0/pcap/blah.pcap

0M Offset:    0GB Pkt:1622282576_055251189 Length: 240 Capture: 240 ChunkID:29484752 0.561Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_629796243 Length:9200 Capture:9200 ChunkID:29487452 5.576Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_749333062 Length:9200 Capture:9200 ChunkID:29489764 4.771Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_869080067 Length:9200 Capture:9200 ChunkID:29492081 4.771Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282586_016069259 Length:9200 Capture:9200 ChunkID:29494924 5.869Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_158294580 Length:9200 Capture:9200 ChunkID:29497677 5.683Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_299258901 Length:9200 Capture:9200 ChunkID:29500403 5.611Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_438808972 Length:9200 Capture:9200 ChunkID:29503103 5.572Gbps CPUIdle:0.000
0M Offset:    5GB Pkt:1622282586_574100988 Length:9200 Capture:9200 ChunkID:29505719 5.403Gbps CPUIdle:0.000
0M Offset:    5GB Pkt:1622282586_720528885 Length:9200 Capture:9200 ChunkID:29508552 5.847Gbps CPUIdle:0.000
0M Offset:    6GB Pkt:1622282586_862964788 Length:9200 Capture:9200 ChunkID:29511307 5.686Gbps CPUIdle:0.000
0M Offset:    6GB Pkt:1622282586_987963325 Length:9200 Capture:9200 ChunkID:29513725 4.975Gbps CPUIdle:0.000
0M Offset:    7GB Pkt:1622282587_103929106 Length:9200 Capture:9200 ChunkID:29515970 4.629Gbps CPUIdle:0.000
0M Offset:    8GB Pkt:1622282587_228624524 Length:9200 Capture:9200 ChunkID:29518382 4.979Gbps CPUIdle:0.000
1M Offset:    8GB Pkt:1622282587_368959022 Length:9200 Capture:9200 ChunkID:29521096 5.604Gbps CPUIdle:0.000

```

### Manual SSH  \(1G Management\)

Stock SSH over 1G management port = 0.48Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 | ssh 192.168.1.175 " cat > /mnt/store0/tmp2/rclone/blah.pcap"

0M Offset:    0GB Pkt:1622282585_492207223 Length:9200 Capture:9200 ChunkID:29484791 0.048Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_503385400 Length:9200 Capture:9200 ChunkID:29485007 0.446Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_514746376 Length:9200 Capture:9200 ChunkID:29485227 0.454Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_526153512 Length:9200 Capture:13296 ChunkID:29485447 0.455Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_537787748 Length:9200 Capture:9200 ChunkID:29485673 0.465Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_549433060 Length:9200 Capture:9200 ChunkID:29485898 0.465Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_561237164 Length:9200 Capture:9200 ChunkID:29486126 0.471Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_573192673 Length:9200 Capture:9200 ChunkID:29486357 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_585146336 Length:9200 Capture:9200 ChunkID:29486589 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_597101846 Length:9200 Capture:9200 ChunkID:29486820 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_609070280 Length:9200 Capture:9200 ChunkID:29487051 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_621018404 Length:9200 Capture:9200 ChunkID:29487282 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_632968384 Length:9200 Capture:9200 ChunkID:29487514 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_644910995 Length:9200 Capture:9200 ChunkID:29487745 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_656883149 Length:9200 Capture:9200 ChunkID:29487976 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_668831299 Length:9200 Capture:9200 ChunkID:29488207 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_680736982 Length:9200 Capture:9200 ChunkID:29488438 0.475Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_692709135 Length:9200 Capture:9200 ChunkID:29488669 0.477Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_704675750 Length:9200 Capture:9200 ChunkID:29488901 0.478Gbps CPUIdle:0.000

```

### Manual SSH \(10G Management\)

Stock SSH over 10G management port also ~ 0.48 Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 | ssh 192.168.2.175 " cat > /mnt/store0/tmp2/rclone/blah.pcap"
0M Offset:    0GB Pkt:1622282585_492207223 Length:9200 Capture:9200 ChunkID:29484791 0.070Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_500733952 Length:9200 Capture:9200 ChunkID:29484956 0.340Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_512887039 Length:9200 Capture:9200 ChunkID:29485191 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_525032740 Length:9200 Capture:9200 ChunkID:29485426 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_537183972 Length:9200 Capture:9200 ChunkID:29485661 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_549337047 Length:9200 Capture:9200 ChunkID:29485896 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_561488276 Length:9200 Capture:9200 ChunkID:29486131 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_573639505 Length:9200 Capture:9200 ChunkID:29486366 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_585790734 Length:9200 Capture:9200 ChunkID:29486601 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_597936423 Length:9200 Capture:9200 ChunkID:29486836 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_610063649 Length:9200 Capture:9200 ChunkID:29487071 0.484Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_622211185 Length:9200 Capture:13296 ChunkID:29487305 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_634360584 Length:9200 Capture:13296 ChunkID:29487540 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_646524765 Length:9200 Capture:9200 ChunkID:29487776 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_658696328 Length:9200 Capture:9200 ChunkID:29488011 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_670834659 Length:9200 Capture:9200 ChunkID:29488246 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_682980376 Length:9200 Capture:9200 ChunkID:29488481 0.485Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_695113167 Length:9200 Capture:9200 ChunkID:29488716 0.484Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_707268116 Length:9200 Capture:9200 ChunkID:29488951 0.485Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_718389073 Length:9200 Capture:9200 ChunkID:29489166 0.444Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_728784387 Length:9200 Capture:9200 ChunkID:29489367 0.413Gbps CPUIdle:0.0
```

### Manual SSH arcfour \(10G Management\)

Using arcfour cipher "-c arcfour" performance is about 1.2Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 | ssh -c arcfour 192.168.2.175 " cat > /mnt/store0/tmp2/rclone/blah.pcap"

0M Offset:    0GB Pkt:1622282585_506241800 Length:9200 Capture:9200 ChunkID:29485062 0.643Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_539874192 Length:9200 Capture:9200 ChunkID:29485713 1.343Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_571565986 Length:9200 Capture:9200 ChunkID:29486326 1.265Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_603374103 Length:9200 Capture:9200 ChunkID:29486941 1.270Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_635289327 Length:9200 Capture:9200 ChunkID:29487558 1.273Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_667396635 Length:9200 Capture:13296 ChunkID:29488179 1.282Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_697670452 Length:9200 Capture:9200 ChunkID:29488765 1.208Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_729663278 Length:9200 Capture:9200 ChunkID:29489384 1.277Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_763941843 Length:9200 Capture:9200 ChunkID:29490047 1.369Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_797193805 Length:9200 Capture:9200 ChunkID:29490690 1.328Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_824173520 Length:9200 Capture:9200 ChunkID:29491212 1.077Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_851123723 Length:9200 Capture:9200 ChunkID:29491733 1.075Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_877660338 Length:9200 Capture:9200 ChunkID:29492247 1.059Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_904750875 Length:9200 Capture:9200 ChunkID:29492771 1.081Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_935715193 Length:9200 Capture:9200 ChunkID:29493369 1.236Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_967401494 Length:9200 Capture:9200 ChunkID:29493982 1.265Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_999126569 Length:9200 Capture:9200 ChunkID:29494596 1.266Gbps CPUIdle:0.000

```

### Manual unencrypted TCP netcat \(10G Management\)

using netcat on a 10G Management interface over TCP results in near line rate throughput

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 | nc 192.168.15.131 1000

0M Offset:    0GB Pkt:1622282576_055251189 Length: 240 Capture: 240 ChunkID:29484752 0.621Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_704588968 Length:9200 Capture:9200 ChunkID:29488899 8.563Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_927155222 Length:9200 Capture:9200 ChunkID:29493204 8.887Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282586_132276814 Length:9200 Capture:9200 ChunkID:29497173 8.194Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_334553100 Length:9200 Capture:9200 ChunkID:29501086 8.077Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_561683869 Length:9200 Capture:9200 ChunkID:29505479 9.064Gbps CPUIdle:0.000
0M Offset:    6GB Pkt:1622282586_785653665 Length:9200 Capture:9200 ChunkID:29509811 8.943Gbps CPUIdle:0.000
0M Offset:    7GB Pkt:1622282587_003006067 Length:9200 Capture:9200 ChunkID:29514016 8.679Gbps CPUIdle:0.000
0M Offset:    8GB Pkt:1622282587_226000777 Length:9200 Capture:9200 ChunkID:29518331 8.898Gbps CPUIdle:0.000
1M Offset:    9GB Pkt:1622282587_450201254 Length:9200 Capture:9200 ChunkID:29522667 8.950Gbps CPUIdle:0.000
1M Offset:   10GB Pkt:1622282587_682339341 Length:9200 Capture:9200 ChunkID:29527158 9.263Gbps CPUIdle:0.000
1M Offset:   11GB Pkt:1622282587_903967999 Length:9200 Capture:9200 ChunkID:29531444 8.844Gbps CPUIdle:0.000
1M Offset:   12GB Pkt:1622282588_109508894 Length:9200 Capture:9200 ChunkID:29535422 8.211Gbps CPUIdle:0.000
1M Offset:   13GB Pkt:1622282588_338390081 Length:9200 Capture:9200 ChunkID:29539849 9.139Gbps CPUIdle:0.000
1M Offset:   14GB Pkt:1622282588_557542580 Length:9200 Capture:9200 ChunkID:29544088 8.751Gbps CPUIdle:0.000
1M Offset:   15GB Pkt:1622282588_773973543 Length:9200 Capture:9200 ChunkID:29548274 8.636Gbps CPUIdle:0.000

```

### Manual Rclone local file system

local file system writes ~ 2.4Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 | rclone rcat local://mnt/store0/tmp2/blah.pcap

0M Offset:    0GB Pkt:1622282576_055251189 Length: 240 Capture: 240 ChunkID:29484752 0.637Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_557930242 Length:9200 Capture:9200 ChunkID:29486062 2.707Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_576658386 Length:9200 Capture:9200 ChunkID:29486424 0.364Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_636531964 Length:9200 Capture:13296 ChunkID:29487582 2.391Gbps CPUIdle:0.000
0M Offset:    0GB Pkt:1622282585_697875404 Length:9200 Capture:9200 ChunkID:29488769 2.446Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_757957634 Length:9200 Capture:9200 ChunkID:29489931 0.821Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_818039752 Length:9200 Capture:9200 ChunkID:29491093 2.395Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_878753413 Length:9200 Capture:9200 ChunkID:29492268 2.419Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_939467105 Length:9200 Capture:9200 ChunkID:29493442 2.421Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_964257052 Length:9200 Capture:9200 ChunkID:29493922 0.409Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282586_024760327 Length:9200 Capture:9200 ChunkID:29495092 2.409Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282586_085053021 Length:9200 Capture:9200 ChunkID:29496260 2.408Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_146261525 Length:9200 Capture:9200 ChunkID:29497444 2.444Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_164884418 Length:9200 Capture:9200 ChunkID:29497804 0.742Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_225177096 Length:9200 Capture:9200 ChunkID:29498970 2.401Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_286941475 Length:9200 Capture:9200 ChunkID:29500165 2.463Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_348284877 Length:9200 Capture:9200 ChunkID:29501352 2.448Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_408367173 Length:9200 Capture:9200 ChunkID:29502514 2.398Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_469693866 Length:9200 Capture:9200 ChunkID:29503700 2.449Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_529584009 Length:9200 Capture:9200 ChunkID:29504858 2.389Gbps CPUIdle:0.000
0M Offset:    5GB Pkt:1622282586_590508196 Length:9200 Capture:9200 ChunkID:29506037 2.428Gbps CPUIdle:0.000


```

### Manual local file system

Standard pipe to file = 8.8Gbps clearly some bottlenecks with rclone

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ sudo stream_cat -v test9k_20210529_1902 > /mnt/store0/tmp2/blah.pcap

0M Offset:    0GB Pkt:1622282576_055251189 Length: 240 Capture: 240 ChunkID:29484752 0.823Gbps CPUIdle:0.000
0M Offset:    1GB Pkt:1622282585_715451427 Length:9200 Capture:9200 ChunkID:29489109 8.997Gbps CPUIdle:0.000
0M Offset:    2GB Pkt:1622282585_944249315 Length:9200 Capture:9200 ChunkID:29493535 9.136Gbps CPUIdle:0.000
0M Offset:    3GB Pkt:1622282586_162785050 Length:9200 Capture:9200 ChunkID:29497764 8.730Gbps CPUIdle:0.000
0M Offset:    4GB Pkt:1622282586_391276769 Length:9200 Capture:9200 ChunkID:29502183 9.124Gbps CPUIdle:0.000
0M Offset:    5GB Pkt:1622282586_566815046 Length:9200 Capture:13296 ChunkID:29505578 7.009Gbps CPUIdle:0.000
0M Offset:    6GB Pkt:1622282586_787764113 Length:9200 Capture:9200 ChunkID:29509852 8.823Gbps CPUIdle:0.000
0M Offset:    7GB Pkt:1622282587_009307881 Length:9200 Capture:9200 ChunkID:29514137 8.846Gbps CPUIdle:0.000
0M Offset:    8GB Pkt:1622282587_231564003 Length:9200 Capture:9200 ChunkID:29518438 8.879Gbps CPUIdle:0.000
1M Offset:    9GB Pkt:1622282587_454907757 Length:9200 Capture:9200 ChunkID:29522758 8.918Gbps CPUIdle:0.000
1M Offset:   10GB Pkt:1622282587_663225292 Length:9200 Capture:9200 ChunkID:29526788 7.426Gbps CPUIdle:0.000
1M Offset:   11GB Pkt:1622282587_883709136 Length:9200 Capture:9200 ChunkID:29531053 8.804Gbps CPUIdle:0.000
1M Offset:   12GB Pkt:1622282588_104470038 Length:9200 Capture:9200 ChunkID:29535325 8.819Gbps CPUIdle:0.000
1M Offset:   13GB Pkt:1622282588_320895306 Length:9200 Capture:9200 ChunkID:29539511 8.642Gbps CPUIdle:0.000
1M Offset:   14GB Pkt:1622282588_544388720 Length:9200 Capture:9200 ChunkID:29543834 8.924Gbps CPUIdle:0.000

```



