# Automatic Push PCAP

**Requires FW:6979+**

FMADIO Packet Capture systems provide a built in Push mode to transfer capture PCAP data on a regular schedule to a remote system. An example is pushing 1minute PCAPs to a remote NFS share or an S3 storage bucket.

Configuration is via configuration scripts located:

```
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

#### A) Push all packet data (no filter)

This push target sends all PCAP data the remote NFS share mounted on&#x20;

/mnt/remote0

See NFS mount configuration section for details on setting up /mnt/remote0 mounting points.

The sepcified is "FilterBPF=nil" meaning there is no filter, thus all traffic is pushed

#### B) Push all TCP data from network 192.168.1.0/24

The second example shows pushing all TCP data on the network 192.168.1.0/24 to the specified /mnt/remote0/push/ directory with a PCAP file prefix of "tcp\_\*"

Note `FilterBP=net 192.168.1.0/24 and tcp`  This applies a full BPF (Berkley Packet Filter [https://en.wikipedia.org/wiki/Berkeley\_Packet\_Filter](https://en.wikipedia.org/wiki/Berkeley\_Packet\_Filter) ) with the filter "tcp" on the packets before writing it to the location. This results in only TCP data written to the /mnt/remote0/push/tcp\_\*.pcap output files

### Supported Endpoints

| Mode                                       | Description                                                                                                                 |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| linux file                                 | linux file on FMADIO capture system                                                                                         |
| NFS                                        | remote NFS mountpoint on FMADIO capture system                                                                              |
| SFTP                                       | remote SSH file system via rclone ( [https://rclone.org/sftp/](https://rclone.org/sftp/) )                                  |
| FTP                                        | FTP push via rclone ( [https://rclone.org/ftp/](https://rclone.org/ftp/) )                                                  |
| S3                                         | S3 protocol via rclone ( [https://rclone.org/s3/](https://rclone.org/s3/) )                                                 |
| Google Drive                               | Google drive via rclone ( [https://rclone.org/drive/](https://rclone.org/drive/) )                                          |
| Digital Ocean                              | Digital Ocan Spaces via rclone ( [https://rclone.org/s3/#digitalocean-spaces](https://rclone.org/s3/#digitalocean-spaces) ) |
| Azure Blob                                 | Microsoft Azure Blob via rclone ( [https://rclone.org/azureblob/](https://rclone.org/azureblob/) )                          |
| Dropbox                                    | Dropbox via rclone ( [https://rclone.org/dropbox/](https://rclone.org/dropbox/) )                                           |
| Hadoop HDFS                                | Hadoop file system via rclone ( [https://rclone.org/hdfs/](https://rclone.org/hdfs/) )                                      |
| Ceph                                       | Ceph S3 interface via rclone (   [https://rclone.org/s3/](https://rclone.org/s3/) )                                         |

and many more, see the rclone documentation for full list of endpoints supported

[https://rclone.org/docs/](https://rclone.org/docs/)

## Command Reference

Following is a description of each option for per push target.

### **Desc**

**Required**

Provides a text human readable description for each push target. It is also used for for log file description

```lua
    Desc     = "pcap-all",
```

For example the above push logfiles will go to /mnt/store0/log/push\_pcap-all\_\* this can be helpful for troubleshooting any problems

### **Mode**

**Default (FILE)**

Specifies how the output files are written. Currently there are 2 modes, standard linux file "File" and rclone which provides multiple end points such as FTP, S3, Google Drive, Azure Cloud and many more.

```lua
    Mode      = "FILE",
```

#### Options

| Command                                               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FILE                                                  | output a regular linux file. this can be ln the local file system or over a remote NFS mount                                                                                                                                                                                                                                                                                                                                                       |
| RCLONE                                                | <p>use rclone as the end point file. Note rclone needs to be setup and configured before remote push is started</p><p></p><p>For RCLONE Config please see their documentation</p><p><a href="https://rclone.org/commands/rclone_config/">https://rclone.org/commands/rclone_config/</a></p><p></p><p>FMADIO by default stores config file into</p><p><code>/opt/fmadio/etc/rclone.conf</code></p><p></p><p><strong>Requires FW:7157+</strong> </p> |
| LXC                                                   | <p>Writes output to the LXC ring buffer. Location of the ring buffer is the Path variable e.g</p><p></p><p>/opt/fmadio/queue/lxc_ring0</p><p></p><p><strong>Requires FW:7738+</strong></p>                                                                                                                                                                                                                                                         |
| CURL                                                  | Pipe down a CURL pipe, e.g. to FTP or HTTP POST                                                                                                                                                                                                                                                                                                                                                                                                    |

### **Path**

**Required**

Full remote path of the target PCAPs + the leading prefix of the remote output.&#x20;

```lua
    Path      = "/mnt/remote0/push/all",
```

The above example uses the "FILE" mode, which specifies a full linux system file path.&#x20;

| Command                                               | Description                                                                                                                               |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| /mnt/remote0/push/all                                 | FILE mode output PCAP files will be written for example as `/mnt/remote0/push/all_`_`20210101_010101.cap`_                                |
| gdrive://pcap/all                                     | RCLONE mode output PCAP files written be written to the rclone configured google drive endpoint into the google drive directory `/pcap/`  |

### **Split**

**Required**

This specifies how to split the incoming PCAP data, either by Bytes or by Time. Following example is splitting by Time

```lua
    Split     = "--split-time 60e9",
```

| Command                                               | Description                                                                          |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------ |
| --split-time \<nano seconds>                          | Splits PCAP data by time, argument is in nanoseconds Scientific notation can be used |
| --split-byte \<bytes>                                 | Splits PCAP data by Size. argument is in bytes, Scientific notation can be used      |

### **FileName**

**Required**

Specifies how to split filename is encoded. Different downstream applications require specific encodings. If your downstream applications need an encoding not listed, please contact us for support.

```lua
    FileName  = "--filename-epoch-sec-startend", 
```

| Command                                                | Description                                                                                                                                                                                                |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --filename-epoch-sec-startend                          | <p>writes the sec epoch start/end time as the file name  </p><p></p><p>e.g March 21, 2021 1:50:55</p><p><code>1616334655-1616334755.pcap</code></p>                                                        |
| --filename-epoch-sec                                   | <p>writes the sec epoch start time as the file name. </p><p></p><p>e.g March 21, 2021 1:50:55</p><p><code>1616334655.pcap</code></p>                                                                       |
| --filename-epoch-msec                                  | <p>Writes the epoch start time in milliseconds as the file name<br><br>e.g for April 22 02:48 GMT<br><code>fmadio_1650595712592.pcap</code></p>                                                            |
| --filename-epoch-usec                                  | <p>Writes the epoch start time in microseconds<br><br>e.g For April 22 02:48 GMT<br><code>fmadio_1650585598007301.pcap</code></p>                                                                          |
| --filename-epoch-nsec                                  | <p>Writes the epoch start time in nanoseconds<br><br>e.g For April 22 02:48 GMT<br><code>fmadio_1650585598007301462.pcap</code></p>                                                                        |
| --filename-tstr-HHMM                                   | <p>writes the YYYYMMDD_HHMM style file name.</p><p></p><p>e.g. 2021 Dec 1st 23:50 </p><p><code>20211201_2350.pcap</code></p>                                                                               |
| --filename-tstr-HHMMSS                                 | <p>writes the YYYYMMDD_HHMMSS style file name. </p><p></p><p>e.g. 2021 Dec 1st 23:50:59 </p><p><code>20211201_235059.pcap</code></p>                                                                       |
| --filename-tstr-HHMMSS\_NS                             | <p>writes the YYYYMMDD_HHMMSS.MSEC.USEC.NSEC style file name.</p><p> </p><p>e.g. 2021 Dec 1st 23:50:59 123456789nsec<code>20211201_235059.123.456.789.pcap</code></p>                                      |
| --filename-tstr-HHMMSS\_TZ                             | <p>Wrties the filename in Hour Min Sec with a local timezone suffix<br><br>e.g 2022 April 22 19:59 CST<br><code>fmadio__2022-04-21_19:59:58-04:00.pcap</code></p>                                          |
| --filename-strftime \<time string>                     | <p>Generic strftime print<br><br>e.g command line<br><code>--filename-strftime "%Y%m%d%H%M%S"</code><br><code></code><br><code>Output is as follows</code><br><code>fmadio__20220421224832.pcap</code></p> |

\


### **FilterBPF**

**Default (nil)**

Full libpcap BPF filter can be applied to reduce the total PCAP size or segment specific list of PCAPs . The system uses the native libpcap library, everything that tcpdump supports FilterBPF also supports.

```lua
    FilterBPF = "net 192.168.1.0/24 and tcp" 
```

The above is an example BPF filter "net 192.168.1.0/24 and tcp" its a slightly more complicated BPF and shows the flexibility and wide range of options available. Technically there is no limit on the complexity of the BPF filter, we recommend to keep it as simple as possible to reduce the CPU load

| Command                                    | Description                                                                                                                                   |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| FilterBPF                                  | <p>Enter a full tcpdump equivlent BPF filter expression </p><p></p><p>example host filter</p><p><code>FilterBPF="host 192.168.1.1"</code></p> |

### Decap

**Default (true)**

In addition to FilterBPF full packet de-encapsulation is performed by default before the BPF filter is applied. This for example can decode VLAN, ERSPAN, GRE tunnels and many more. It enables the BPF filter is applied on the inner payload instead of the encapsulated output,

Example to disable automatic De-encapsulation

```lua
Decap = false,
```

Configuration is a simple boolean type only

| Command                                         | Description                                                            |
| ----------------------------------------------- | ---------------------------------------------------------------------- |
| Decap                                           | boolean value of "true" enables Packet De-encapsulation (Default true) |

### PipeCmd

**Requires FW:7157+**

**Default (nil)**

Pipe commands are processed on a per PCAP split basis before the end transport is applied. Examples to use this are to GZIP or compress files before sending to the endpoint.

This is a generic stdin/stdout linux application, gzip, lz4 are current examples, Other options are possible, please contact us for more details

```bash
PipeCmd="gzip -1 -c"
```

The above runs gzip with compression level 1 on the split PCAP before sending to the output location. Some examples are shown below

| Command                                      | Description                                             |
| -------------------------------------------- | ------------------------------------------------------- |
| gzip -c -1                                   | Run GZIP on split PCAPs with fastest compression mode   |
| gzip -c -9                                   | Run GZIP on split PCAPs in maximum compression mode     |
| lz4 -c                                       | Run LZ4 compression on split PCAPs for fast compression |

### FileSuffix

**Requires FW:7157+**

**Default (nil)**

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

| Command   | Description          |
| --------- | -------------------- |
| .pcap     | Default suffix.      |
| .pcap.gz  | GZIP Compressed PCAP |
| .pcap.lz4 | LZ4 compressed PCAP  |

### Chunked

**Requires FW: 7355+**

**Default: (nil)**

Chunked mode is a more optimized processing mode. It increases the aggregate thoughput of the Push operation specifically for network traffic profiles skewed towards small packets.

By default its disabled.

Example as follows

```
Chunked = true,
```

|      | Description        |
| ---- | ------------------ |
| true | Enables chunk mode |

### FollowStart

**Requires FW: 7355+**

FollowStart forces the push to start from the beginning of the capture. If its disabled it will push from the current capture position.

Default is "false" push from the current capture position

Example as follows

```
FollowStart = true,
```

|       | Description                                         |
| ----- | --------------------------------------------------- |
| false | Push from the current capture position              |
| true  | Push from the start of the currently active capture |

### CPU

**Requires FW: 7750+**

Sets a specific CPU for stream\_cat to run on by overriding the default CPU setting. This is helpful when multiple pushes are running in parallel.

Default: the system assigned CPU number for push (typically CPU 23)

Example as follows

```
CPU = 30,
```

| Setting          | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| nil              | Uses the default system CPU for push operations                  |
| \<numeric value> | Literal Numeric value indicating which CPU to run stream\_cat on |

### Consumer Application Restart

The default behavior of the system is to constantly re-try sending data downstream without loss. In some cases its better to restart the push process and reset the start sending position, when the consumer application restarts.

An example is, if an application shuts down between 1AM and 6AM but the capture process runs 24/7. The application wants to only receive data starting at 6AM when the application starts up.

Another use case is, if the application has an error for an unspecified amount of time. The application requires real-time processing, and requires FMADIO Capture system to send data from the current time now without sending old historical data.

Adding the following configuration to the push\_realtime.lua config will cause stream\_cat to exit if the downstream consumer is unable to process data. FMADIO system scheduler will constantly restart the push process from the current time, until the consumer process starts processing data.

```
.
.
StreamCat = "--ring-timeout-exit ",
FollowStart = false,
.
.
```

NOTE: Please keep the additional white space at the end of the command.

## Analytics Scheduler

In addition to configuration of

`/opt/fmadio/etc/push_realtime.lua`

To specify when the Push operation occurs the Analytics scheduler must be configured. This is on the "CONFIG" tab of the FMADIO GUI. An Example configuration to push files 24/7,&#x20;

The "Analytics Engine" field must be exactly the following text.&#x20;

```bash
push_realtime
```

Screenshot of 24/7 schedule is shown below

![Analytics Schedule to Push PCAP 24/7](<../.gitbook/assets/image (42) (1).png>)

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

In addition each Push entry has a log file with the following format. The Desc value is described&#x20;

[https://docs.fmad.io/fmadio-documentation/configuration/automatic-push-pcap#desc](https://docs.fmad.io/fmadio-documentation/configuration/automatic-push-pcap#desc)

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

For problems per push target, the logfile shown in the above command line here `/mnt/store0/log/push_pcap-all.cur`&#x20;

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

## Performance testing

Push performance is critical and subject to multiple factors. The following provides a baseline test of different variables.

### Remote Write Performace

A first initial setp is to confirm the writing to the remote file system has sufficent bandwidth. This is simply achieved running the commands

```
time sudo dd if=/dev/zero bs=1G count=20 > /mnt/remote0/test1.bin
```

Example run

```
fmadio@fmadio20v3-287:~$ time sudo dd if=/dev/zero bs=1G count=20 > /mnt/remote0/push/test1.bin
20+0 records in
20+0 records out
21474836480 bytes (20.0GB) copied, 30.170007 seconds, 678.8MB/s
real    0m 30.17s
user    0m 0.00s
sys     0m 22.03s
fmadio@fmadio20v3-287:~$

```

The above command writes a 20GB file to the remote file location. In this case its writing @ 678MB/sec ( 5.4Gbps throughput)

## Maintenance

### Force PCAP Link Layer Setting

To force the PCAP link layer type to Ethernet use the following CLI command on the PCAP

```
printf "\x1" | dd of=<path to pcap>.pcap bs=1 seek=20 count=1 conv=notrunc
```

The symptoms of this is unusual TCPDUMP output such as the following

![](<../.gitbook/assets/image (70) (1) (1).png>)

After setting the PCAP Link Layer setting using the above command the output is as follows

![](<../.gitbook/assets/image (90) (1) (1) (1) (1) (1).png>)

Which contains the correctly decoded packets.

## Examples

Example Configuration files for refence

### Push to NFS Share with BPF Filter and 1 minute PCAPs

```
local Config = {}

Config.Target = {}

-- push all tcp data to /mnt/remote0/push/tcp_*.pcap
table.insert(Config.Target, 
{
    Desc      = "nfs-tcp", 
    Mode      = "File", 
    Path      = "/mnt/remote0/push/tcp",   
    Split     = "--split-time 60e9", 
    FileName  = "--filename-epoch-sec-startend", 
    FilterBPF = "net 192.168.1.0/24 and tcp" 
})

return Config
```

### Push to NFS Share with BPF Filter and HHMMSS Timezone

**FW: 7936+**

Example pushes a single UDP multicast group 1001 at 1 minute snapshots using an Hour Min Sec with Timezone filename.

```
local Config = {}

Config.Target = {}

-- push all multicast port 10001 data to /mnt/remote0/push/udp-10001_*.pcap
table.insert(Config.Target, 
{
    Desc      = "udp-multicast-1001", 
    Mode      = "File", 
    Path      = "/mnt/remote0/push/udp-10001",   
    Split     = "--split-time 60e9", 
    FileName  = "--filename-tstr-HHMMSS_TZ", 
    FilterBPF = "multicast and port 10001" 
})

```

### Push to NFS Share 1min Splits with BPF Filter and LZ4 compression

Example pushes 1min PCAPs with a BPF filter (port 80) and applying LZ4 compression. LZ4 compression is fast and reasonably good compression rates.

```
local Config = {}
Config.Target = {}
table.insert(Config.Target,
{
    Desc      = "port80-lz4",
    Mode      = "File",
    Path      = "/mnt/store0/tmp2/push/udp-10001",
    Split     = "--split-time 60e9",
    FileName  = "--filename-tstr-HHMMSS_TZ",
    FilterBPF = "port 80",
    PipeCmd   = "lz4 -c",
    FileSuffix   = ".pcap.lz4",
})

return Config
```

### Push to NFS share 1min Splits with BPF Filter and ZSTD compression

Example pushes 1min PCAPs with a BPF filter (port 80) and applying ZSTD compression. ZSTD is a new compression format with performance close to LZ4 but compression rates close to GZIP.

```
local Config = {}

Config.Target = {}

table.insert(Config.Target,
{
    Desc      = "port80 zstd",
    Mode      = "File",
    Path      = "/mnt/store0/tmp2/push/udp-10001",
    Split     = "--split-time 60e9",
    FileName  = "--filename-tstr-HHMMSS_TZ",
    FilterBPF = "port 80",
    PipeCmd   = "zstd -c",
    FileSuffix   = ".pcap.zstd",
})

return Config
```

### Push to NFS/CIFS Share 1GB splits

**FW: 7936+**

Example pushes the raw data to a remote NFS/CIFS (Windows Share) splitting by 1GB file size writing a gzip compressed PCAP file to the remote location.

```
local Config = {}

Config.Target = {}

-- push everything to /mnt/remote0/push/capture*.pcap.gz at 1GB splits compressed gz
table.insert(Config.Target, 
{
    Desc      = "capture", 
    Mode      = "File", 
    Path      = "/mnt/remote0/push/capture",   
    Split     = "--split-size 1e9", 
    FileName  = "--filename-tstr-HHMMSS",
    FileSuffix = "pcap.gz", 
    FilterBPF = "" 
})

return Config
```

### Push to MAGPACK over FTP

```
local Config = {}

Config.Target = {}

table.insert(Config.Target,
{
        Desc            = "magpack-all",
        Mode            = "CURL",
        Path            = "ftp://192.168.1.100/device-prefix",
        Split           = "--split-byte 1e9",
        FileName        = "--filename-tstr-HHMMSS_SUB",
        FilterBPF       = "not (host 192.168.1.105 or host 192.168.1.102)",
        Chunked         = true,
        FollowStart     = true,
        ScriptNew       = "/opt/fmadio/analytics/push_realtime_checkremote.lua 10"
})

return Config

```

### Push to an LXC 24/7

```
local Config = {}

Config.Target = {}

-- push all pcap data to lxc ring buffer 0
table.insert(Config.Target,
{
    Desc      = "lxc-all",
    Mode      = "LXC",
    Path      = "/opt/fmadio/queue/lxc_ring0",
    FilterBPF = nil
})

return Config
```

### Push to Multiple lxc\_ring 24/7

Example pushes different VLAN traffic to seperate lxc\_rings

```
local Config = {}

Config.Target = {}

-- push vlan1 data to lxc ring buffer 0
table.insert(Config.Target,
{
    Desc      = "lxc-vlan0",
    Mode      = "LXC",
    Path      = "/opt/fmadio/queue/lxc_ring0",
    FilterBPF = "vlan 1"
})

-- push vlan2 data to lxc ring buffer 1
table.insert(Config.Target,
{
    Desc      = "lxc-vlan1",
    Mode      = "LXC",
    Path      = "/opt/fmadio/queue/lxc_ring1",
    FilterBPF = "vlan 1"
})

return Config
```

### Push to AWS S3 Bucket with Compression

Pushing captured PCAP data from the local device to AWS S3 Bucket can be done using the RCLONE support.

Below is an example push\_pcap.lua config file for that

```
-- autogenerated Thu Jun  9 20:07:14 2022 from fmadio_config
local Config = {}
Config.FollowStart = true
Config.Decap       = true
Config.Target      = {}
table.insert(Config.Target,
{
    ["Desc"]        = "S3Cloud",
    ["Mode"]        = "RCLONE",
    ["Path"]        = "fmadio-s3://fmadio-pcap/pcap/fmadio20p3-coffee/full",
    ["Split"]       = "--split-time 60e9",
    ["SplitCmd"]    = "",
    ["PipeCmd"]     = " gzip -c ",
    ["FileSuffix"] = ".gz",
    ["FileName"]    = "--filename-tstr-HHMM",
    ["FilterBPF"]   = "",
    ["FilterFrame"] = "",
})
return Config
```

This uses gzip to compress the data. Also note we added a PreCapture filter to 64B Slice all traffic to AWS S3 IP address. This prevents the capture size for a run-away explosion.

Below is the resulting output in AWS S3

&#x20;

![PCAP Push to S3](<../.gitbook/assets/image (90).png>)

This does require RCLONE S3 Config to be configured before using.

## Performance

Performance of push varies by protocol filter and compression mode. Typically the remote push locations IO bandwidth is typically the bottleneck.

Here we are testing against a RAM disk on the local system. This removes the network and remote IO performance from the benchmark. Focusing entirely on the FMADIO fetch filter and compress performance.

Parallel GZIP (pigz) running on an FMADIO 100Gv2 Analytics System (96 CPUs total). Making good use of all the CPUs

<figure><img src="../.gitbook/assets/image (3) (2).png" alt=""><figcaption><p>Parallel GZIP 64 CPUs</p></figcaption></figure>

XZ compression using 64 CPUs, more utilization than Parallel GZ

<figure><img src="../.gitbook/assets/image (4) (4).png" alt=""><figcaption><p>xz compression 64 CPUs</p></figcaption></figure>

### Traffic Profile ISP

The dataset we are testing with is a WAN connection that has an ISP like L2-L7 packet distribution. As its ISP like traffic the data compression rate is not high.

| Description                              | Gbps      | Size   | Ratio    |
| ---------------------------------------- | --------- | ------ | -------- |
| raw (not compressed)                     | 6.9 Gbps  | 45.5GB | 1.0      |
| lz4                                      | 2.5Gbps   | 41.2GB | x 1.104  |
| gzip -1  (fast)                          | 0.16Gbps  | 40.5GB | x 1..123 |
| gzip default                             | 0.15Gbps  | 40.2GB | x 1.131  |
| pigz 64 CPU (Parallel GZIP with 64 CPUs) | 4.8Gbps   | 40.3GB | x 1.129  |
| pigz 32 CPU (Parallel GZIP with 32 CPUs) | 4.4Gbps   | 40.3GB | x 1.129  |
| pigz 8 CPU (Parallel GZIP with 8 CPUs)   | 1.04Gbps  | 40.3GB | x 1.129  |
| zstd default                             | 0.77Gbps  | 39.5GB | x 1.15   |
| zstd --fast                              | 2.4Gbps   | 40.3GB | x 1.129  |
| xz default                               | 0.019Gbps | 39.2GB | x 1.160  |
| xz 64 CPU (-T 64)                        | 0.909Gbps | 38.9GB | x 1.17   |
| xz 32 CPU (-T 32)                        | 0.502Gbps | 39.3GB | x 1.157  |

### Traffic Profile Finance

The data set tested is a full days worth of OPRA A+B Feed dataset, raw uncompressed data size is just under 1TB. Financial data typically gets x2 to x3 compression ratio with xz maxing out at x5.

| Description                 | Gbps      | Time  | Size  | Ratio   |
| --------------------------- | --------- | ----- | ----- | ------- |
| raw (not compressed)        | 4.29Gbps  | 0.6H  | 979GB | x 1.0   |
| lz4                         | 1.65Gbps  | 1.3H  | 372GB | x 2.629 |
| gzip -1 (fast)              | 0.341Gbps | 6.3H  | 311GB | x 3.14  |
| gzip (default)              | 0.129Gbps | 16.6H | 266GB | x 3.67  |
| pigz 64 CPU (Parallel GZIP) | 3.92Gbps  | 0.55H | 264GB | x 3.70  |
| pigz 32 CPU (Parallel GZIP) | 3.92Gbps  | 0.55H | 264GB | x 3.70  |
| pigz 16 CPU (Parallel GZIP) | 1.98Gbps  | 1.09H | 264GB | x 3.70  |
| pigz 8 CPU (Parallel GZIP)  | 0.997     | 2.1 H | 264GB | x 3.70  |
| zstd default                | 1.118Gbps | 1.9H  | 255GB | x 3.83  |
| zstd --fast                 | 1.90Gbps  | 1.13H | 294GB | x 3.32  |
| xz default                  | 0.012Gbps | 181H  | 184GB | x 5.32  |
| xz 64 CPU                   | 0.604Gbps | 3.6H  | 184GB | x 5.32  |
| xz 32 CPU                   | 0.372Gbps | 5.8H  | 184GB | x 5.32  |
| xz 16 CPU                   | 0.192Gbps | 11.3H | 184GB | x 5.32  |
| xz 8 CPU                    | 0.096Gbps | 22.6H | 184GB | x 5.32  |

