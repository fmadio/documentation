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

![stream\_cat with delta histogram](<../.gitbook/assets/image (70).png>)

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
