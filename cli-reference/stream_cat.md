# stream\_cat

stream\_cat is the core utility to extract data off the system. By default it outputs a standard nanosecond PCAP to stdout. This can be piped in multiple ways per the unix philosophy

```
$ sudo stream_cat --help
stream_cat -vf (stream name)

 -v                         : verbose print of status
 --follow                   : follow mode
 --follow-start             : follow mode but start at the begining of the file
 --check-fcs                : check for FCS errors
 --force-hdd                : force reading from HDD
 --info                     : dump info on the current capture
 --cpu <cpuid>              : pin to a specific CPU
 --io-priority <level>      : sets IO priority (default 20)
 -n N                       : follow mode start N packets from the end
 --bpf "bpf expression"   : add a BPF filter expression
 --chunked                  : output chunks of packets
 --decap                    : de-encapsulate packets
 --pktslice <bytes>         : slice packets before sending down the pipe
 --time-start <HH:MM:SS>    : time start in local timezone hour:min:sec
 --time-stop <HH:MM:SS>     : time stop in local timezone hour:min:sec

 --epoch-start <epoch time> : time start in epoch nano seconds
 --epoch-stop <epoch time>  : time stop in epoch nano seconds
 --no-pcap-header           : remove pcap header from output stream

Example: 'sudo stream_cat -f -n 1000000 | tcpdump -r - -nn '
Example: 'sudo stream_cat --bpf "host 192.168.1.1" | tcpdump -r - -nn '

$
```

## Reference

CLI argument reference

### --cpu \<cpu number>

Pins stream\_cat to a specific CPU number.

### --ring \<ring path> \<bpf filter>

Writes PCAP to the specified LXC \<ring path> when the \<bpf filter> matches.&#x20;

Multiple rings can be specified

NOTE: if no BPF is used \<bpf filter> needs to be ""

Example:

```
sudo stream_cat --ring /opt/fmadio/queue/lxc_ring0 "net 192.168.0.0/24"  
                --ring /opt/fmadio/queue/lxc_ring1 "net 192.168.1.0/24"  
                --ring /opt/fmadio/queue/lxc_ring2 "net 192.168.2.0/24"  
                --ring /opt/fmadio/queue/lxc_ring3 "net 192.168.3.0/24"  
                my_capture_20220325_000
```

### --ring-reset

Reset the LXC Ring read/write pointers

### --ring-timeout \<timeout in nanoseconds>

Default: 30e9 (30 seconds)

Sets the default LXC Ring timeout value in nanoseconds.

### --delta-histo

Generates a histogram of the time between packets displaying it in a vertical histogram form.&#x20;

Default bin size is 1nsec

Default offset is 0nsec

Example output is shown below

```
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$  sudo stream_cat --bpf "vlan and dst 10.1.2.3" 
                                                           --epoch-start 1651131955760021825 
                                                           --epoch-stop 1651134322452715943   
                                                           test_capture_20220201 
                                                           | capinfos2 -v --delta-histo 
                                                           --delta-histo-offset 10e6
                                                           --delta-histo-bin 1e6
0.03GB    0.185 Gbps    0.102 Mpps
Total Packets: 114698
Delta Time Histogram
TSHisto          0 ns :     1744 (0.015) 0.000 : *******
TSHisto    1000000 ns :        7 (0.000) 0.015 : *
TSHisto    3000000 ns :        2 (0.000) 0.015 : *
TSHisto    4000000 ns :      805 (0.007) 0.015 : ***
TSHisto    5000000 ns :     1663 (0.014) 0.022 : *******
TSHisto    6000000 ns :       11 (0.000) 0.037 : *
TSHisto    7000000 ns :        2 (0.000) 0.037 : *
TSHisto    8000000 ns :       29 (0.000) 0.037 : *
TSHisto    9000000 ns :     3274 (0.029) 0.037 : ************
TSHisto   10000000 ns :    20403 (0.178) 0.066 : ***************************************************************************
TSHisto   11000000 ns :      307 (0.003) 0.244 : **
TSHisto   12000000 ns :        7 (0.000) 0.246 : *
TSHisto   13000000 ns :      175 (0.002) 0.246 : *
TSHisto   14000000 ns :    11768 (0.103) 0.248 : *******************************************
TSHisto   15000000 ns :    12553 (0.109) 0.350 : **********************************************
TSHisto   16000000 ns :      118 (0.001) 0.460 : *
TSHisto   17000000 ns :        4 (0.000) 0.461 : *
TSHisto   18000000 ns :      115 (0.001) 0.461 : *
TSHisto   19000000 ns :     7503 (0.065) 0.462 : ****************************
TSHisto   20000000 ns :    22200 (0.194) 0.527 : *********************************************************************************
TSHisto   21000000 ns :      266 (0.002) 0.721 : *
TSHisto   22000000 ns :        8 (0.000) 0.723 : *
TSHisto   23000000 ns :      150 (0.001) 0.723 : *
TSHisto   24000000 ns :     8978 (0.078) 0.725 : *********************************
TSHisto   25000000 ns :    13168 (0.115) 0.803 : *************************************************
TSHisto   26000000 ns :      143 (0.001) 0.918 : *
TSHisto   27000000 ns :        4 (0.000) 0.919 : *
TSHisto   28000000 ns :       46 (0.000) 0.919 : *
TSHisto   29000000 ns :     3015 (0.026) 0.919 : ************
TSHisto   30000000 ns :     3868 (0.034) 0.946 : ***************
TSHisto   31000000 ns :       24 (0.000) 0.979 : *
TSHisto   33000000 ns :        4 (0.000) 0.980 : *
TSHisto   34000000 ns :      411 (0.004) 0.980 : **
TSHisto   35000000 ns :      858 (0.007) 0.983 : ****
TSHisto   36000000 ns :        2 (0.000) 0.991 : *
TSHisto   38000000 ns :        1 (0.000) 0.991 : *
TSHisto   39000000 ns :      128 (0.001) 0.991 : *
TSHisto   40000000 ns :      604 (0.005) 0.992 : ***
TSHisto   41000000 ns :        2 (0.000) 0.997 : *
TSHisto   44000000 ns :       16 (0.000) 0.997 : *
TSHisto   45000000 ns :       80 (0.001) 0.997 : *
TSHisto   49000000 ns :        4 (0.000) 0.998 : *
TSHisto   50000000 ns :       53 (0.000) 0.998 : *
TSHisto   54000000 ns :        1 (0.000) 0.998 : *
TSHisto   55000000 ns :       20 (0.000) 0.998 : *
TSHisto   59000000 ns :        2 (0.000) 0.999 : *
TSHisto   60000000 ns :       14 (0.000) 0.999 : *
TSHisto   65000000 ns :        6 (0.000) 0.999 : *
TSHisto   69000000 ns :        1 (0.000) 0.999 : *
TSHisto   70000000 ns :        6 (0.000) 0.999 : *
TSHisto   75000000 ns :        6 (0.000) 0.999 : *
TSHisto   79000000 ns :        2 (0.000) 0.999 : *
TSHisto   80000000 ns :        8 (0.000) 0.999 : *
TSHisto   85000000 ns :        6 (0.000) 0.999 : *
TSHisto   90000000 ns :        9 (0.000) 0.999 : *
TSHisto   91000000 ns :        1 (0.000) 0.999 : *
TSHisto   95000000 ns :        4 (0.000) 0.999 : *
TSHisto  100000000 ns :       11 (0.000) 0.999 : *
TSHisto  105000000 ns :       17 (0.000) 0.999 : *
TSHisto  110000000 ns :       23 (0.000) 0.999 : *
TSHisto  111000000 ns :        1 (0.000) 1.000 : *
TSHisto  115000000 ns :       13 (0.000) 1.000 : *
TSHisto  116000000 ns :        1 (0.000) 1.000 : *
TSHisto  119000000 ns :        1 (0.000) 1.000 : *
TSHisto  120000000 ns :       11 (0.000) 1.000 : *
TSHisto  125000000 ns :        3 (0.000) 1.000 : *
TSHisto  130000000 ns :        2 (0.000) 1.000 : *
TSHisto -484934592 ns :        1 (0.000) 1.000 : *
TSHisto 1028065408 ns :        1 (0.000) 1.000 : *
TSHisto 1409065408 ns :        4 (0.000) 1.000 : *
DeltaMean      : 18.409467 ns
DeltaStdDev    : 70.044783 ns
TotalBytes     : 24315976
TotalPackets   : 114698
PayloadCRC     : 294607d5a42766
ErrorSeq       : 0
ErrorPktSize   : 0
LastByte       : 0x00000000
SeqStart       : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
SeqEnd         : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
PacketCnt      : 0 0 0 0
TimeOrder      : 0
CRCFail        : 0
TotalPCAPTime  : 2366668062775 ns
Bandwidth      : 0.000 Gbps
Packet Rate    : 0.000 Mpps

Complete
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$
```

## Example Usage

The following section shows how to use stream\_cat on the command line in various different ways.

Where **test\_capture** is used, replace with a stream capture name from your fmadio system.

Where **sample\_file.pcap** is used, replace with your own pcap filename.

### Whole file

To create a whole pcap of an entire fmadio system capture use the following:

```
stream_cat -v test_capture > sample_file.pcap
```

### Time Selection

To choose a selection of time for a pcap on the fmadio system the following can be used. The following example selects a time period using epoch nano seconds. 1000 nanoseconds of capture time will be extracted - assuming the stream was captured during this epoch period.

```
stream_cat -v --epoch-start 1622000000000000000 --epoch-stop 1622000000000001000 test_capture > sample_file.pcap
```

### Packet Filters

Stream\_cat can be executed with packet filtering commands. These are similar to the filter methods used by [wireshark filtering](https://www.wireshark.org/docs/man-pages/wireshark-filter.html). Example filters are also available in the [fmadio user guide](https://fmad.io/en.fmadio10-manual.html#software-capture-prefilter).

The examples here show some simple filter examples.

Stream\_cat with a IP and UDP filter:

```
stream_cat -v --bpf "ip and udp" test_capture > sample_file.pcap
```

Stream\_cat with a UDP port 80 filter:

```
stream_cat -v --bpf "udp port 80" test_capture > sample_file.pcap
```

Stream\_cat with a complex filter - select port 80 packets with tcp range selectors :

```
stream_cat -v --bpf "port 80 and tcp[((tcp[12:1] & 0xf0) >> 2):4]" test_capture > sample_file.pcap
```

### Piping

Stream\_cat is very useful for piping output to other programs to process the data. Examples are shown in the **stream\_cat --help**. The example here shows stream\_cat used with gzip to compress the output pcap into a smaller sized file.&#x20;

```
stream_cat -v -bpf "udp port 80" test_capture | gzip --fast > sample_file.pcap.gz
```

## LXC Ring

stream_cat can write directly to an lxc ring buffer located in /opt/fmadio/queue/lxc_\_ring0

It can also write to multiple lxcring buffers from a single stream\_cat instance by issuing the --ring command line multiple times.

### LXC Ring - One Ring - No Filter

This example writes all captured data to a single LXC Ring with no BPF filter applied

```
$ sudo stream_cat --ring /opt/fmadio/queue/lxc_ring0 ""  my_captuyre_20220325_000
```

NOTE: the "" arguments are required. This indicates a NULL BPF filter

### LXC Ring - One Ring - BPF Filter&#x20;

The following example writes to a single LXC Ring with a simple "tcp" BPF filter

```
$ sudo stream_cat --ring /opt/fmadio/queue/lxc_ring0 "tcp"  my_captuyre_20220325_000
```

### LXC Ring - 4 Ring - BPF Filter

Example below shows a single stream\_cat instance writing to 4 separate LXC Ring rings each with a different BPF Filter

```
$ sudo stream_cat --ring /opt/fmadio/queue/lxc_ring0 "net 192.168.0.0/24"  
                  --ring /opt/fmadio/queue/lxc_ring1 "net 192.168.1.0/24"  
                  --ring /opt/fmadio/queue/lxc_ring2 "net 192.168.2.0/24"  
                  --ring /opt/fmadio/queue/lxc_ring3 "net 192.168.3.0/24"  
                  my_captuyre_20220325_000

```

NOTE: above should be a single line
