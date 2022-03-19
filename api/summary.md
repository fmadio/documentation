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
Get system status information. Example output below

``![](<../.gitbook/assets/image (123).png>)``

`Full example output in raw JSON`

{"uptime":"1D 15H 59M","hostname":"fmadio20n40v3-364","capture\_received":4002000000,"capture\_bytes":256128000000,"capture\_dropped":0,"capture\_errors":0,"generate\_sent":0,"generate\_bytes":0,"generate\_errors":0,"bytes\_pending":160033931264,"bytes\_disk":160674349056,"bytes\_overflow":0,"smart\_errors":0,"raid\_errors":0,"stream\_errors":0,"chunk\_errors":0,"ecc\_errors":0,"packets\_oldest":"12 Jul 2021 14:06:57","packets\_oldest\_ts":"1626066417155033344","capture\_days":"249D 22H 28M","total\_capture\_byte":17679439364096,"total\_store\_byte":7.2067576967987e+16,"max\_store\_byte":11002199408640,"average\_compression":"0.00024531752569601","disk\_os0\_valid":true,"disk\_os0\_serial":"E0220226500000001952","disk\_os0\_smart":0,"disk\_os0\_temp":31,"disk\_os0\_read\_error":0,"disk\_os0\_dma\_error":0,"disk\_os0\_reallocate":0,"disk\_os0\_block\_free":99,"disk\_os0\_link\_speed":6,"disk\_os0\_wear\_level":0,"disk\_os0\_total\_write":7.634843220992,"disk\_os0\_desc":"GOOD","disk\_scr0\_valid":true,"disk\_scr0\_serial":"19512638AC99","disk\_scr0\_smart":0,"disk\_scr0\_temp":32,"disk\_scr0\_read\_error":0,"disk\_scr0\_dma\_error":0,"disk\_scr0\_reallocate":0,"disk\_scr0\_block\_free":99,"disk\_scr0\_link\_speed":6,"disk\_scr0\_wear\_level":2,"disk\_scr0\_total\_write":180.91357474867,"disk\_scr0\_desc":"GOOD","disk\_hdd0\_valid":true,"disk\_hdd0\_serial":"WD-WCC6Y2HE60HT","disk\_hdd0\_smart":0,"disk\_hdd0\_temp":22,"disk\_hdd0\_read\_error":0,"disk\_hdd0\_dma\_error":0,"disk\_hdd0\_reallocate":0,"disk\_hdd0\_block\_free":99,"disk\_hdd0\_link\_speed":6,"disk\_hdd0\_wear\_level":0,"disk\_hdd0\_total\_write":0,"disk\_hdd0\_desc":"GOOD","disk\_hdd1\_valid":true,"disk\_hdd1\_serial":"WD-WCC6Y6YAT9UT","disk\_hdd1\_smart":0,"disk\_hdd1\_temp":22,"disk\_hdd1\_read\_error":0,"disk\_hdd1\_dma\_error":0,"disk\_hdd1\_reallocate":0,"disk\_hdd1\_block\_free":99,"disk\_hdd1\_link\_speed":6,"disk\_hdd1\_wear\_level":0,"disk\_hdd1\_total\_write":0,"disk\_hdd1\_desc":"GOOD","disk\_hdd2\_valid":true,"disk\_hdd2\_serial":"WD-WCC6Y6KR53CY","disk\_hdd2\_smart":0,"disk\_hdd2\_temp":21,"disk\_hdd2\_read\_error":0,"disk\_hdd2\_dma\_error":0,"disk\_hdd2\_reallocate":0,"disk\_hdd2\_block\_free":99,"disk\_hdd2\_link\_speed":6,"disk\_hdd2\_wear\_level":0,"disk\_hdd2\_total\_write":0,"disk\_hdd2\_desc":"GOOD","disk\_hdd3\_valid":true,"disk\_hdd3\_serial":"WD-WCC6Y2HE6K1R","disk\_hdd3\_smart":0,"disk\_hdd3\_temp":22,"disk\_hdd3\_read\_error":0,"disk\_hdd3\_dma\_error":1,"disk\_hdd3\_reallocate":0,"disk\_hdd3\_block\_free":99,"disk\_hdd3\_link\_speed":6,"disk\_hdd3\_wear\_level":0,"disk\_hdd3\_total\_write":0,"disk\_hdd3\_desc":"GOOD","disk\_hdd4\_valid":true,"disk\_hdd4\_serial":"WD-WCC6Y4TD1TZS","disk\_hdd4\_smart":0,"disk\_hdd4\_temp":22,"disk\_hdd4\_read\_error":0,"disk\_hdd4\_dma\_error":0,"disk\_hdd4\_reallocate":0,"disk\_hdd4\_block\_free":99,"disk\_hdd4\_link\_speed":6,"disk\_hdd4\_wear\_level":0,"disk\_hdd4\_total\_write":0,"disk\_hdd4\_desc":"GOOD","disk\_hdd5\_valid":true,"disk\_hdd5\_serial":"WD-WCC6Y3RVDVRK","disk\_hdd5\_smart":0,"disk\_hdd5\_temp":22,"disk\_hdd5\_read\_error":0,"disk\_hdd5\_dma\_error":0,"disk\_hdd5\_reallocate":0,"disk\_hdd5\_block\_free":99,"disk\_hdd5\_link\_speed":6,"disk\_hdd5\_wear\_level":0,"disk\_hdd5\_total\_write":0,"disk\_hdd5\_desc":"GOOD","disk\_hdd6\_valid":true,"disk\_hdd6\_serial":"WD-WCC6Y4EL48JU","disk\_hdd6\_smart":0,"disk\_hdd6\_temp":21,"disk\_hdd6\_read\_error":0,"disk\_hdd6\_dma\_error":1,"disk\_hdd6\_reallocate":0,"disk\_hdd6\_block\_free":99,"disk\_hdd6\_link\_speed":6,"disk\_hdd6\_wear\_level":0,"disk\_hdd6\_total\_write":0,"disk\_hdd6\_desc":"GOOD","disk\_hdd7\_valid":true,"disk\_hdd7\_serial":"WD-WCC6Y3EV3P2U","disk\_hdd7\_smart":0,"disk\_hdd7\_temp":22,"disk\_hdd7\_read\_error":0,"disk\_hdd7\_dma\_error":0,"disk\_hdd7\_reallocate":0,"disk\_hdd7\_block\_free":99,"disk\_hdd7\_link\_speed":6,"disk\_hdd7\_wear\_level":0,"disk\_hdd7\_total\_write":0,"disk\_hdd7\_desc":"GOOD","disk\_??_valid":true,"disk_??_serial":"WD-WCC6Y5RA8P9R","disk_??_smart":0,"disk_??_temp":22,"disk_??_read\_error":0,"disk_??_dma\_error":0,"disk_??_reallocate":0,"disk_??_block\_free":99,"disk_??_link\_speed":6,"disk_??_wear\_level":0,"disk_??_total\_write":0,"disk_??_desc":"GOOD","disk_??_valid":true,"disk_??_serial":"WD-WCC6Y7YCNP0E","disk_??_smart":0,"disk_??_temp":22,"disk_??_read\_error":9,"disk_??_dma\_error":0,"disk_??_reallocate":0,"disk_??_block\_free":99,"disk_??_link\_speed":6,"disk_??_wear\_level":0,"disk_??_total\_write":0,"disk_??_desc":"GOOD","disk_??_valid":true,"disk_??_serial":"WD-WCC6Y6KV1N59","disk_??_smart":0,"disk_??_temp":22,"disk_??_read\_error":0,"disk_??_dma\_error":0,"disk_??_reallocate":0,"disk_??_block\_free":99,"disk_??_link\_speed":6,"disk_??_wear\_level":0,"disk_??_total\_write":0,"disk_??\_desc":"GOOD","disk\_par0\_valid":true,"disk\_par0\_serial":"WD-WCC6Y3RVDJPX","disk\_par0\_smart":0,"disk\_par0\_temp":22,"disk\_par0\_read\_error":0,"disk\_par0\_dma\_error":1,"disk\_par0\_reallocate":0,"disk\_par0\_block\_free":99,"disk\_par0\_link\_speed":6,"disk\_par0\_wear\_level":0,"disk\_par0\_total\_write":0,"disk\_par0\_desc":"GOOD","disk\_ssd2\_valid":true,"disk\_ssd2\_serial":"S5JXNG0N108745Y","disk\_ssd2\_smart":0,"disk\_ssd2\_temp":32,"disk\_ssd2\_read\_error":0,"disk\_ssd2\_dma\_error":0,"disk\_ssd2\_reallocate":0,"disk\_ssd2\_block\_free":99,"disk\_ssd2\_link\_speed":24,"disk\_ssd2\_wear\_level":0,"disk\_ssd2\_total\_write":0.012758904832,"disk\_ssd2\_desc":"GOOD","disk\_ssd0\_valid":true,"disk\_ssd0\_serial":"S462NF0MA04379A","disk\_ssd0\_smart":0,"disk\_ssd0\_temp":30,"disk\_ssd0\_read\_error":0,"disk\_ssd0\_dma\_error":0,"disk\_ssd0\_reallocate":0,"disk\_ssd0\_block\_free":99,"disk\_ssd0\_link\_speed":24,"disk\_ssd0\_wear\_level":0,"disk\_ssd0\_total\_write":0.012817337856,"disk\_ssd0\_desc":"GOOD","disk\_ssd1\_valid":true,"disk\_ssd1\_serial":"S462NF0MA05134B","disk\_ssd1\_smart":0,"disk\_ssd1\_temp":32,"disk\_ssd1\_read\_error":0,"disk\_ssd1\_dma\_error":0,"disk\_ssd1\_reallocate":0,"disk\_ssd1\_block\_free":99,"disk\_ssd1\_link\_speed":24,"disk\_ssd1\_wear\_level":0,"disk\_ssd1\_total\_write":0.01281732864,"disk\_ssd1\_desc":"GOOD","disk\_ssd3\_valid":true,"disk\_ssd3\_serial":"S5JXNG0N108746D","disk\_ssd3\_smart":0,"disk\_ssd3\_temp":29,"disk\_ssd3\_read\_error":0,"disk\_ssd3\_dma\_error":0,"disk\_ssd3\_reallocate":0,"disk\_ssd3\_block\_free":99,"disk\_ssd3\_link\_speed":24,"disk\_ssd3\_wear\_level":0,"disk\_ssd3\_total\_write":0.012758775296,"disk\_ssd3\_desc":"GOOD","raid\_errors":0,"raid\_status":"GOOD","eth0\_link":1,"eth0\_link\_uptime":"1D 15H 58M","eth0\_speed":1,"man0\_link":1,"man0\_link\_uptime":"1D 15H 58M","man0\_speed":10,"man0\_mode":" 10G Base-SR","man0\_wavelength":"850nm","man0\_vendor":"fmadio","man0\_vendorpn":"SFP-10GSR-85","man0\_powerrx":"0.7449 mW","man0\_powertx":"0.1585 mW","man0\_voltage":" 3.2510 V","man0\_temperature":" 32.00 C ","man0\_error\_code":0,"man0\_error\_other":0,"cap0\_link":1,"cap0\_link\_uptime":"1D 15H 58M","cap0\_speed":10,"cap0\_mode":" 10G SR","cap0\_wavelength":" 850nm","cap0\_vendor":" FS ","cap0\_vendorpn":" SFP-10GSR-85 ","cap0\_powerrx":" 14.080 mW","cap0\_powertx":" 4.710 mA","cap0\_voltage":" 3.119 V","cap0\_temperature":" 42.226 C","cap1\_link":1,"cap1\_link\_uptime":"1D 15H 58M","cap1\_speed":10,"cap1\_mode":" 10G SR","cap1\_wavelength":" 850nm","cap1\_vendor":" FS ","cap1\_vendorpn":" SFP-10GSR-85 ","cap1\_powerrx":" 11.627 mW","cap1\_powertx":" 4.764 mA","cap1\_voltage":" 3.228 V","cap1\_temperature":" 45.944 C","clock\_gm\_mac":"","clock\_gm\_sync":1,"clock\_gm\_offset\_mean":1.76,"clock\_gm\_offset\_stddev":25.006847062355,"clock\_gm\_uptime":"1D 15H 57M","clock\_sys\_sync":1,"clock\_sys\_offset\_mean":1,"clock\_sys\_offset\_stddev":34.965697476241,"clock\_sys\_uptime":"1D 15H 57M","clock\_isys\_sync":1,"clock\_isys\_offset\_mean":-1.5383714798126,"clock\_isys\_offset\_stddev":305.82805634024,"clock\_isys\_uptime":"1D 15H 57M","clock\_pps\_sync":1,"clock\_pps\_offset\_mean":0,"clock\_pps\_offset\_stddev":0,"clock\_pps\_uptime":"1D 15H 57M","clock\_ntp\_master":"","clock\_ntp\_sync":0,"clock\_ntp\_offset\_mean":0,"clock\_ntp\_offset\_stddev":0,"clock\_ntp\_uptime":"0D 0H 0M","pad":"0"}
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

### Downloading PCAP

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



{% swagger baseUrl="/api/v1/pcap/timerange" path="" method="get" summary="TimeRange PCAP Download" %}
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
Time Range mode to use for time: msecs or nanos..
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSBegin" type="integer" required="true" %}
Start time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSEnd" type="integer" required="true" %}
Stop time in nanoseconds epoch.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="FilterBPF" type="string" %}

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
