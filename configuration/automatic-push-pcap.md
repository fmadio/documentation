# Automatic Push PCAP

**Requires FW:6979+**

FMADIO Packet Capture systems provide a built in Push mode to transfer capture PCAP data on a regular schedule to a remote system. An example is pushing 1minute PCAPs to a remote NFS share or an S3 storage bucket.

Configuration is via configuration scripts located:

```text
/opt/fmadio/etc/push_realtime.lua
```

An example is shown as follows:

```lua
fmadio@fmadio20v3-287:/opt/fmadio/etc$ cat push_realtime.lua
local Config = {}

Config.Target = {}

-- push all pcap data to /mnt/remote0/push/all_*.pcap
table.insert(Config.Target, 
{
    Desc      = "pcap-all",
    Mode      = "File",
    Path      = "/mnt/remote0/push/all",
    Split     = "--split-time 60e9",
    FileName  = "--filename-epoch-sec-startend",
    FilterBPF = nil 
})

-- push all tcp data to /mnt/remote0/push/tcp_*.pcap
table.insert(Config.Target, 
{
    Desc      = "pcap-tcp", 
    Mode      = "File", 
    Path      = "/mnt/remote0/push/tcp",   
    Split     = "--split-time 60e9", 
    FileName  = "--filename-epoch-sec-startend", 
    FilterBPF = "tcp" 
})

return Config
```

Multiple push targets can be specified, there is no real limit however throughput does get effected.

In the above example thre are 2 push rules

#### A\) Push all packet data \(no filter\)

This push target sends all PCAP data the remote NFS share mounted on 

/mnt/remote0

See NFS mount configuration section for details on setting up /mnt/remote0 mounting points.

The sepcified is "FilterBPF=nil" meaning there is no filter, thus all traffic is pushed

#### B\) Push all TCP data 

The second example shows pushing all TCP data to the specified /mnt/remote0/push/ directory with a PCAP file prefix of "tcp\_\*"

Note FilterBP=tcp  This applies a full BPF \(Berkley Packet Filter [https://en.wikipedia.org/wiki/Berkeley\_Packet\_Filter](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter) \) with the filter "tcp" on the packets before writing it to the location. This results in only TCP data written to the /mnt/remote0/push/tcp\_\*.pcap output files

## Command Reference

Following is a description of each option for per push target.

### **DESC**

Provides a text human readable description for each push target. It is also used for for log file description

```lua
    Desc     = "pcap-all",
```

For example the above push logfiles will go to /mnt/store0/log/push\_pcap-all\_\* this can be helpful for troubleshooting any problems

### **MODE**

Specifies how the output files are written. Currently there are 2 modes, standard linux file "File" and rclone which provides multiple end points such as FTP, S3, Google Drive, Azure Cloud and many more.

```lua
    Mode      = "File",
```

#### Options

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">FILE</td>
      <td style="text-align:left">output a regular linux file. this can be ln the local file system or over
        a remote NFS mount</td>
    </tr>
    <tr>
      <td style="text-align:left">RCLONE</td>
      <td style="text-align:left">
        <p>use rclone as the end point file. Note rclone needs to be setup and configured
          before remote push is started</p>
        <p></p>
        <p><b>Requires FW:7157+ </b>
        </p>
      </td>
    </tr>
  </tbody>
</table>

 

### **PATH**

Full remote path of the target PCAPs + the leading prefix of the remote output. 

```lua
    Path      = "/mnt/remote0/push/all",
```

The above example uses the "FILE" mode, which specifies a full linux system file path. 

| Command                                               | Description |
| :--- | :--- |
| /mnt/remote0/push/all | FILE mode output PCAP files will be written for example as `/mnt/remote0/push/all_`_`20210101_010101.cap`_ |
| gdrive://pcap/all | RCLONE mode output PCAP files written be written to the rclone configured google drive endpoint into the google drive directory `/pcap/`  |

### **SPLIT**

This specifies how to split the incoming PCAP data, either by Bytes or by Time. Following example is splitting by Time

```lua
    Split     = "--split-time 60e9",
```

| Command                                               | Description |
| :--- | :--- |
| --split-time &lt;nano seconds&gt; | Splits PCAP data by time, argument is in nanoseconds Scientific notation can be used |
| --split-byte &lt;bytes&gt; | Splits PCAP data by Size. argument is in bytes, Scientific notation can be used |

### **FILENAME**

Specifies how to split filename is encoded. Different downstream applications require specific encodings. If your downstream applications need an encoding not listed, please contact us for support.

```lua
    FileName  = "--filename-epoch-sec-startend", 
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">--filename-epoch-sec-startend</td>
      <td style="text-align:left">
        <p>writes the sec epoch start/end time as the file name</p>
        <p></p>
        <p>e.g March 21, 2021 1:50:55</p>
        <p><code>1616334655-1616334755.pcap</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">--filename-epoch-sec</td>
      <td style="text-align:left">
        <p>writes the sec epoch start time as the file name.</p>
        <p></p>
        <p>e.g March 21, 2021 1:50:55</p>
        <p><code>1616334655.pcap</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">--filename-tstr-HHMM</td>
      <td style="text-align:left">
        <p>writes the YYYYMMDD_HHMM style file name.</p>
        <p></p>
        <p>e.g. 2021 Dec 1st 23:50</p>
        <p><code>20211201_2350.pcap</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">--filename-timestr-HHMMSS</td>
      <td style="text-align:left">
        <p>writes the YYYYMMDD_HHMMSS style file name.</p>
        <p></p>
        <p>e.g. 2021 Dec 1st 23:50:59</p>
        <p><code>20211201_235059.pcap</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">--filename-timestr-HHMMSS_NS</td>
      <td style="text-align:left">
        <p>writes the YYYYMMDD_HHMMSS.MSEC.USEC.NSEC style file name.</p>
        <p></p>
        <p>e.g. 2021 Dec 1st 23:50:59 123456789nsec<code>20211201_235059.123.456.789.pcap</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

  


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



