# Performance

Download performance examples using the CLI interface to extract PCAP data to downstream systems.

Please use "stream\_dump" to list all captures which can be used with

```text
sudo stream_cat -v  <capture name> |  ....
```

the output of stream\_cat is a standard PCAP via the stdout pipe

NOTE: throughput is heavily dependent on the packet size mix of the capture. Lager packet sizes get a better over all throughput

<table>
  <thead>
    <tr>
      <th style="text-align:left">Transport</th>
      <th style="text-align:left">
        <p>Physical</p>
        <p>Interface</p>
      </th>
      <th style="text-align:left">
        <p>64B</p>
        <p>Packet</p>
      </th>
      <th style="text-align:left">
        <p>9200B Jumbo</p>
        <p>Packet</p>
      </th>
      <th style="text-align:left">
        <p>IMIX</p>
        <p>Packet</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Local File System</td>
      <td style="text-align:left">local</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">8.8Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Local File System (rclone)</td>
      <td style="text-align:left">local</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">2.4Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NFS Remote</td>
      <td style="text-align:left">10G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">5.4Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">SSH Pipe</td>
      <td style="text-align:left">1G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">0.48Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">SSH Pipe</td>
      <td style="text-align:left">10G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">048Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">SSH Pipe (arcfour)</td>
      <td style="text-align:left">10G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">1.2Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NetCat TCP</td>
      <td style="text-align:left">10G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">8.8Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP localhost</td>
      <td style="text-align:left">local</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">6.2Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HTTPS localhost</td>
      <td style="text-align:left">local</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">3.3Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP remote</td>
      <td style="text-align:left">1G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">0.9Gbps</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HTTPS remote</td>
      <td style="text-align:left">10G</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">1.7Gbpbs</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

## Manual local file system

Standard pipe to file = 8.8Gbps using linux pipe to file

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

## Manual Rclone local file system

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

## Manual NFS remote filesystem \(10G Management\)

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

## Manual SSH  \(1G Management\)

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

## Manual SSH \(10G Management\)

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

## Manual SSH arcfour \(10G Management\)

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

## Manual unencrypted TCP netcat \(10G Management\)

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

### HTTP on local host

local host HTTP download ~ 6.2Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ curl -u user:pass -s  "http://localhost/pcap/single?StreamName=test9k_20210529_1902" | pipe_mon --local-bytes  > /dev/null

LocalPipe 818937856 Bytes 6534731708 Bps 00:00:01 | 0.819 GB 6534.732 Mbps | Target 0.000 Mbps
LocalPipe 1628307456 Bytes 6459537883 Bps 00:00:02 | 1.628 GB 6459.538 Mbps | Target 0.000 Mbps
LocalPipe 2438987776 Bytes 6469417812 Bps 00:00:03 | 2.439 GB 6469.418 Mbps | Target 0.000 Mbps
LocalPipe 3231449088 Bytes 6324536906 Bps 00:00:04 | 3.231 GB 6324.537 Mbps | Target 0.000 Mbps
LocalPipe 4039114752 Bytes 6444755845 Bps 00:00:05 | 4.039 GB 6444.756 Mbps | Target 0.000 Mbps
LocalPipe 4839702528 Bytes 6389507958 Bps 00:00:06 | 4.840 GB 6389.508 Mbps | Target 0.000 Mbps
LocalPipe 5641994240 Bytes 6402493926 Bps 00:00:07 | 5.642 GB 6402.494 Mbps | Target 0.000 Mbps
LocalPipe 6444417024 Bytes 6403559078 Bps 00:00:08 | 6.444 GB 6403.559 Mbps | Target 0.000 Mbps
LocalPipe 7245660160 Bytes 6392818727 Bps 00:00:09 | 7.246 GB 6392.819 Mbps | Target 0.000 Mbps
LocalPipe 8045068288 Bytes 6380023149 Bps 00:00:10 | 8.045 GB 6380.023 Mbps | Target 0.000 Mbps
LocalPipe 8848146432 Bytes 6409364455 Bps 00:00:11 | 8.848 GB 6409.364 Mbps | Target 0.000 Mbps
LocalPipe 9659613184 Bytes 6473375523 Bps 00:00:12 | 9.660 GB 6473.376 Mbps | Target 0.000 Mbps

```

### HTTPS on local host

local host HTTPS download = 3.3Gbps

```text
fmadio@fmadio20n40v3-363:/opt/fmadio/analytics$ curl -u user:pass -s -k  "https://localhost/pcap/single?StreamName=test9k_20210529_1902" | pipe_mon --local-bytes  > /dev/null

LocalPipe 605159424 Bytes 4828459859 Bps 00:00:01 | 0.605 GB 4828.460 Mbps | Target 0.000 Mbps
LocalPipe 1197211648 Bytes 4724728813 Bps 00:00:02 | 1.197 GB 4724.729 Mbps | Target 0.000 Mbps
LocalPipe 1787559936 Bytes 4710501317 Bps 00:00:03 | 1.788 GB 4710.501 Mbps | Target 0.000 Mbps
LocalPipe 2376728576 Bytes 4701632651 Bps 00:00:04 | 2.377 GB 4701.633 Mbps | Target 0.000 Mbps
LocalPipe 2967601152 Bytes 4715474849 Bps 00:00:05 | 2.968 GB 4715.475 Mbps | Target 0.000 Mbps
LocalPipe 3553230848 Bytes 4673974271 Bps 00:00:06 | 3.553 GB 4673.974 Mbps | Target 0.000 Mbps
LocalPipe 4000186368 Bytes 3567065368 Bps 00:00:07 | 4.000 GB 3567.065 Mbps | Target 0.000 Mbps
LocalPipe 4413456384 Bytes 3298046933 Bps 00:00:08 | 4.413 GB 3298.047 Mbps | Target 0.000 Mbps
LocalPipe 4827774976 Bytes 3306335798 Bps 00:00:09 | 4.828 GB 3306.336 Mbps | Target 0.000 Mbps
LocalPipe 5238030336 Bytes 3273864766 Bps 00:00:10 | 5.238 GB 3273.865 Mbps | Target 0.000 Mbps
LocalPipe 5650382848 Bytes 3290488579 Bps 00:00:11 | 5.650 GB 3290.489 Mbps | Target 0.000 Mbps
LocalPipe 6062342144 Bytes 3287665615 Bps 00:00:12 | 6.062 GB 3287.666 Mbps | Target 0.000 Mbps
LocalPipe 6475087872 Bytes 3294125805 Bps 00:00:13 | 6.475 GB 3294.126 Mbps | Target 0.000 Mbps
LocalPipe 6889275392 Bytes 3303543281 Bps 00:00:14 | 6.889 GB 3303.543 Mbps | Target 0.000 Mbps
LocalPipe 7301234688 Bytes 3287167179 Bps 00:00:15 | 7.301 GB 3287.167 Mbps | Target 0.000 Mbps
LocalPipe 7717650432 Bytes 3322749934 Bps 00:00:16 | 7.718 GB 3322.750 Mbps | Target 0.000 Mbps
LocalPipe 8135770112 Bytes 3336692453 Bps 00:00:17 | 8.136 GB 3336.692 Mbps | Target 0.000 Mbps
LocalPipe 8552710144 Bytes 3326999809 Bps 00:00:18 | 8.553 GB 3327.000 Mbps | Target 0.000 Mbps
LocalPipe 8973058048 Bytes 3354286823 Bps 00:00:19 | 8.973 GB 3354.287 Mbps | Target 0.000 Mbps
LocalPipe 9399566336 Bytes 3403930909 Bps 00:00:20 | 9.400 GB 3403.931 Mbps | Target 0.000 Mbps

```

### HTTP on remote 1G 

1G link is the bottleneck ~  0.9Gbps

```text
fmadio@fmadio100v2-228U:~$  curl -u user:pass -s -k  "https://192.168.2.225/pcap/single?StreamName=test9k_20210529_1902" | pipe_mon --local-bytes  > /dev/null
LocalPipe 166330368 Bytes 927814503 Bps 00:00:01 | 0.166 GB 927.815 Mbps | Target 0.000 Mbps
LocalPipe 333316096 Bytes 932791270 Bps 00:00:02 | 0.333 GB 932.791 Mbps | Target 0.000 Mbps
LocalPipe 500301824 Bytes 932874648 Bps 00:00:04 | 0.500 GB 932.875 Mbps | Target 0.000 Mbps
LocalPipe 665321472 Bytes 921817653 Bps 00:00:05 | 0.665 GB 921.818 Mbps | Target 0.000 Mbps
LocalPipe 831651840 Bytes 929096659 Bps 00:00:07 | 0.832 GB 929.097 Mbps | Target 0.000 Mbps
LocalPipe 996016128 Bytes 917807466 Bps 00:00:08 | 0.996 GB 917.807 Mbps | Target 0.000 Mbps
LocalPipe 1163132928 Bytes 933238679 Bps 00:00:10 | 1.163 GB 933.239 Mbps | Target 0.000 Mbps
LocalPipe 1329332224 Bytes 928205077 Bps 00:00:11 | 1.329 GB 928.205 Mbps | Target 0.000 Mbps
LocalPipe 1495138304 Bytes 926206867 Bps 00:00:12 | 1.495 GB 926.207 Mbps | Target 0.000 Mbps
LocalPipe 1660813312 Bytes 925473395 Bps 00:00:14 | 1.661 GB 925.473 Mbps | Target 0.000 Mbps
LocalPipe 1827667968 Bytes 931651862 Bps 00:00:15 | 1.828 GB 931.652 Mbps | Target 0.000 Mbps
LocalPipe 1994260480 Bytes 930385558 Bps 00:00:17 | 1.994 GB 930.386 Mbps | Target 0.000 Mbps
LocalPipe 2160852992 Bytes 930127777 Bps 00:00:18 | 2.161 GB 930.128 Mbps | Target 0.000 Mbps
LocalPipe 2327445504 Bytes 930038204 Bps 00:00:20 | 2.327 GB 930.038 Mbps | Target 0.000 Mbps
LocalPipe 2494431232 Bytes 932802994 Bps 00:00:21 | 2.494 GB 932.803 Mbps | Target 0.000 Mbps
LocalPipe 2660237312 Bytes 925971515 Bps 00:00:22 | 2.660 GB 925.972 Mbps | Target 0.000 Mbps
LocalPipe 2826436608 Bytes 928112424 Bps 00:00:24 | 2.826 GB 928.112 Mbps | Target 0.000 Mbps
LocalPipe 2992111616 Bytes 925527034 Bps 00:00:25 | 2.992 GB 925.527 Mbps | Target 0.000 Mbps

```

### HTTPS on remote 10G

10G link HTTPS download ~ 1.7Gbps

```text
fmadio@fmadio100v2-228U:~$  curl -u user:pass -s -k  "https://192.168.15.225/pcap/single?StreamName=test9k_20210529_1902" | pipe_mon --local-bytes  > /dev/null

LocalPipe 305790976 Bytes 1707039342 Bps 00:00:01 | 0.306 GB 1707.039 Mbps | Target 0.000 Mbps
LocalPipe 612237312 Bytes 1711520992 Bps 00:00:02 | 0.612 GB 1711.521 Mbps | Target 0.000 Mbps
LocalPipe 919339008 Bytes 1715025565 Bps 00:00:04 | 0.919 GB 1715.026 Mbps | Target 0.000 Mbps
LocalPipe 1227751424 Bytes 1722767466 Bps 00:00:05 | 1.228 GB 1722.767 Mbps | Target 0.000 Mbps
LocalPipe 1535246336 Bytes 1717408524 Bps 00:00:07 | 1.535 GB 1717.409 Mbps | Target 0.000 Mbps
LocalPipe 1841430528 Bytes 1709901716 Bps 00:00:08 | 1.841 GB 1709.902 Mbps | Target 0.000 Mbps
LocalPipe 2147745792 Bytes 1711122237 Bps 00:00:10 | 2.148 GB 1711.122 Mbps | Target 0.000 Mbps
LocalPipe 2455633920 Bytes 1720100662 Bps 00:00:11 | 2.456 GB 1720.101 Mbps | Target 0.000 Mbps
LocalPipe 2762866688 Bytes 1716251145 Bps 00:00:12 | 2.763 GB 1716.251 Mbps | Target 0.000 Mbps
LocalPipe 3070361600 Bytes 1717679541 Bps 00:00:14 | 3.070 GB 1717.680 Mbps | Target 0.000 Mbps
LocalPipe 3378380800 Bytes 1720775250 Bps 00:00:15 | 3.378 GB 1720.775 Mbps | Target 0.000 Mbps
LocalPipe 3684040704 Bytes 1707462492 Bps 00:00:17 | 3.684 GB 1707.462 Mbps | Target 0.000 Mbps
LocalPipe 4003463168 Bytes 1783957320 Bps 00:00:18 | 4.003 GB 1783.957 Mbps | Target 0.000 Mbps
LocalPipe 4303486976 Bytes 1675754912 Bps 00:00:20 | 4.303 GB 1675.755 Mbps | Target 0.000 Mbps
LocalPipe 4611768320 Bytes 1721719137 Bps 00:00:21 | 4.612 GB 1721.719 Mbps | Target 0.000 Mbps
LocalPipe 4916510720 Bytes 1702167210 Bps 00:00:22 | 4.917 GB 1702.167 Mbps | Target 0.000 Mbps
LocalPipe 5225971712 Bytes 1724637586 Bps 00:00:24 | 5.226 GB 1724.638 Mbps | Target 0.000 Mbps
LocalPipe 5550112768 Bytes 1810395082 Bps 00:00:25 | 5.550 GB 1810.395 Mbps | Target 0.000 Mbps

```


