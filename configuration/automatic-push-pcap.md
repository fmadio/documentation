# Automatic Push PCAP

FMADIO Packet Capture systems provide a built in Push mode to transfer capture PCAP data on a regular schedule to a remote system. An example is pushing 1minute PCAPs to a remote NFS share

Configuration is via configuration scripts located:

```text
/opt/fmadio/etc/push_realtime.lua
```

An example is shown as follows:

```lua
fmadio@fmadio20v3-287:/opt/fmadio/etc$ cat push_realtime.lua
local Config = {}

Config.Target = {}

table.insert(Config.Target, { Desc = "pcap-all", Mode = "File", Path = "/mnt/remote0/push/",   Split="--split-time 60e9", FileName="--filename-epoch-sec-startend", FilterBPF=nil })

return Config

fmadio@fmadio20v3-287:/mnt/store0/etc$
```

Multiple push targets can be specified. In the above example all PCAP data is sent to the remote NFS share mounted on /mnt/remote0. See NFS mount configuration for details on setting up /mnt/remote0 mounting points.  
Configuration options as follows  
  


**DESC**

Text field providing user information about the push target. Recommend no spaces or special characters.  


**MODE**

- File : write a file \(currently this is the only mode\)  


**PATH**

Full remote path of the target PCAPs. This include any subdirectories within the NFS mount the PCAPs are to be written to  


**SPLIT**

What kind of split mode to apply:

--split-time \(time in nanonseconds\) : the example is 1 minute \(60e9 nanonseconds\)

--split-byte \(bytes\) : the number of bytes to split by. scientific notation can be used \(e.g 1e9 for 1GB\)

**FILENAME**

Specifies how the split filename is encoded  


  
 `--filename-epoch-sec-startend : writes the sec epoch start/end time as the file name. (e.g. 1616334655-1616334755.pcap)  
 --filename-epoch-sec : writes the sec epoch start time as the file name. (e.g. 1616334655.pcap)  
 --filename-timestr-HHMM : writes the YYYYMMDD_HHMM style file name. (e.g. 2021 Dec 1st 23:50 20211201_2350.pcap)  
 --filename-timestr-HHMMSS : writes the YYYYMMDD_HHMMSS style file name. (e.g. 2021 Dec 1st 23:50:59 20211201_235059.pcap)  
 --filename-timestr-HHMMSS_NS : writes the YYYYMMDD_HHMMSS.MSEC.USEC.NSEC style file name. (e.g. 2021 Dec 1st 23:50:59 123456789nsec 20211201_235059.123.456.789.pcap)`  
  


**FILTERBPF**

Full libpcap BPF filter can be applied to reduce the total PCAP size. Example might be  
`"tcp"`

  
to write TCP only traffic. A more likely example is to exclude backup traffic from specific ip  
`"not host 192.168.1.100"`

\`\`

#### ANALYTICS CONFIGURATION

In addition to`/opt/fmadio/etc/push_realtime.lua`Analytics scheduler must be set to start the push operation. Configuration must be set as follows  
  
![](https://fmad.io/images/fmadio10-manual/20210321_analytics_push.png)  
  
Currently it only pushes the currently active capture.



