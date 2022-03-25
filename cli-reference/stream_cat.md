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
