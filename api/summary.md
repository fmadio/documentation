---
description: A summary for developers to access a FMADIO device using the API.
---

# Summary

## FMADIO API

The FMADIO API is simple and designed for easy scripting integration.

**Note**: Replace the IP 127.0.0.1 with the host IP of your FMADIO device.

### Device Operation

{% swagger baseUrl="/sysmaster/capture_start?StreamName=<capture name>" path="" method="get" summary="Capture Start" %}
{% swagger-description %}
This Command starts a capture running on the device.
{% endswagger-description %}

{% swagger-parameter in="query" name="StreamName" type="string" %}
Stream capture name
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "Status": true,
  "Str": "[Fri May 28 09:48:34 2021] successfully started capture [test1234]"
}

```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/sysmaster/capture_stop" path="" method="get" summary="Capture Stop" %}
{% swagger-description %}
Stops any currently capturing process. 

\


NOTE: this does NOT stop scheduled captures.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
{
    "Status":true,
    "Str":"[Mon Jul  2 11:26:13 2018] successfully stopped capture [TestCapture]"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/sysmaster/capture_status" summary="Capture Status JSON format" %}
{% swagger-description %}
Returns status of the capture in JSON format
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "Status": true,
  "GUIMode": "full",
  "StreamName": "capturetest",
  "CaptureEnable": "false",
  "CaptureTime": 1642057937.6518,
  "CaptureByte": 9960256568,
  "CapturePacket": 36312769,
  "CaptureBps": 687557440,
  "CapturePps": 266806.4375,
  "CurrentTime": "2022/01/13   07:12"
}

```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/sysmaster/status" path="" method="get" summary="General System Status (CSV)" %}
{% swagger-description %}
Returns Capture status of currently active capture.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
$ curl -u fmadio:100g http://192.168.2.75/sysmaster/status
uptime,                                             0D 1H 57M
packets_received,                                222652795259
packets_dropped,                                            0
packets_errors,                                        300000
packets_captured,                                222265863667
bytes_captured,                                20084978997482
bytes_pending,                                              0
bytes_disk,                                    21817945751552
bytes_overflow,                                 230924484608
bytes_overflow_now,                                            0
capture0_link,                                              up
capture0_link_uptime,                                0D 1H 57M
capture0_link_speed,                                     10000
capture1_link,                                              up
capture1_link_uptime,                                0D 1H 57M
capture1_link_speed,                                     10000
capture_bytes,                                              0
capture_packets,                                            0
capture_bps,                                                0
capture_pps,                                                0
capture_name,                                     TestCapture
capture_active,                                          true
```
{% endswagger-response %}
{% endswagger %}

### Device Management

{% swagger baseUrl="/sysmaster/stats_summary" path="" method="get" summary="System Status" %}
{% swagger-description %}
Get system status information. Partial example output in parsed JSON format below

``![](<../.gitbook/assets/image (123) (1) (1).png>)``
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
{
	"uptime":"0D 7H 16M",
	"packets_received":1454363817,
	"packets_dropped":0,
	"packets_errors":0,
	"packets_captured":1454363968,
	"packets_oldest":"19 May 2014 15:48:38",
	"packets_oldest_ts":"1400482118411568128",
	"capture_days":"1505D  0H 57M",
	"bytes_captured":105800185305,
	"bytes_pending":0,
	"bytes_disk":171117117440,
	"bytes_overflow":0,
	"smart_errors":0,
	"raid_errors":0,
	"raid_status":"clean : raid5",
	"stream_errors":0,
	"chunk_errors":0,
	"ecc_errors":0,
.
.
.
.
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/pcap/del?StreamName=<full capture name>" path="" method="get" summary="Delete Capture" %}
{% swagger-description %}
Deletes capture off the system
{% endswagger-description %}

{% swagger-parameter in="path" name="StreamName" type="string" %}
full name of the capture file to be deleted
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

## V1 API

The FMADIO V1 API uses endpoints with parameters. All V1 versions of the API endpoints shall have the format:

`/api/v1/<operation>/<task>?<params=x>`

The V1 API has advantages of the previous API. These are:

* Multiple downloads can occur at the same time on the same device (up to 4 concurrently).
* Improved download performance
* Ability to use compressed and bpf filter  parameters on most downloads.

Note: All original API url's shall be available as well as the new V1 endpoints.

### Downloading PCAP

{% swagger baseUrl="/api/v1/pcap/single" path="" method="get" summary="Single PCAP Download" %}
{% swagger-description %}
Download entire capture as a single file. Piping to a file or any other analysis tools is possible.
{% endswagger-description %}

{% swagger-parameter in="query" name="Compression" type="string" %}
Compress the returned stream with gzip. 

\


'fast'   Fastest compression but not smallest.

\


'best'  Slowest compression smallest size.

\


1-9      The range from 'fast' to 'best'
{% endswagger-parameter %}

{% swagger-parameter in="query" name="StreamName" type="string" required="true" %}
Stream capture name.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" type="string" %}
BPF Filter to be applied to the stream.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterPort" %}
Port filter string. e.g  0001 &#x20;

Port 0 - Enable

Port 1 - Disable

Port 2 - Disable

Port 3 - Disable
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
$ curl -u fmadio:100g "http://192.168.2.75/api/v1/pcap/single?StreamName=TestCapture_20180702_1127&&FilterBPF=tcp" | tcpdump  -r - -nn | head
11:33:08.000000 66:77:88:99:aa:bb > 00:44:44:44:44:44 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 153a 2d03 153a 2e03 153a 2f03  ..,..:-..:...:/.
        0x0010:  153a 3003 153a 3103 153a 3203 153a 3303  .:0..:1..:2..:3.
        0x0020:  153a 3403 153a 3503 153a 3603 153a 3703  .:4..:5..:6..:7.
        0x0030:  153a a878 4e26                           .:.xN&
11:33:08.000000 66:77:88:99:aa:bb > 00:33:33:33:33:33 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 152a 2d03 152a 2e03 152a 2f03  ..,..*-..*...*/.
        0x0010:  152a 3003 152a 3103 152a 3203 152a 3303  .*0..*1..*2..*3.
        0x0020:  152a 3403 152a 3503 152a 3603 152a 3703  .*4..*5..*6..*7.
        0x0030:  152a 7b57 491d                           .*{WI.
.
.
.
.
.   
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v1/pcap/splittime" summary="Get Specific PCAP timerange (optional BPF filter)" %}
{% swagger-description %}
Gets PCAP from the specified StreamName with Start/Stop EPOCH time with an optional BPF filter
{% endswagger-description %}

{% swagger-parameter in="query" name="StreamName" required="true" %}
Capture name to fetch from
{% endswagger-parameter %}

{% swagger-parameter in="query" name="Start" required="true" %}
EPOCH Nanosecond start time
{% endswagger-parameter %}

{% swagger-parameter in="query" name="Stop" required="true" %}
EPOCH Nanosecond stop time
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" %}
Escape Encoded BPF filter
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterPort" %}
Port filter string. e.g  0001 &#x20;

Port 0 - Enable

Port 1 - Disable

Port 2 - Disable

Port 3 - Disable
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}



{% swagger baseUrl="/api/v1/pcap/timerange" path="" method="get" summary="TimeRange PCAP Download (optional BPF and Frame Filter)" %}
{% swagger-description %}
Download a timerange of pcap data that can cross over a multiple pcap files.\
The timerange results may be a portion of a single pcap stream, or a portion of multiple streams that share a connected time series.

**Examples**

****[**https://docs.fmad.io/fmadio-documentation/api/usage-guide#timerange-1**](https://docs.fmad.io/fmadio-documentation/api/usage-guide#timerange-1)****

**TSBegin** and **TSEnd**

`curl -u fmadio:xxx "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000"`

``

**TSBegin**, **TSEnd** and **FilterBPF**

`curl -u fmadio:xxx "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000" -G --data-urlencode "FilterBPF=tcp"`

``

**TSBegin**, **TSEnd**, **FilterBPF** and **Compression**

`curl -u fmadio:`**xxx** `"http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000&Compression=fast" -G --data-urlencode "FilterBPF=tcp"`
{% endswagger-description %}

{% swagger-parameter in="query" name="Compression" type="string" %}
Compress the returned stream with gzip. 

\


'fast'    Fastest compression but not smallest. 

\


'best'   Slowest compression smallest size. 

\


1-9       The range from 'fast' to 'best'
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSMax" type="integer" %}
Maximum nanosecond of packets to download.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSMode" type="string" %}
Time Range mode to use for time selection



nanos : Nanoseconds (default)



msecs : Milliseconds



sec : Secconds
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSBegin" type="integer" required="true" %}
Start time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSEnd" type="integer" required="true" %}
Stop time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterFrame" %}
Filtering based on frame parameters



capture.port=0,1

(fetch data for capture ports 0 and 1 only)



a7130.srcdevice!=0

(fetch data for anything that has a valid Arista 7130 device id)



a7130.srcdevice==0

(fetch anything that does NOT have a valid Arista 7130 device id)



a7130.srcdevice=54931 and a7130.srcport=1

(fetch data only for Arista 7130 device id 54931 and Arista 7130 port id 1)



a7130.srcdevice==54931 and a7130.srcport=1,2,3,4

(fetch data for Arista 7130 device id 54931 and Arista 7130 ports 1, 2, 3, 4)



a7130.srcdevice==54931 and a7130.srcport!=1

(fetch data for Arista 7130 device ie 54931 and all ports except port 1)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
$ curl -u fmadio:100g "http://192.168.2.75/pcap/timerange?TSBegin=1530498788000000000&TSEnd=1530498789000000000&" | tcpdump  -r - -nn | head
11:33:08.000000 66:77:88:99:aa:bb > 00:44:44:44:44:44 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 153a 2d03 153a 2e03 153a 2f03  ..,..:-..:...:/.
        0x0010:  153a 3003 153a 3103 153a 3203 153a 3303  .:0..:1..:2..:3.
        0x0020:  153a 3403 153a 3503 153a 3603 153a 3703  .:4..:5..:6..:7.
        0x0030:  153a a878 4e26                           .:.xN&
11:33:08.000000 66:77:88:99:aa:bb > 00:33:33:33:33:33 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 152a 2d03 152a 2e03 152a 2f03  ..,..*-..*...*/.
        0x0010:  152a 3003 152a 3103 152a 3203 152a 3303  .*0..*1..*2..*3.
        0x0020:  152a 3403 152a 3503 152a 3603 152a 3703  .*4..*5..*6..*7.
        0x0030:  152a 7b57 491d                           .*{WI.
.
.
.
.
.

```
{% endswagger-response %}
{% endswagger %}

### System

{% swagger method="get" path="" baseUrl="/api/v1/system/time_current" summary="Read the current FPGA System Time" %}
{% swagger-description %}
Returns the current time on the fpga in epoch nanoseconds. This time/clock is used directly to timestamp packets on the FPGA.
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
$ curl http://127.0.0.1/api/v1/system/time_current
1652273869313808661
$
```
{% endswagger-response %}
{% endswagger %}

### Legacy Download

NOTE: These interfaces are legacy, recommend using the V1 API interfaces

{% swagger baseUrl="/stream/list" path="" method="get" summary="List All Captures" %}
{% swagger-description %}
Lists all captures on the device.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
$ curl -u fmadio:100g http://192.168.2.75/stream/list
{"Path":"/capture/","StreamList":true,"List":[
    {"id":"1","Path":"TestCapture_20180702_1127","PCAP":"/pcap/single?StreamName=TestCapture_20180702_1127&","Filter":"/en.filter.html?StreamName=TestCapture_20180702_1127&","Analytics":"/en.analytics.html?StreamName=TestCapture_20180702_1127&","TCPScope":"/en.tcpscope.html?StreamName=TestCapture_20180702_1127&","Link":"/en.files.html?Fn=view&StreamName=TestCapture_20180702_1127&","Date":1.5304988337881e+18,"Size":168169046016,"Del":"/pcap/del?StreamName=TestCapture_20180702_1127&rand=1530498848939065088&","IsActive":false,"Type":"","Desc":"Mon . 11:33:53 . 02-07-2018"},
    {"id":"2","Path":"TestCapture_20180702_1118","PCAP":"/pcap/single?StreamName=TestCapture_20180702_1118&","Filter":"/en.filter.html?StreamName=TestCapture_20180702_1118&","Analytics":"/en.analytics.html?StreamName=TestCapture_20180702_1118&","TCPScope":"/en.tcpscope.html?StreamName=TestCapture_20180702_1118&","Link":"/en.files.html?Fn=view&StreamName=TestCapture_20180702_1118&","Date":1.5304978842841e+18,"Size":0,"Del":"/pcap/del?StreamName=TestCapture_20180702_1118&rand=1530498848939096064&","IsActive":false,"Type":"","Desc":"Mon . 11:18:04 . 02-07-2018"}
]}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/stream/ssize?StreamName=<capture sname>&StreamView=<split mode>" path="" method="get" summary="Split Capture by filesize" %}
{% swagger-description %}
Lists splits for a specific capture based on file size. 

\




\


Usually this is a 2 step process of 

\


1\) get the split list 

\


2\) download a specific split. 
{% endswagger-description %}

{% swagger-parameter in="query" name="StreamView" type="string" %}
Stream time slice name

\




\


split_10MB

\


split_100MB

\


split_250MB

\


split_1GB

\


split_2GB

\


split_5GB

\


split_10GB

\


split_100GB

\


split_1TB
{% endswagger-parameter %}

{% swagger-parameter in="query" name="StreamName" type="string" %}
Stream capture name
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "Path": "/capture/test1234_20210611_1258/split_1GB",
  "SplitFile": true,
  "CloudShark": false,
  "List": [
    {
      "id": "0",
      "Path": "20210611_12:58:47.332.461.824",
      "PCAP": "/pcap/splittime?StreamName=test1234_20210611_1258&Start=1623383927332461824ULL&Stop=1623383937678376671ULL&&",
      "Filter": "/en.filter.html?StreamName=test1234_20210611_1258&StartTS=1623383927332461824&StopTS=1623383937678376671&",
      "Date": 1623383927332500000,
      "Size": 1000079360,
      "PacketCnt": 12357055,
      "ValidPct": 100,
      "Type": "",
      "Desc": "Fri . 12:58:47 . 11-06-2021"
    },
    {
      "id": "1",
      "Path": "20210611_12:58:57.678.376.671",
      "PCAP": "/pcap/splittime?StreamName=test1234_20210611_1258&Start=1623383937678376671ULL&Stop=1623383938108305103ULL&&",
      "Filter": "/en.filter.html?StreamName=test1234_20210611_1258&StartTS=1623383937678376671&StopTS=1623383938108305103&",
      "Date": 1623383937678400000,
      "Size": 1000079360,
      "PacketCnt": 12497940,
      "ValidPct": 100,
      "Type": "",
      "Desc": "Fri . 12:58:57 . 11-06-2021"
    },
    .
    .
  }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/stream/stime?StreamName=<capture sname>&StreamView=<split mode>" path="" method="get" summary="Split Capture by time" %}
{% swagger-description %}
Lists splits for a specific capture based on a time unit.

\




\


Usually this is a 2 step process of 

\


1\) get the split list 

\


2\) download a specific split

\



{% endswagger-description %}

{% swagger-parameter in="query" name="StreamView" type="string" %}
Split options for the time split

\




\


split_1sec

\


split_10sec

\


split_1min

\


split_10min

\


split_15min

\


split_1hour

\


split_2hour

\


split_4hour

\


split_6hour

\


split_8hour

\


split_12hour
{% endswagger-parameter %}

{% swagger-parameter in="query" name="StreamName" type="string" %}
Stream capture name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/pcap/single?StreamName=<capture name>&FilterRE=<string>" path="" method="get" summary="Single PCAP Download" %}
{% swagger-description %}
Download entire capture as a single file. \
Piping to a file or any other analysis tools is possible.

Compression example:\
`curl -u fmadio:100g "http://192.168.2.75/pcap/single?StreamName=TestCapture_20180702_1127&Compression=fast"`

FilterBPF example:\
`curl -u fmadio:100g "http://192.168.2.75/pcap/single?StreamName=hitcon_20180702_1503_58&" -G --data-urlencode "FilterBPF=tcp"`
{% endswagger-description %}

{% swagger-parameter in="query" name="FilterRE" type="string" %}
Download the capture with using a RegEx DPI filter.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" type="string" %}
BPF Filter to be applied to the stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="Compression" type="string" %}
Compress the returned stream with gzip. 

\


'fast'      Fastest compression but not smallest

\


'best'    Slowest compression smallest size

\


1-9        The range from 'fast' to 'best' 
{% endswagger-parameter %}

{% swagger-parameter in="query" name="StreamName" type="string" %}
Stream capture name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}



{% swagger baseUrl="/pcap/splittime?StreamName=<string>&Start=<int>&Stop=<int>&FilterBPF=<string>&FilterPort=<int>" path="" method="get" summary="Split PCAP Time Download" %}
{% swagger-description %}
Download the capture with a time filter. 

\


Note: the nanosecond Epoch Start is 1530498788000000000. 

\


Removing the nanosecond part convert epoch to date/time.
{% endswagger-description %}

{% swagger-parameter in="query" name="FilterPort" type="integer" %}
Download the capture specifying the port capture number.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" type="string" %}
BPF Filter to be applied to the stream.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="StreamName" type="string" required="true" %}
Stream capture name.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="Stop" type="integer" required="true" %}
Stop time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="Start" type="integer" required="true" %}
Start time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-response status="200" description="PCAP Data stream. Usually used with tools like curl." %}
```
$ curl -u fmadio:100g "http://192.168.2.75/pcap/splittime?StreamName=TestCapture_20180702_1127&&Start=1530498788000000000&Stop=1530498789000000000&" | tcpdump  -r - -nn | head
11:33:08.000000 66:77:88:99:aa:bb > 00:44:44:44:44:44 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 153a 2d03 153a 2e03 153a 2f03  ..,..:-..:...:/.
        0x0010:  153a 3003 153a 3103 153a 3203 153a 3303  .:0..:1..:2..:3.
        0x0020:  153a 3403 153a 3503 153a 3603 153a 3703  .:4..:5..:6..:7.
        0x0030:  153a a878 4e26                           .:.xN&
11:33:08.000000 66:77:88:99:aa:bb > 00:33:33:33:33:33 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 152a 2d03 152a 2e03 152a 2f03  ..,..*-..*...*/.
        0x0010:  152a 3003 152a 3103 152a 3203 152a 3303  .*0..*1..*2..*3.
        0x0020:  152a 3403 152a 3503 152a 3603 152a 3703  .*4..*5..*6..*7.
        0x0030:  152a 7b57 491d                           .*{WI.
.
.
.
.
.
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="/pcap/timerange?TSBegin=<epoch start>&TSEnd=<epoch stop>&TSMax=<size>&TSMode=<nanos or msecs>" path="" method="get" summary="TimeRange PCAP Download" %}
{% swagger-description %}
Download a timerange of pcap data without any capture file referenced. The system will search all captures for the specified timerange.

\




\


At most it can cross two pcap files
{% endswagger-description %}

{% swagger-parameter in="query" name="TSMax" type="integer" %}
Maximum nanosecond of packets to download.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSMode" type="string" %}
Time Range mode to use for time: 

\




\


nsec    | epoch in  nano seconds (default)

\


usec    | epoch in micro seconds

\


msec   | epoch in milli seconds

\


sec:      | epoch in seconds
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSBegin" type="integer" required="true" %}
Start time in epoch (default nano seconds)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSEnd" type="integer" required="true" %}
Stop time in epoch (default nano seconds).
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

