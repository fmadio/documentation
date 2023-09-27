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

![](<../.gitbook/assets/image (123) (1) (1).png>)
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

{% swagger-parameter in="query" name="FilterFrame" %}
Filter on the Packet Frame



a7130.device=\<device id>

a7130.srcport=\<port id>

c3550.srcport=\<portid>

capture.port=\<portid>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSMode" %}
Sets the Timestamp of the PCAP



nic - FMADIO FPGA timestamp

arista7130 - Arista 7130 (Metamako)

arista7150\_overwrite - Arista 7150   FCS Overwrite

arista7150\_insert - Arista 7150 Insert 32bit

arista7280\_eth64 - Arista 7280 Ethernet 64bit header

arista7280\_mac48 - Arista 7280 SrcMAC 48bit Overwrite&#x20;

cisco\_erspan3 - Cisco ERPSANv3

cisco3550 - Cisco 3550 (Exablaze)
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

{% swagger-parameter in="query" name="FilterFrame" %}
Filter on the Packet Frame



a7130.device=\<device id>

a7130.srcport=\<port id>

c3550.srcport=\<portid>

capture.port=\<portid>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="TSMode" %}
Sets the Timestamp of the PCAP



nic - FMADIO FPGA timestamp

arista7130 - Arista 7130 (Metamako)

arista7150\_overwrite - Arista 7150   FCS Overwrite

arista7150\_insert - Arista 7150 Insert 32bit

arista7280\_eth64 - Arista 7280 Ethernet 64bit header

arista7280\_mac48 - Arista 7280 SrcMAC 48bit Overwrite&#x20;

cisco\_erspan3 - Cisco ERPSANv3

cisco3550 - Cisco 3550 (Exablaze)
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

[**https://docs.fmad.io/fmadio-documentation/api/usage-guide#timerange-1**](https://docs.fmad.io/fmadio-documentation/api/usage-guide#timerange-1)

**TSBegin** and **TSEnd**

`curl -u fmadio:xxx "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000"`



**TSBegin**, **TSEnd** and **FilterBPF**

`curl -u fmadio:xxx "http://127.0.0.1/api/v1/pcap/timerange?TSBegin=1621772572136996000&TSEnd=1621774913584264000" -G --data-urlencode "FilterBPF=tcp"`



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

{% swagger-parameter in="query" name="TSUnit" type="string" %}
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

{% swagger-parameter in="query" name="TSMode" %}
Sets the Timestamp of the PCAP



nic - FMADIO FPGA timestamp

arista7130 - Arista 7130 (Metamako)

arista7150\_overwrite - Arista 7150   FCS Overwrite

arista7150\_insert - Arista 7150 Insert 32bit

arista7280\_eth64 - Arista 7280 Ethernet 64bit header

arista7280\_mac48 - Arista 7280 SrcMAC 48bit Overwrite&#x20;

cisco\_erspan3 - Cisco ERPSANv3

cisco3550 - Cisco 3550 (Exablaze)
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

{% swagger method="get" path="" baseUrl="/api/v1/system/version" summary="Get current system version information" %}
{% swagger-description %}
Returns the current system version.

Example output:

`fmadio@fmadio100v2-228U:$ curl http://127.0.0.1/api/v1/system/version {"version":"9120","device":"fmadio100v2","build":"Wed Sep 27 03:08:27 2023"} fmadio@fmadio100v2-228U:$`
{% endswagger-description %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v1/system/port_stats" summary="Get RMON1 capture port stats " %}
{% swagger-description %}
returns RMON1 capture port statistics&#x20;

Example:

`fmadio@fmadio100v2-228U:$ curl -s http://127.0.0.1/api/v1/system/port_stats | jq`&#x20;

`{`&#x20;

`"cap0":`&#x20;

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`&#x20;

`"cap1":`&#x20;

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap2":`

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap3":`

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap4":`

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap5":`

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap6":`&#x20;

`{ "Pkt": 7237, "Byte": 1936022, "Pkt_RUNT": 0, "Pkt_64": 55, "Pkt_65_127": 559, "Pkt_128_255": 257, "Pkt_256_511": 6136, "Pkt_512_1023": 180, "Pkt_1024_1518": 50, "Pkt_1024_2047": 50, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`

`"cap7":`

`{ "Pkt": 0, "Byte": 0, "Pkt_RUNT": 0, "Pkt_64": 0, "Pkt_65_127": 0, "Pkt_128_255": 0, "Pkt_256_511": 0, "Pkt_512_1023": 0, "Pkt_1024_1518": 0, "Pkt_1024_2047": 0, "Pkt_2048_4095": 0, "Pkt_4096_8191": 0, "Pkt_8192_9216": 0, "Pkt_OVER": 0 },`&#x20;

`"port_config": "8x10G"`&#x20;

`}`

`fmadio@fmadio100v2-228U:$`


{% endswagger-description %}
{% endswagger %}

###

{% swagger method="get" path="" baseUrl="/api/v1/system_status" summary="Get system status" %}
{% swagger-description %}
Returns the system status, this is identical to the Telemetry data

[https://docs.fmad.io/fmadio-documentation/monitoring/syslog-fmadio100gv2](https://docs.fmad.io/fmadio-documentation/monitoring/syslog-fmadio100gv2)

Example output:

fmadio@fmadio100v2-228U:\~$ curl -s http://127.0.0.1/api/v1/system/status

Resulting JSON blob

{"timestamp":1695785446,"ver":"9120","temperature":{"module":"system","subsystem":"temperature","timestamp":1695785446,"ver":"9120","Temperature\_CPU0":55.00,"Temperature\_CPU1":70.00,"Temperature\_PCH":46.00,"Temperature\_SYS":42.00,"Temperature\_PER":24.00,"Temperature\_NIC":49.00,"Temperature\_AirIn":24.00,"Temperature\_AirOut":0.00,"Temperature\_Transceiver0":42.00,"Temperature\_Transceiver1":42.00},"fan":{"module":"system","subsystem":"fan","timestamp":1695785446,"ver":"9120","Fan\_SYS0":21450,"Fan\_SYS1":21450,"Fan\_SYS2":21300,"Fan\_SYS3":21450,"Fan\_SYS4":21450,"Fan\_SYS5":21450,"Fan\_SYS6":21600,"Fan\_SYS7":21450},"disk":{"module":"system","subsystem":"disk","timestamp":1695785446,"ver":"9120","FreeGB\_System":8.977,"FreeGB\_Store0":4720.779,"FreeGB\_Store1":0.000,"FreeGB\_Remote0":46349.530,"FreeGB\_Remote1":46349.530,"DiskPresent\_os0":true,"DiskTemperature\_os0":40,"DiskSMART\_os0":0,"DiskPresent\_ssd0":true,"DiskTemperature\_ssd0":36,"DiskSMART\_ssd0":0,"DiskPresent\_ssd1":true,"DiskTemperature\_ssd1":34,"DiskSMART\_ssd1":0,"DiskPresent\_ssd2":true,"DiskTemperature\_ssd2":35,"DiskSMART\_ssd2":0,"DiskPresent\_ssd3":true,"DiskTemperature\_ssd3":34,"DiskSMART\_ssd3":0,"DiskPresent\_ssd4":true,"DiskTemperature\_ssd4":35,"DiskSMART\_ssd4":0,"DiskPresent\_ssd5":true,"DiskTemperature\_ssd5":40,"DiskSMART\_ssd5":0,"DiskPresent\_ssd6":true,"DiskTemperature\_ssd6":36,"DiskSMART\_ssd6":0,"DiskPresent\_ssd7":true,"DiskTemperature\_ssd7":37,"DiskSMART\_ssd7":0,"DiskPresent\_par0":true,"DiskTemperature\_par0":34,"DiskSMART\_par0":0},"link":{"module":"system","subsystem":"link","timestamp":1695785446,"ver":"9120","cap0\_link":true,"cap1\_link":true,"cap2\_link":true,"cap3\_link":true,"cap4\_link":true,"cap5\_link":true,"cap6\_link":true,"cap7\_link":true,"man0\_link":true,"man10\_link":true},"io":{"module":"system","subsystem":"io","timestamp":1695785446,"ver":"9120","DiskRdGbps":0.40,"DiskWrGbps":0.25},"capture":{"module":"system","subsystem":"capture","timestamp":1695785446,"ver":"9120","CaptureEnb":true,"CapturePkt":10497009,"CaptureByte":14691374307,"CaptureDrop":0,"CaptureFCSError":0,"CaptureRateGbps":0.237402,"CaptureRateMpps":0.020893,"CaptureName":"wan\_colo0\_20230927\_0320","CapturePort0\_Byte":0,"CapturePort0\_Pkt":0,"CapturePort1\_Byte":14734141814,"CapturePort1\_Pkt":10527209,"CapturePort2\_Byte":0,"CapturePort2\_Pkt":0,"CapturePort3\_Byte":0,"CapturePort3\_Pkt":0,"CapturePort4\_Byte":0,"CapturePort4\_Pkt":0,"CapturePort5\_Byte":0,"CapturePort5\_Pkt":0,"CapturePort6\_Byte":0,"CapturePort6\_Pkt":0,"CapturePort7\_Byte":0,"CapturePort7\_Pkt":0},"power":{"module":"system","subsystem":"power","timestamp":1695785446,"ver":"9120","PSU0\_Status":false,"PSU1\_Status":true,"PSU\_PowerWatt":370},"other":{"module":"system","subsystem":"other","timestamp":1695785446,"ver":"9120","UptimeHour":0.33,"MemFree":352248299520,"MemErrorECC":0,"MemCached":17432817664,"MemMapped":5480136704,"MemBuffer":57147392,"MemDirty":0,"PageInByte":0,"FDCnt":1198,"WritebackB":0,"WritebackPct":0.000000,"WritebackDropTotalG":0.00,"WritebackDropG":0.00,"CacheSize":30726047137792,"StoreSize":30725994708992,"CPULoad":5.10,"SerialNo":"undef-undef-e0d55e5d2150","PortConfig":"8x10G","Version":"fmadio100v2:9120pcap2json:715"},"cat":{"module":"system","subsystem":"cat","timestamp":1695785446,"ver":"9120","cat\_0\_Enable":true,"cat\_0\_Mode":"FMADRing","cat\_0\_CPUMain":0,"cat\_0\_TSPCAP":1695785445,"cat\_0\_ReadPkt":21098,"cat\_0\_ReadByte":30071136,"cat\_0\_ReadTotalPkt":10510527,"cat\_0\_ReadTotalByte":14779806736,"cat\_0\_ReadGbps":0.231257,"cat\_0\_ReadMpps":0.020281,"cat\_0\_WritePkt":42196,"cat\_0\_WriteByte":59997961,"cat\_0\_WriteTotalPkt":10510527,"cat\_0\_WriteTotalByte":14779806736,"cat\_0\_WriteGbps":0.461405,"cat\_0\_WriteMpps":0.040563,"cat\_0\_PendingByte":30670848,"cat\_0\_PktDiscard":0,"cat\_0\_PktDiscardTotal":0,"cat\_0\_PktSlice":0,"cat\_0\_IOPriority":20,"cat\_0\_ChunkID":10996718,"cat\_0\_CmdLine":"/opt/fmadio/bin/stream\_cat--uidpush\_pcap\_1695784865262465024--follow-start--nop-truncate--ring-eof--ring/opt/fmadio/queue/pcap\_ring\_all1sec--ring-cpu/opt/fmadio/queue/pcap\_ring\_all1sec23--ring-filter-bpf/opt/fmadio/queue/pcap\_ring\_all1sec--ring-filter-frame/opt/fmadio/queue/pcap\_ring\_all1sec--ring/opt/fmadio/queue/pcap\_ring\_icmp--ring-cpu/opt/fmadio/queue/pcap\_ring\_icmp23--ring-filter-bpf/opt/fmadio/queue/pcap\_ring\_icmpicmp--ring-filter-frame/opt/fmadio/queue/pcap\_ring\_icmp","cat\_0\_StreamName":"wan\_colo0\_20230927\_0320","cat\_0\_FilterBPF":"","cat\_0\_CPUIdle":0.7622,"cat\_0\_CPUFetch":0.0154,"cat\_0\_CPUProcess":0.0154,"cat\_0\_CPUSend":0.0000,"cat\_1\_Enable":true,"cat\_1\_Mode":"FMADRing","cat\_1\_CPUMain":0,"cat\_1\_TSPCAP":1695785443,"cat\_1\_ReadPkt":199179,"cat\_1\_ReadByte":283074384,"cat\_1\_ReadTotalPkt":9743856,"cat\_1\_ReadTotalByte":13763158272,"cat\_1\_ReadGbps":0.226272,"cat\_1\_ReadMpps":0.019901,"cat\_1\_WritePkt":199179,"cat\_1\_WriteByte":281761649,"cat\_1\_WriteTotalPkt":9743856,"cat\_1\_WriteTotalByte":13763158272,"cat\_1\_WriteGbps":0.225223,"cat\_1\_WriteMpps":0.019901,"cat\_1\_PendingByte":33030144,"cat\_1\_PktDiscard":0,"cat\_1\_PktDiscardTotal":0,"cat\_1\_PktSlice":0,"cat\_1\_IOPriority":20,"cat\_1\_ChunkID":10996554,"cat\_1\_CmdLine":"/opt/fmadio/bin/stream\_cat--uidpush\_lxc\_1695784924583593984--follow--ring/opt/fmadio/queue/lxc\_market2json\_euronext\_sbe--ring-filter-bpf/opt/fmadio/queue/lxc\_market2json\_euronext\_sbevlan177andnet224.0.208.0/24--ring-filter-frame/opt/fmadio/queue/lxc\_market2json\_euronext\_sbe--cpu23-v--print-period10e9","cat\_1\_StreamName":"wan\_colo0\_20230927\_0320","cat\_1\_FilterBPF":"","cat\_1\_CPUIdle":0.9921,"cat\_1\_CPUFetch":0.0071,"cat\_1\_CPUProcess":0.0071,"cat\_1\_CPUSend":0.0000,"cat\_2\_Enable":false,"cat\_2\_Mode":"","cat\_2\_CPUMain":0,"cat\_2\_TSPCAP":0,"cat\_2\_ReadPkt":0,"cat\_2\_ReadByte":0,"cat\_2\_ReadTotalPkt":0,"cat\_2\_ReadTotalByte":0,"cat\_2\_ReadGbps":0.000000,"cat\_2\_ReadMpps":0.000000,"cat\_2\_WritePkt":0,"cat\_2\_WriteByte":0,"cat\_2\_WriteTotalPkt":0,"cat\_2\_WriteTotalByte":0,"cat\_2\_WriteGbps":0.000000,"cat\_2\_WriteMpps":0.000000,"cat\_2\_PendingByte":0,"cat\_2\_PktDiscard":0,"cat\_2\_PktDiscardTotal":0,"cat\_2\_PktSlice":0,"cat\_2\_IOPriority":0,"cat\_2\_ChunkID":0,"cat\_2\_CmdLine":"","cat\_2\_StreamName":"","cat\_2\_FilterBPF":"","cat\_2\_CPUIdle":0.0000,"cat\_2\_CPUFetch":0.0000,"cat\_2\_CPUProcess":0.0000,"cat\_2\_CPUSend":0.0000,"cat\_3\_Enable":false,"cat\_3\_Mode":"","cat\_3\_CPUMain":0,"cat\_3\_TSPCAP":0,"cat\_3\_ReadPkt":0,"cat\_3\_ReadByte":0,"cat\_3\_ReadTotalPkt":0,"cat\_3\_ReadTotalByte":0,"cat\_3\_ReadGbps":0.000000,"cat\_3\_ReadMpps":0.000000,"cat\_3\_WritePkt":0,"cat\_3\_WriteByte":0,"cat\_3\_WriteTotalPkt":0,"cat\_3\_WriteTotalByte":0,"cat\_3\_WriteGbps":0.000000,"cat\_3\_WriteMpps":0.000000,"cat\_3\_PendingByte":0,"cat\_3\_PktDiscard":0,"cat\_3\_PktDiscardTotal":0,"cat\_3\_PktSlice":0,"cat\_3\_IOPriority":0,"cat\_3\_ChunkID":0,"cat\_3\_CmdLine":"","cat\_3\_StreamName":"","cat\_3\_FilterBPF":"","cat\_3\_CPUIdle":0.0000,"cat\_3\_CPUFetch":0.0000,"cat\_3\_CPUProcess":0.0000,"cat\_3\_CPUSend":0.0000,"cat\_4\_Enable":false,"cat\_4\_Mode":"","cat\_4\_CPUMain":0,"cat\_4\_TSPCAP":0,"cat\_4\_ReadPkt":0,"cat\_4\_ReadByte":0,"cat\_4\_ReadTotalPkt":0,"cat\_4\_ReadTotalByte":0,"cat\_4\_ReadGbps":0.000000,"cat\_4\_ReadMpps":0.000000,"cat\_4\_WritePkt":0,"cat\_4\_WriteByte":0,"cat\_4\_WriteTotalPkt":0,"cat\_4\_WriteTotalByte":0,"cat\_4\_WriteGbps":0.000000,"cat\_4\_WriteMpps":0.000000,"cat\_4\_PendingByte":0,"cat\_4\_PktDiscard":0,"cat\_4\_PktDiscardTotal":0,"cat\_4\_PktSlice":0,"cat\_4\_IOPriority":0,"cat\_4\_ChunkID":0,"cat\_4\_CmdLine":"","cat\_4\_StreamName":"","cat\_4\_FilterBPF":"","cat\_4\_CPUIdle":0.0000,"cat\_4\_CPUFetch":0.0000,"cat\_4\_CPUProcess":0.0000,"cat\_4\_CPUSend":0.0000,"cat\_5\_Enable":false,"cat\_5\_Mode":"","cat\_5\_CPUMain":0,"cat\_5\_TSPCAP":0,"cat\_5\_ReadPkt":0,"cat\_5\_ReadByte":0,"cat\_5\_ReadTotalPkt":0,"cat\_5\_ReadTotalByte":0,"cat\_5\_ReadGbps":0.000000,"cat\_5\_ReadMpps":0.000000,"cat\_5\_WritePkt":0,"cat\_5\_WriteByte":0,"cat\_5\_WriteTotalPkt":0,"cat\_5\_WriteTotalByte":0,"cat\_5\_WriteGbps":0.000000,"cat\_5\_WriteMpps":0.000000,"cat\_5\_PendingByte":0,"cat\_5\_PktDiscard":0,"cat\_5\_PktDiscardTotal":0,"cat\_5\_PktSlice":0,"cat\_5\_IOPriority":0,"cat\_5\_ChunkID":0,"cat\_5\_CmdLine":"","cat\_5\_StreamName":"","cat\_5\_FilterBPF":"","cat\_5\_CPUIdle":0.0000,"cat\_5\_CPUFetch":0.0000,"cat\_5\_CPUProcess":0.0000,"cat\_5\_CPUSend":0.0000,"cat\_6\_Enable":false,"cat\_6\_Mode":"","cat\_6\_CPUMain":0,"cat\_6\_TSPCAP":0,"cat\_6\_ReadPkt":0,"cat\_6\_ReadByte":0,"cat\_6\_ReadTotalPkt":0,"cat\_6\_ReadTotalByte":0,"cat\_6\_ReadGbps":0.000000,"cat\_6\_ReadMpps":0.000000,"cat\_6\_WritePkt":0,"cat\_6\_WriteByte":0,"cat\_6\_WriteTotalPkt":0,"cat\_6\_WriteTotalByte":0,"cat\_6\_WriteGbps":0.000000,"cat\_6\_WriteMpps":0.000000,"cat\_6\_PendingByte":0,"cat\_6\_PktDiscard":0,"cat\_6\_PktDiscardTotal":0,"cat\_6\_PktSlice":0,"cat\_6\_IOPriority":0,"cat\_6\_ChunkID":0,"cat\_6\_CmdLine":"","cat\_6\_StreamName":"","cat\_6\_FilterBPF":"","cat\_6\_CPUIdle":0.0000,"cat\_6\_CPUFetch":0.0000,"cat\_6\_CPUProcess":0.0000,"cat\_6\_CPUSend":0.0000,"cat\_7\_Enable":false,"cat\_7\_Mode":"","cat\_7\_CPUMain":0,"cat\_7\_TSPCAP":0,"cat\_7\_ReadPkt":0,"cat\_7\_ReadByte":0,"cat\_7\_ReadTotalPkt":0,"cat\_7\_ReadTotalByte":0,"cat\_7\_ReadGbps":0.000000,"cat\_7\_ReadMpps":0.000000,"cat\_7\_WritePkt":0,"cat\_7\_WriteByte":0,"cat\_7\_WriteTotalPkt":0,"cat\_7\_WriteTotalByte":0,"cat\_7\_WriteGbps":0.000000,"cat\_7\_WriteMpps":0.000000,"cat\_7\_PendingByte":0,"cat\_7\_PktDiscard":0,"cat\_7\_PktDiscardTotal":0,"cat\_7\_PktSlice":0,"cat\_7\_IOPriority":0,"cat\_7\_ChunkID":0,"cat\_7\_CmdLine":"","cat\_7\_StreamName":"","cat\_7\_FilterBPF":"","cat\_7\_CPUIdle":0.0000,"cat\_7\_CPUFetch":0.0000,"cat\_7\_CPUProcess":0.0000,"cat\_7\_CPUSend":0.0000,"cat\_EnableCnt":2,"cat\_ReadPkt":220277,"cat\_ReadByte":313145520,"cat\_ReadTotalPkt":20254383,"cat\_ReadTotalByte":28542965008,"cat\_ReadGbps":0.457529,"cat\_ReadMpps":0.457529,"cat\_WritePkt":241375,"cat\_WriteByte":341759610,"cat\_WriteTotalPkt":20254383,"cat\_WriteTotalByte":28542965008,"cat\_WriteGbps":0.686627,"cat\_WriteMpps":0.686627},"ptp":{"module":"system","subsystem":"ptp","timestamp":1695785446,"ver":"9120","TimeFPGA":1695785446482977592,"TimeSYS":1695785446286598912,"GMOffset":0.00,"GMSync":false,"GMMaster":"","SysOffset":0.00,"SysSync":false,"iSysOffset":0.00,"iSysSync":true,"NTPOffset":0.00,"NTPSync":false,"NTPMaster":"","GMUpTime":0,"SysUptime":0,"iSysUptime":1061,"PPSUptime":690,"NTPUptime":0,"PPSPeriod":6.399925850,"PPSOffset":-867,"PPSdPhase":2065,"PPSdZero":-867,"PPSCnt":1225,"Clk156Period":0.000000,"Clk156Offset":0,"Clk250Period":0.000000,"Clk250Offset":0,"Clk322Period":0.000000,"Clk322Offset":0\}}

Pretty print output


{% endswagger-description %}
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

