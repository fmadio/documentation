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
    FilterBPF = "net 192.168.1.0/24 and tcp" 
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

#### B\) Push all TCP data from network 192.168.1.0/24

The second example shows pushing all TCP data on the network 192.168.1.0/24 to the specified /mnt/remote0/push/ directory with a PCAP file prefix of "tcp\_\*"

Note `FilterBP=net 192.168.1.0/24 and tcp`  This applies a full BPF \(Berkley Packet Filter [https://en.wikipedia.org/wiki/Berkeley\_Packet\_Filter](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter) \) with the filter "tcp" on the packets before writing it to the location. This results in only TCP data written to the /mnt/remote0/push/tcp\_\*.pcap output files

### Supported Endpoints

| Mode                                       | Description |
| :--- | :--- |
| linux file | linux file on FMADIO capture system |
| NFS | remote NFS mountpoint on FMADIO capture system |
| SFTP | remote SSH file system via rclone \( [https://rclone.org/sftp/](https://rclone.org/sftp/) \) |
| FTP | FTP push via rclone \( [https://rclone.org/ftp/](https://rclone.org/ftp/) \) |
| S3 | S3 protocol via rclone \( [https://rclone.org/s3/](https://rclone.org/s3/) \) |
| Google Drive | Google drive via rclone \( [https://rclone.org/drive/](https://rclone.org/drive/) \) |
| Digital Ocean | Digital Ocan Spaces via rclone \( [https://rclone.org/s3/\#digitalocean-spaces](https://rclone.org/s3/#digitalocean-spaces) \) |
| Azure Blob | Microsoft Azure Blob via rclone \( [https://rclone.org/azureblob/](https://rclone.org/azureblob/) \) |
| Dropbox | Dropbox via rclone \( [https://rclone.org/dropbox/](https://rclone.org/dropbox/) \) |
| Hadoop HDFS | Hadoop file system via rclone \( [https://rclone.org/hdfs/](https://rclone.org/hdfs/) \) |
| Ceph | Ceph S3 interface via rclone \(   [https://rclone.org/s3/](https://rclone.org/s3/) \) |

and many more, see the rclone documentation for full list of endpoints supported

[https://rclone.org/docs/](https://rclone.org/docs/)

## Command Reference

Following is a description of each option for per push target.

### **DESC**

**Required**

Provides a text human readable description for each push target. It is also used for for log file description

```lua
    Desc     = "pcap-all",
```

For example the above push logfiles will go to /mnt/store0/log/push\_pcap-all\_\* this can be helpful for troubleshooting any problems

### **MODE**

**Default \(FILE\)**

Specifies how the output files are written. Currently there are 2 modes, standard linux file "File" and rclone which provides multiple end points such as FTP, S3, Google Drive, Azure Cloud and many more.

```lua
    Mode      = "FILE",
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
        <p>For RCLONE Config please see their documentation</p>
        <p><a href="https://rclone.org/commands/rclone_config/">https://rclone.org/commands/rclone_config/</a>
        </p>
        <p></p>
        <p>FMADIO by default stores config file into</p>
        <p><code>/opt/fmadio/etc/rclone.conf</code>
        </p>
        <p></p>
        <p><b>Requires FW:7157+ </b>
        </p>
      </td>
    </tr>
  </tbody>
</table>

 

### **PATH**

**Required**

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

**Required**

This specifies how to split the incoming PCAP data, either by Bytes or by Time. Following example is splitting by Time

```lua
    Split     = "--split-time 60e9",
```

| Command                                               | Description |
| :--- | :--- |
| --split-time &lt;nano seconds&gt; | Splits PCAP data by time, argument is in nanoseconds Scientific notation can be used |
| --split-byte &lt;bytes&gt; | Splits PCAP data by Size. argument is in bytes, Scientific notation can be used |

### **FILENAME**

**Required**

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
      <td style="text-align:left">--filename-tstr-HHMMSS</td>
      <td style="text-align:left">
        <p>writes the YYYYMMDD_HHMMSS style file name.</p>
        <p></p>
        <p>e.g. 2021 Dec 1st 23:50:59</p>
        <p><code>20211201_235059.pcap</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">--filename-tstr-HHMMSS_NS</td>
      <td style="text-align:left">
        <p>writes the YYYYMMDD_HHMMSS.MSEC.USEC.NSEC style file name.</p>
        <p></p>
        <p>e.g. 2021 Dec 1st 23:50:59 123456789nsec<code>20211201_235059.123.456.789.pcap</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

  


### **FILTERBPF**

**Default \(nil\)**

Full libpcap BPF filter can be applied to reduce the total PCAP size or segment specific list of PCAPs . The system uses the native libpcap library, everything that tcpdump supports FilterBPF also supports.

```lua
    FilterBPF = "net 192.168.1.0/24 and tcp" 
```

The above is an example BPF filter "net 192.168.1.0/24 and tcp" its a slightly more complicated BPF and shows the flexibility and wide range of options available. Technically there is no limit on the complexity of the BPF filter, we recommend to keep it as simple as possible to reduce the CPU load

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">FilterBPF</td>
      <td style="text-align:left">
        <p>Enter a full tcpdump equivlent BPF filter expression</p>
        <p></p>
        <p>example host filter</p>
        <p><code>FilterBPF=&quot;host 192.168.1.1&quot;</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

### DECAP

**Default \(false\)**

In addition to FilterBPF full packet de-encapsulation can be performed before the BPF filter is applied. This for example can decode VLAN, ERSPAN, GRE tunnels and many more. This enables the BPF filter is applied on the inner payload instead of the encapsulated output,

Example configuration is

```lua
Decap = true,
```

Configuration is a simple boolean type only

| Command                                         | Description |
| :--- | :--- |
| Decap | boolean value of "true" enables Packet De-encapsulation |

### PipeCmd

**Requires FW:7157+**

**Default \(nil\)**

Pipe commands are processed on a per PCAP split basis before the end transport is applied. Examples to use this are to GZIP or compress files before sending to the endpoint.

This is a generic stdin/stdout linux application, gzip, lz4 are current examples, Other options are possible, please contact us for more details

```bash
PipeCmd="gzip -1 -c"
```

The above runs gzip with compression level 1 on the split PCAP before sending to the output location. Some examples are shown below

| Command                                      | Description |
| :--- | :--- |
| gzip -c -1 | Run GZIP on split PCAPs with fastest compression mode |
| gzip -c -9 | Run GZIP on split PCAPs in maximum compression mode |
| lz4 -c | Run LZ4 compression on split PCAPs for fast compression |

### FileSuffix

**Requires FW:7157+**

**Default \(nil\)**

By default the split PCAP filename suff is `.pcap`  For most operations that is sufficient, however for more complicated operations such as GZIP compressing with PipeCmd a .pcap.gz file suffix is more appropriate. The Following is an example config target that compresses and outputs splits in .pcap.gz file format

```lua
table.insert(Config.Target, 
{ 
    Desc       = "pcap-all-gz", 
    Mode       = "rclone", 
    Path       = "s3-fmad://pcap/all", 
    Split      = "--split-time 60e9", 
    FileName   = "--filename-tstr-HHMMSS", 
    FilterBPF  = nil, 
    PipeCmd    = "gzip -c", 
    FileSuffix = ".pcap.gz" 
})
```

The above example pushes gzip 1minute PCAP splits to an S3 protocol storage device

| Command  | Description |
| :--- | :--- |
| .pcap | Default suffix. |
| .pcap.gz | GZIP Compressed PCAP |
| .pcap.lz4 | LZ4 compressed PCAP |

## Analytics Scheduler

In addition to configuration of

`/opt/fmadio/etc/push_realtime.lua`

To specify when the Push operation occurs the Analytics scheduler must be configured. This is on the "CONFIG" tab of the FMADIO GUI. An Example configuration to push files 24/7, 

The "Analytics Engine" field must be exactly the following text. 

```bash
push_realtime
```

Screenshot of 24/7 schedule is shown below

![Analytics Schedule to Push PCAP 24/7](../.gitbook/assets/image%20%2842%29.png)

## Troubleshooting

### Logfiles

Configuration problems often occour when setting up the system. The following log files can be used to debug

```bash
/mnt/store0/log/analytics_push_realtime.cur
```

Monitoring the output can be as follows

```bash
fmadio@fmadio20v3-287:/mnt/store0/log$ tail -F analytics_push_realtime.cur
[Sat May 29 15:36:46 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:37:01 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:37:16 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:37:31 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:37:46 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:38:01 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 15:38:16 2021] push [pcap-all                                 : stream_cat:true split:true]

```

In addition each Push entry has a log file with the following format. The Desc value is described 

[https://docs.fmad.io/fmadio-documentation/configuration/automatic-push-pcap\#desc](https://docs.fmad.io/fmadio-documentation/configuration/automatic-push-pcap#desc)

```bash
/mnt/store0/log/push_<Desc value>_YYYYMMDD_HHMM
```

Example output of correct functionality is as follows

```bash
fmadio@fmadio20v3-287:/mnt/store0/log$ head -n 100 push_pcap-all_20210524_1851
args
  --uid
  push_1621849863557502976
  -o
  /mnt/remote0/push/
  --split-time
  60e9
  --filename-epoch-sec-startend
--uid
UID [push_1621849863557502976]
-o
--split-time
Split Every 4768169126130614272 Sec
--filename-epoch-sec-startend
Filename EPOCH Sec Start/End
PCAP Nano
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.000 GB Speed: 0.000Gbps : New Split
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.080 GB Speed: 0.256Gbps
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.160 GB Speed: 0.418Gbps
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.240 GB Speed: 0.518Gbps
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.320 GB Speed: 0.601Gbps
[0.001 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.400 GB Speed: 0.661Gbps
[0.002 H][2021-05-24 18:51:04] /mnt/remote0/push/1621849860-1621849920.pcap : Total Bytes 0.480 GB Speed: 0.711Gbps

```

### Manual Offline mode

In addition to log files its sometimes easier to debug via the CLI interface, by manually starting the push on specific capture files. This can also be helpful to push historical PCAP files.

This is done as the following CLI command

```bash
sudo /opt/fmadio/analytics/push_realtime.lua --force --offline <capture name>
```

Replace capture name with the complete name of the capture. Also ensure push scheduler has been disabled

Example output of successful offline mode run is shown below.

```bash
fmadio@fmadio20n40v3-363:~$ sudo /opt/fmadio/analytics/push_realtime.lua  --force --offline capture1_20210521_0101
loading filename [/opt/fmadio/analytics/push_realtime.lua]
sudo /opt/fmadio/bin/stream_cat --uid push_1622272417078768896 capture1_20210521_0101  | /opt/fmadio/bin/pcap_split --uid push_1622272417078768896 -o /mnt/remote0/push/  --split-time 60e9 --filename-epoch-sec-startend > /mnt/store0/log/push_pcap-all_20210529_1613 2>&1 &
stream_cat UID [push_1622272417078768896]
stream_cat ioqueue: 4
[Sat May 29 16:13:37 2021] push [pcap-all                                 : stream_cat:true split:true]
StartChunkID: 28872404
StartChunk: 28872404 Offset: 0 Stride: 1
StartChunk: 28872404
[Sat May 29 16:13:52 2021] push [pcap-all                                 : stream_cat:true split:true]
[Sat May 29 16:14:07 2021] push [pcap-all                                 : stream_cat:true split:true]
.
.
.

```

Note the following repeated status line indicates the push is operating successfully

```bash
stream_cat:true split:true
```

For problems per push target, the logfile shown in the above command line here `/mnt/store0/log/push_pcap-all.cur` 

A good way to debug that is running tail -F /mnt/store0/log/push\_pcap-all.cur to monitor it such as the following

```bash
$ tail -F /mnt/store0/log/push_pcap-all.cur
args
  --uid
  push_1622275884783852032
  -o
  ssh://mnt/store0/tmp2/rclone/everything
  --split-time
  60e9
  --filename-tstr-HHMMSS
  --pipe-cmd
  gzip -c -1
  --rclone
  --filename-suffix
  .pcap.gz
--uid
UID [push_1622275884783852032]
-o
--split-time
Split Every 4768169126130614272 Sec
--filename-tstr-HHMMSS
Filename TimeString HHMMSS
--pipe-cmd
pipe cmd [gzip -c -1]
--rclone
Output Mode RClone
--filename-suffix
Filename Suffix [.pcap.gz]
PCAP Nano
[gzip -c -1 | rclone --config=/opt/fmadio/etc/rclone.conf rcat ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz.pending]
[0.000 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.000 GB Speed: 0.003Gbps : New Split
[0.000 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.080 GB Speed: 0.552Gbps
[0.001 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.160 GB Speed: 0.558Gbps
[0.001 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.240 GB Speed: 0.563Gbps
[0.001 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.320 GB Speed: 0.565Gbps
[0.002 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.400 GB Speed: 0.566Gbps
[0.002 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.480 GB Speed: 0.567Gbps
[0.002 H][2021-05-21 01:05:40] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.560 GB Speed: 0.568Gbps
[0.003 H][2021-05-21 01:05:41] ssh://mnt/store0/tmp2/rclone/everything_20210521_010600.pcap.gz : Total Bytes 0.640 GB Speed: 0.568Gbps
.
.
.

```



