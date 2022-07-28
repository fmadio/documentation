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

### --ring \<ring path> \<bpf filter> \<cpu number>

Writes PCAP to the specified LXC \<ring path> when the \<bpf filter> matches.&#x20;

Multiple rings can be specified

NOTE: if no BPF is used \<bpf filter> needs to be ""

NOTE: If no CPU is specified, use setting of "0"

All fields must be populated.

Example:

```
sudo stream_cat --ring /opt/fmadio/queue/lxc_ring0 "net 192.168.0.0/24"  0
                --ring /opt/fmadio/queue/lxc_ring1 "net 192.168.1.0/24"  29
                --ring /opt/fmadio/queue/lxc_ring2 "net 192.168.2.0/24"  30
                --ring /opt/fmadio/queue/lxc_ring3 "net 192.168.3.0/24"  31
                my_capture_20220325_000
```

### --ring-reset

Reset the LXC Ring read/write pointers

### --ring-depth

Adjusted the size of the ring FIFO depth. This value must be a Power of 2. Maximum value is 1024&#x20;

Default value 8

### --ring-timeout \<timeout in nanoseconds>

Default: 30e9 (30 seconds)

Sets the default LXC Ring timeout value in nanoseconds.

### --delta-histo

Used in combination with capinfos2 Generates a histogram of the time between packets displaying it in a vertical histogram form.&#x20;

Default bin size is 1nsec

Default offset is 0nsec

Example output is shown below:

```
sudo stream_cat --bpf "vlan and dst 10.1.2.3" 
                --epoch-start 1651131955760021825 
                --epoch-stop 1651134322452715943  
                test_capture_20220201 
                | capinfos2 -v 
                --delta-histo  
                --delta-histo-offset 10e6    
                --delta-histo-bin 1e6     
```

![stream\_cat with delta histogram](<../.gitbook/assets/image (70) (2).png>)

Above example uses stream\_cat with an epoch and BPF filter to isolate the packet histogram deltas between packets. This is particularly useful for checking QoS SLAs

### --delta-histo-bin \<nanos>

Used with capinfos2 it specifes the width of each timebin (e.g. the histogram resolution). By default it uses 1nsec. Example usage below, this uses a 1e6 (1 millisecond) time bin with a 10msec offset.

```
sudo stream_cat --bpf "vlan and dst 10.1.2.3" 
                --epoch-start 1651131955760021825 
                --epoch-stop 1651134322452715943  
                test_capture_20220201 
                | capinfos2 -v 
                --delta-histo  
                --delta-histo-offset 10e6    
                --delta-histo-bin 1e6     
```

### --delta-histo-offset \<nanos>

As the number of timebins is limited, it may be nessecarry to offset the histogram to where the data is. The example below offsets it by 10msec with a time bin of 1msec.

```
sudo stream_cat --bpf "vlan and dst 10.1.2.3" 
                --epoch-start 1651131955760021825 
                --epoch-stop 1651134322452715943  
                test_capture_20220201 
                | capinfos2 -v 
                --delta-histo  
                --delta-histo-offset 10e6    
                --delta-histo-bin 1e6     
```

### --epoch-start \<nanosecond epoch>

Filters the specified capture using start time specified argument epoch time value.

Value of 0 means filter is disabled

NOTE: typically --epoch-start  and --epoch-stop are used together

Example: filter from epoch 1497015595000000000. This uses capinfos2 to verify the first packed (Time First) is as specified in the filter

```
fmadio@fmadio100v2-228U:~$ sudo stream_cat --epoch-start 1497015595000000000    interop17_20220430_0852  | capinfos2 -v
Epoch Start 13:39:55.000.000.000 1497015595000000000
Epoch found start Chunks:26491 Bytes 6.944 GB skipped
StartChunkID: 3737931
StartChunk: 3737931 Offset: 0 Stride: 1
StartChunk: 3737931
PCAP nano
0.00GB    0.000 Gbps    0.000 Mpps
0.40GB    3.191 Gbps    2.063 Mpps
.
.

9.22GB    3.202 Gbps    2.067 Mpps
9.60GB    3.074 Gbps    1.992 Mpps
packet stream end
20220511_131303 25.643s : SUCCESS
Total Packets: 50746286
TotalBytes     : 57674285331
TotalPackets   : 50746286
PayloadCRC     : 33ad29d7358038fd
ErrorSeq       : 0
ErrorPktSize   : 0
LastByte       : 0x00000000
SeqStart       : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
SeqEnd         : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
PacketCnt      : 0 0 0 0
TimeOrder      : 0
CRCFail        : 0
Time First     : 20170609_133955 13:39:55.000.000.000 (1497015595.000000000)
Time Last      : 20170609_133956 13:39:56.807.825.664 (1497015596.807825664)
TotalPCAPTime  : 1807825664 ns
Bandwidth      : 255.221 Gbps
Packet Rate    : 28.070 Mpps

Complete
fmadio@fmadio100v2-228U:~$

```

### --epoch-stop \<nanosecond epoch>

Filters the specified capture using and end time specified argument epoch time value.

Value of 0 means filter is disabled

NOTE: typically --epoch-start  and --epoch-stop are used together

Example: filter up to epoch time 1497015594000000000. This example uses capinfos2 to verify the last packet (Time Last) meets the specified filter value.

```
fmadio@fmadio100v2-228U:~$ sudo stream_cat --epoch-stop 1497015594000000000    interop17_20220430_0852  | capinfos2 -v
Epoch Stop 13:39:54.000.000.000 1497015594000000000
StartChunkID: 3711440
StartChunk: 3711440 Offset: 0 Stride: 1
StartChunk: 3711440
PCAP nano
0.00GB    0.000 Gbps    0.000 Mpps
0.48GB    3.804 Gbps    2.460 Mpps
0.89GB    3.269 Gbps    2.106 Mpps
1.29GB    3.270 Gbps    2.113 Mpps
TimeStop reached
20220511_131655 4.686s : SUCCESS
Total Packets: 7839777
TotalBytes     : 9003464361
TotalPackets   : 7839777
PayloadCRC     : 81ff1b0c04cdca1
ErrorSeq       : 0
ErrorPktSize   : 0
LastByte       : 0x00000000
SeqStart       : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
SeqEnd         : 0x00000000 0x00000000 0x00000000 0x00000000 : 0x00000000
PacketCnt      : 0 0 0 0
TimeOrder      : 0
CRCFail        : 0
Time First     : 20170609_133953 13:39:53.717.953.280 (1497015593.717953280)
Time Last      : 20170609_133953 13:39:53.999.999.744 (1497015593.999999744)
TotalPCAPTime  : 282046464 ns
Bandwidth      : 255.375 Gbps
Packet Rate    : 27.796 Mpps

Complete
fmadio@fmadio100v2-228U:~$

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
