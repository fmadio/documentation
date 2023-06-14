# Telemetry Syslog

FMADIO Syslog provides rich telemetry stream for monitoring the health of the system.

Telemetry is provided in JSON format for easy ingestion in monitoring and observability infrastructure. Each syslog moniting log event is described below, these are categorized into subsystems as follows

* Generic SYSLOG events
* FMADIO Temperature (Physical Thermal monitoring)
* FMADIO Time (NTP, PTPv2, PPS)
* FMADIO Other (uncategorized sensors)
* FMADIO Power (status of power supply and consumption)
* FMADIO Capture (monitoring state of the Packet Capture )
* FMADIO IO (metrics related to Disk IO)
* FMADIO Link (capture and management link status)
* FMADIO Fan (physical status of FAN and cooling)
* FMADIO Disk (status of both capture and OS disks)
* FMADIO Cat (status of the stream\_cat process used for extracting data off the disks)
* FMADIO Push PCAP (status of the PCAP Push process)
* FMADIO Push PCAP Split (event indicating each PCAP split completion)
* FMADIO Alert (Custom onbox system alerts)

All FMADIO JSON messages have the following header fields

```
{
  "module": "system",
  "subsystem": "alert",
  "timestamp": 1686730481,
  "ver": "8779",
  .
  .
}

```

### module

This provides a high level granularity on what the event/status is for, these map the sections below.

### subsystem

This provides more granular view on what kind of event/status this is for

### timestamp

This is the epoch time in seconds, where the FMADIO Monitoring system generated the event. Not the rsyslog time, although under normal situations they should be the same.

### ver

This is the Firmware Version, it allows easy backwards compatibility as the system updates and improves as data is ingested into the monitoring system.

## Generic SYSLOG

This includes generic syslog messages in free form plain text events. Example shown below

```
2023.06.14-15:48:02.323374 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session opened for user root(uid=0) by (uid=0)
2023.06.14-15:48:02.500231 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session opened for user root(uid=0) by (uid=0)
2023.06.14-15:48:02.541424 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session opened for user root(uid=0) by (uid=0)
2023.06.14-15:48:03.359497 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session closed for user root
2023.06.14-15:48:03.538123 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session closed for user root
2023.06.14-15:48:03.566477 (+08:00) | fmadio100v2-228U | authpriv.info   | sudo      | pam_unix(sudo:session): session closed for user root

```

## FMADIO Temperature

This provides thermal monitoring of the system to ensure its running within operating ranges.

```
2023.06.14-15:51:48.439475 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"temperature","timestamp":1686729108,"ver":"8779","Temperature_CPU0":39.00,"Temperature_CPU1":44.00,"Temperature_PCH":41.00,"Temperature_SYS":37.00,"Temperature_PER":22.00,"Temperature_NIC":48.00,"Temperature_AirIn":22.00,"Temperature_AirOut":0.00,"Temperature_Transceiver0":39.00,"Temperature_Transceiver1":0.00}
```

Example JSON Pretty

```json
{
  "module": "system",
  "subsystem": "temperature",
  "timestamp": 1686729129,
  "ver": "8779",
  "Temperature_CPU0": 39,
  "Temperature_CPU1": 44,
  "Temperature_PCH": 41,
  "Temperature_SYS": 37,
  "Temperature_PER": 22,
  "Temperature_NIC": 48,
  "Temperature_AirIn": 22,
  "Temperature_AirOut": 0,
  "Temperature_Transceiver0": 39,
  "Temperature_Transceiver1": 0
}

```

## FMADIO Time

Provides monitoring of time synchronization

```
2023.06.14-15:53:23.496058 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"ptp"        ,"timestamp":1686729203,"ver":"8779","TimeFPGA":1686729203472942027,"TimeSYS":1686729203473304064,"GMOffset":    0.00,"GMSync":false,"GMMaster":"","SysOffset":    0.00,"SysSync":false,"iSysOffset":    0.00,"iSysSync":true,"NTPOffset":-445000.00,"NTPSync":true,"NTPMaster":"*192.168.2.5","GMUpTime":0,"SysUptime":0,"iSysUptime":156545,"PPSUptime":155994,"NTPUptime":156094,"PPSPeriod":3.103035477,"PPSOffset":-1,"PPSdPhase":-4,"PPSdZero":-1,"PPSCnt":157188,"Clk156Period":0.000000,"Clk156Offset":0,"Clk250Period":0.000000,"Clk250Offset":0,"Clk322Period":0.000000,"Clk322Offset":0}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "ptp",
  "timestamp": 1686729331,
  "ver": "8779",
  "TimeFPGA": 1686729331198241000,
  "TimeSYS": 1686729331198588000,
  "GMOffset": 0,
  "GMSync": false,
  "GMMaster": "",
  "SysOffset": 0,
  "SysSync": false,
  "iSysOffset": 0,
  "iSysSync": true,
  "NTPOffset": -445000,
  "NTPSync": true,
  "NTPMaster": "*192.168.2.5",
  "GMUpTime": 0,
  "SysUptime": 0,
  "iSysUptime": 156675,
  "PPSUptime": 156124,
  "NTPUptime": 156224,
  "PPSPeriod": 3.103035483,
  "PPSOffset": 0,
  "PPSdPhase": -4,
  "PPSdZero": 0,
  "PPSCnt": 157316,
  "Clk156Period": 0,
  "Clk156Offset": 0,
  "Clk250Period": 0,
  "Clk250Offset": 0,
  "Clk322Period": 0,
  "Clk322Offset": 0
}
```

## FMADIO Other

Miscellaneous other fields

```
2023.06.14-15:54:27.535663 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"other"      ,"timestamp":1686729267,"ver":"8779","UptimeHour":43.52,"MemFree":366353580032,"MemErrorECC":0,"WritebackB":0,"WritebackPct":0.000000,"WritebackDropTotalG":0.00,"WritebackDropG":0.00,"CacheSize":30726047137792,"StoreSize":30725994708992,"CPULoad":  0.39,"SerialNo":"undef-undef-e0d55e5d2150","PortConfig":"2x100G","Version":"fmadio100v2:8779 pcap2json:715"}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "other",
  "timestamp": 1686729394,
  "ver": "8779",
  "UptimeHour": 43.55,
  "MemFree": 366355021824,
  "MemErrorECC": 0,
  "WritebackB": 0,
  "WritebackPct": 0,
  "WritebackDropTotalG": 0,
  "WritebackDropG": 0,
  "CacheSize": 30726047137792,
  "StoreSize": 30725994708992,
  "CPULoad": 0.39,
  "SerialNo": "undef-undef-e0d55e5d2150",
  "PortConfig": "2x100G",
  "Version": "fmadio100v2:8779 pcap2json:715"
}
```

## FMADIO Power

Provides power status and utilization information

```
2023.06.14-15:53:23.488485 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"power"      ,"timestamp":1686729203,"ver":"8779","PSU0_Status":false,"PSU1_Status":true,"PSU_PowerWatt":270}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "power",
  "timestamp": 1686729447,
  "ver": "8779",
  "PSU0_Status": false,
  "PSU1_Status": true,
  "PSU_PowerWatt": 270
}

```

## FMADIO Capture

Provides status information around the capture process

```
2023.06.14-15:56:34.457781 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"capture"    ,"timestamp":1686729394,"ver":"8779","CaptureEnb":false,"CapturePkt":      2795520,"CaptureByte":    190095360,"CaptureDrop": 952825443,"CaptureFCSError":      0,"CaptureRateGbps":            0.000000,"CaptureRateMpps":            0.000000,"CaptureName":"","CapturePort0_Byte":0,"CapturePort0_Pkt":0,"CapturePort1_Byte":68000000000,"CapturePort1_Pkt":1000000000,"CapturePort2_Byte":0,"CapturePort2_Pkt":0,"CapturePort3_Byte":0,"CapturePort3_Pkt":0,"CapturePort4_Byte":0,"CapturePort4_Pkt":0,"CapturePort5_Byte":0,"CapturePort5_Pkt":0,"CapturePort6_Byte":0,"CapturePort6_Pkt":0,"CapturePort7_Byte":0,"CapturePort7_Pkt":0}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "capture",
  "timestamp": 1686673255,
  "ver": "8779",
  "CaptureEnb": false,
  "CapturePkt": 2795520,
  "CaptureByte": 190095360,
  "CaptureDrop": 952825443,
  "CaptureFCSError": 0,
  "CaptureRateGbps": 0,
  "CaptureRateMpps": 0,
  "CaptureName": "",
  "CapturePort0_Byte": 0,
  "CapturePort0_Pkt": 0,
  "CapturePort1_Byte": 68000000000,
  "CapturePort1_Pkt": 1000000000,
  "CapturePort2_Byte": 0,
  "CapturePort2_Pkt": 0,
  "CapturePort3_Byte": 0,
  "CapturePort3_Pkt": 0,
  "CapturePort4_Byte": 0,
  "CapturePort4_Pkt": 0,
  "CapturePort5_Byte": 0,
  "CapturePort5_Pkt": 0,
  "CapturePort6_Byte": 0,
  "CapturePort6_Pkt": 0,
  "CapturePort7_Byte": 0,
  "CapturePort7_Pkt": 0
}

```

## FMADIO IO

Provides status and performance of IO disk/network related systems

```
2023.06.14-15:54:27.528914 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"io"         ,"timestamp":1686729267,"ver":"8779","DiskRdGbps":  0.00,"DiskWrGbps":  0.00}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "io",
  "timestamp": 1686729638,
  "ver": "8779",
  "DiskRdGbps": 0,
  "DiskWrGbps": 0
}
```

## FMADIO Link

Status information around port link status

```
{"module":"system","subsystem":"link"       ,"timestamp":1686729256,"ver":"8779","cap0_link":true,"cap1_link":true,"cap2_link":false,"cap3_link":false,"cap4_link":false,"cap5_link":false,"cap6_link":false,"cap7_link":false,"man0_link":true,"man10_link":true}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "link",
  "timestamp": 1686729744,
  "ver": "8779",
  "cap0_link": true,
  "cap1_link": true,
  "cap2_link": false,
  "cap3_link": false,
  "cap4_link": false,
  "cap5_link": false,
  "cap6_link": false,
  "cap7_link": false,
  "man0_link": true,
  "man10_link": true
}
```

## FMADIO Fan

Status information around fans and cooling

```
2023.06.14-15:53:02.396976 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"fan"        ,"timestamp":1686729182,"ver":"8779","Fan_SYS0":21450,"Fan_SYS1":21450,"Fan_SYS2":21300,"Fan_SYS3":21450,"Fan_SYS4":21450,"Fan_SYS5":21450,"Fan_SYS6":21450,"Fan_SYS7":21450}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "fan",
  "timestamp": 1686729819,
  "ver": "8779",
  "Fan_SYS0": 21600,
  "Fan_SYS1": 21450,
  "Fan_SYS2": 21450,
  "Fan_SYS3": 21450,
  "Fan_SYS4": 21450,
  "Fan_SYS5": 21450,
  "Fan_SYS6": 21750,
  "Fan_SYS7": 21600
}
```

## FMADIO Disk

Status around the capture and os disk

```
2023.06.14-15:59:56.186455 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"disk"       ,"timestamp":1686729596,"ver":"8779","FreeGB_System":6.078,"FreeGB_Store0":5528.059,"FreeGB_Store1":0.000,"FreeGB_Remote0":83294.126,"FreeGB_Remote1":83294.126,"DiskPresent_os0":true,"DiskTemperature_os0":37,"DiskSMART_os0":0,"DiskPresent_ssd0":true,"DiskTemperature_ssd0":31,"DiskSMART_ssd0":0,"DiskPresent_ssd1":true,"DiskTemperature_ssd1":29,"DiskSMART_ssd1":0,"DiskPresent_ssd2":true,"DiskTemperature_ssd2":30,"DiskSMART_ssd2":0,"DiskPresent_ssd3":true,"DiskTemperature_ssd3":29,"DiskSMART_ssd3":0,"DiskPresent_ssd4":true,"DiskTemperature_ssd4":30,"DiskSMART_ssd4":0,"DiskPresent_ssd5":true,"DiskTemperature_ssd5":31,"DiskSMART_ssd5":0,"DiskPresent_ssd6":true,"DiskTemperature_ssd6":31,"DiskSMART_ssd6":0,"DiskPresent_ssd7":true,"DiskTemperature_ssd7":32,"DiskSMART_ssd7":0,"DiskPresent_par0":true,"DiskTemperature_par0":30,"DiskSMART_par0":0}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "disk",
  "timestamp": 1686729925,
  "ver": "8779",
  "FreeGB_System": 6.078,
  "FreeGB_Store0": 5528.059,
  "FreeGB_Store1": 0,
  "FreeGB_Remote0": 83294.126,
  "FreeGB_Remote1": 83294.126,
  "DiskPresent_os0": true,
  "DiskTemperature_os0": 37,
  "DiskSMART_os0": 0,
  "DiskPresent_ssd0": true,
  "DiskTemperature_ssd0": 31,
  "DiskSMART_ssd0": 0,
  "DiskPresent_ssd1": true,
  "DiskTemperature_ssd1": 29,
  "DiskSMART_ssd1": 0,
  "DiskPresent_ssd2": true,
  "DiskTemperature_ssd2": 30,
  "DiskSMART_ssd2": 0,
  "DiskPresent_ssd3": true,
  "DiskTemperature_ssd3": 29,
  "DiskSMART_ssd3": 0,
  "DiskPresent_ssd4": true,
  "DiskTemperature_ssd4": 30,
  "DiskSMART_ssd4": 0,
  "DiskPresent_ssd5": true,
  "DiskTemperature_ssd5": 31,
  "DiskSMART_ssd5": 0,
  "DiskPresent_ssd6": true,
  "DiskTemperature_ssd6": 31,
  "DiskSMART_ssd6": 0,
  "DiskPresent_ssd7": true,
  "DiskTemperature_ssd7": 32,
  "DiskSMART_ssd7": 0,
  "DiskPresent_par0": true,
  "DiskTemperature_par0": 30,
  "DiskSMART_par0": 0
}
```

## FMADIO Cat

stream\_cat is the core FMADIO utility for extracting packets off the storage system. Its used heavily for download, realtime processing and troubleshooting. This provides statistics and state of the currently running stream\_cat instances.

Up to 8 simultaniousle stream\_cat instances can be run at the same time. cat\_\*\_ is per instance.

```
2023.06.14-16:06:28.566556 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"cat"        ,"timestamp":1686729988,"ver":"8779","cat_0_Enable":false,"cat_0_Mode":"","cat_0_CPUMain":0,"cat_0_TSPCAP":0,"cat_0_ReadPkt":0,"cat_0_ReadByte":0,"cat_0_ReadTotalPkt":0,"cat_0_ReadTotalByte":0,"cat_0_ReadGbps":0.000000,"cat_0_ReadMpps":0.000000,"cat_0_WritePkt":0,"cat_0_WriteByte":0,"cat_0_WriteTotalPkt":0,"cat_0_WriteTotalByte":0,"cat_0_WriteGbps":0.000000,"cat_0_WriteMpps":0.000000,"cat_0_PendingByte":0,"cat_0_PktDiscard":0,"cat_0_PktDiscardTotal":0,"cat_0_PktSlice":0,"cat_0_IOPriority":0,"cat_0_ChunkID":0,"cat_0_CmdLine":"","cat_0_StreamName":"","cat_0_FilterBPF":"","cat_0_CPUIdle":0.0000,"cat_0_CPUFetch":0.0000,"cat_0_CPUProcess":0.0000,"cat_0_CPUSend":0.0000,"cat_1_Enable":false,"cat_1_Mode":"","cat_1_CPUMain":0,"cat_1_TSPCAP":0,"cat_1_ReadPkt":0,"cat_1_ReadByte":0,"cat_1_ReadTotalPkt":0,"cat_1_ReadTotalByte":0,"cat_1_ReadGbps":0.000000,"cat_1_ReadMpps":0.000000,"cat_1_WritePkt":0,"cat_1_WriteByte":0,"cat_1_WriteTotalPkt":0,"cat_1_WriteTotalByte":0,"cat_1_WriteGbps":0.000000,"cat_1_WriteMpps":0.000000,"cat_1_PendingByte":0,"cat_1_PktDiscard":0,"cat_1_PktDiscardTotal":0,"cat_1_PktSlice":0,"cat_1_IOPriority":0,"cat_1_ChunkID":0,"cat_1_CmdLine":"","cat_1_StreamName":"","cat_1_FilterBPF":"","cat_1_CPUIdle":0.0000,"cat_1_CPUFetch":0.0000,"cat_1_CPUProcess":0.0000,"cat_1_CPUSend":0.0000,"cat_2_Enable":false,"cat_2_Mode":"","cat_2_CPUMain":0,"cat_2_TSPCAP":0,"cat_2_ReadPkt":0,"cat_2_ReadByte":0,"cat_2_ReadTotalPkt":0,"cat_2_ReadTotalByte":0,"cat_2_ReadGbps":0.000000,"cat_2_ReadMpps":0.000000,"cat_2_WritePkt":0,"cat_2_WriteByte":0,"cat_2_WriteTotalPkt":0,"cat_2_WriteTotalByte":0,"cat_2_WriteGbps":0.000000,"cat_2_WriteMpps":0.000000,"cat_2_PendingByte":0,"cat_2_PktDiscard":0,"cat_2_PktDiscardTotal":0,"cat_2_PktSlice":0,"cat_2_IOPriority":0,"cat_2_ChunkID":0,"cat_2_CmdLine":"","cat_2_StreamName":"","cat_2_FilterBPF":"","cat_2_CPUIdle":0.0000,"cat_2_CPUFetch":0.0000,"cat_2_CPUProcess":0.0000,"cat_2_CPUSend":0.0000,"cat_3_Enable":false,"cat_3_Mode":"","cat_3_CPUMain":0,"cat_3_TSPCAP":0,"cat_3_ReadPkt":0,"cat_3_ReadByte":0,"cat_3_ReadTotalPkt":0,"cat_3_ReadTotalByte":0,"cat_3_ReadGbps":0.000000,"cat_3_ReadMpps":0.000000,"cat_3_WritePkt":0,"cat_3_WriteByte":0,"cat_3_WriteTotalPkt":0,"cat_3_WriteTotalByte":0,"cat_3_WriteGbps":0.000000,"cat_3_WriteMpps":0.000000,"cat_3_PendingByte":0,"cat_3_PktDiscard":0,"cat_3_PktDiscardTotal":0,"cat_3_PktSlice":0,"cat_3_IOPriority":0,"cat_3_ChunkID":0,"cat_3_CmdLine":"","cat_3_StreamName":"","cat_3_FilterBPF":"","cat_3_CPUIdle":0.0000,"cat_3_CPUFetch":0.0000,"cat_3_CPUProcess":0.0000,"cat_3_CPUSend":0.0000,"cat_4_Enable":false,"cat_4_Mode":"","cat_4_CPUMain":0,"cat_4_TSPCAP":0,"cat_4_ReadPkt":0,"cat_4_ReadByte":0,"cat_4_ReadTotalPkt":0,"cat_4_ReadTotalByte":0,"cat_4_ReadGbps":0.000000,"cat_4_ReadMpps":0.000000,"cat_4_WritePkt":0,"cat_4_WriteByte":0,"cat_4_WriteTotalPkt":0,"cat_4_WriteTotalByte":0,"cat_4_WriteGbps":0.000000,"cat_4_WriteMpps":0.000000,"cat_4_PendingByte":0,"cat_4_PktDiscard":0,"cat_4_PktDiscardTotal":0,"cat_4_PktSlice":0,"cat_4_IOPriority":0,"cat_4_ChunkID":0,"cat_4_CmdLine":"","cat_4_StreamName":"","cat_4_FilterBPF":"","cat_4_CPUIdle":0.0000,"cat_4_CPUFetch":0.0000,"cat_4_CPUProcess":0.0000,"cat_4_CPUSend":0.0000,"cat_5_Enable":false,"cat_5_Mode":"","cat_5_CPUMain":0,"cat_5_TSPCAP":0,"cat_5_ReadPkt":0,"cat_5_ReadByte":0,"cat_5_ReadTotalPkt":0,"cat_5_ReadTotalByte":0,"cat_5_ReadGbps":0.000000,"cat_5_ReadMpps":0.000000,"cat_5_WritePkt":0,"cat_5_WriteByte":0,"cat_5_WriteTotalPkt":0,"cat_5_WriteTotalByte":0,"cat_5_WriteGbps":0.000000,"cat_5_WriteMpps":0.000000,"cat_5_PendingByte":0,"cat_5_PktDiscard":0,"cat_5_PktDiscardTotal":0,"cat_5_PktSlice":0,"cat_5_IOPriority":0,"cat_5_ChunkID":0,"cat_5_CmdLine":"","cat_5_StreamName":"","cat_5_FilterBPF":"","cat_5_CPUIdle":0.0000,"cat_5_CPUFetch":0.0000,"cat_5_CPUProcess":0.0000,"cat_5_CPUSend":0.0000,"cat_6_Enable":false,"cat_6_Mode":"","cat_6_CPUMain":0,"cat_6_TSPCAP":0,"cat_6_ReadPkt":0,"cat_6_ReadByte":0,"cat_6_ReadTotalPkt":0,"cat_6_ReadTotalByte":0,"cat_6_ReadGbps":0.000000,"cat_6_ReadMpps":0.000000,"cat_6_WritePkt":0,"cat_6_WriteByte":0,"cat_6_WriteTotalPkt":0,"cat_6_WriteTotalByte":0,"cat_6_WriteGbps":0.000000,"cat_6_WriteMpps":0.000000,"cat_6_PendingByte":0,"cat_6_PktDiscard":0,"cat_6_PktDiscardTotal":0,"cat_6_PktSlice":0,"cat_6_IOPriority":0,"cat_6_ChunkID":0,"cat_6_CmdLine":"","cat_6_StreamName":"","cat_6_FilterBPF":"","cat_6_CPUIdle":0.0000,"cat_6_CPUFetch":0.0000,"cat_6_CPUProcess":0.0000,"cat_6_CPUSend":0.0000,"cat_7_Enable":false,"cat_7_Mode":"","cat_7_CPUMain":0,"cat_7_TSPCAP":0,"cat_7_ReadPkt":0,"cat_7_ReadByte":0,"cat_7_ReadTotalPkt":0,"cat_7_ReadTotalByte":0,"cat_7_ReadGbps":0.000000,"cat_7_ReadMpps":0.000000,"cat_7_WritePkt":0,"cat_7_WriteByte":0,"cat_7_WriteTotalPkt":0,"cat_7_WriteTotalByte":0,"cat_7_WriteGbps":0.000000,"cat_7_WriteMpps":0.000000,"cat_7_PendingByte":0,"cat_7_PktDiscard":0,"cat_7_PktDiscardTotal":0,"cat_7_PktSlice":0,"cat_7_IOPriority":0,"cat_7_ChunkID":0,"cat_7_CmdLine":"","cat_7_StreamName":"","cat_7_FilterBPF":"","cat_7_CPUIdle":0.0000,"cat_7_CPUFetch":0.0000,"cat_7_CPUProcess":0.0000,"cat_7_CPUSend":0.0000,"cat_EnableCnt":0,"cat_ReadPkt":0,"cat_ReadByte":0,"cat_ReadTotalPkt":0,"cat_ReadTotalByte":0,"cat_ReadGbps":0.000000,"cat_ReadMpps":0.000000,"cat_WritePkt":0,"cat_WriteByte":0,"cat_WriteTotalPkt":0,"cat_WriteTotalByte":0,"cat_WriteGbps":0.000000,"cat_WriteMpps":0.000000}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "cat",
  "timestamp": 1686730073,
  "ver": "8779",
  "cat_0_Enable": false,
  "cat_0_Mode": "",
  "cat_0_CPUMain": 0,
  "cat_0_TSPCAP": 0,
  "cat_0_ReadPkt": 0,
  "cat_0_ReadByte": 0,
  "cat_0_ReadTotalPkt": 0,
  "cat_0_ReadTotalByte": 0,
  "cat_0_ReadGbps": 0,
  "cat_0_ReadMpps": 0,
  "cat_0_WritePkt": 0,
  "cat_0_WriteByte": 0,
  "cat_0_WriteTotalPkt": 0,
  "cat_0_WriteTotalByte": 0,
  "cat_0_WriteGbps": 0,
  "cat_0_WriteMpps": 0,
  "cat_0_PendingByte": 0,
  "cat_0_PktDiscard": 0,
  "cat_0_PktDiscardTotal": 0,
  "cat_0_PktSlice": 0,
  "cat_0_IOPriority": 0,
  "cat_0_ChunkID": 0,
  "cat_0_CmdLine": "",
  "cat_0_StreamName": "",
  "cat_0_FilterBPF": "",
  "cat_0_CPUIdle": 0,
  "cat_0_CPUFetch": 0,
  "cat_0_CPUProcess": 0,
  "cat_0_CPUSend": 0,
  
  "cat_1_Enable": false,
  "cat_1_Mode": "",
  "cat_1_CPUMain": 0,
  "cat_1_TSPCAP": 0,
  "cat_1_ReadPkt": 0,
  "cat_1_ReadByte": 0,
  "cat_1_ReadTotalPkt": 0,
  "cat_1_ReadTotalByte": 0,
  "cat_1_ReadGbps": 0,
  "cat_1_ReadMpps": 0,
  "cat_1_WritePkt": 0,
  "cat_1_WriteByte": 0,
  "cat_1_WriteTotalPkt": 0,
  "cat_1_WriteTotalByte": 0,
  "cat_1_WriteGbps": 0,
  "cat_1_WriteMpps": 0,
  "cat_1_PendingByte": 0,
  "cat_1_PktDiscard": 0,
  "cat_1_PktDiscardTotal": 0,
  "cat_1_PktSlice": 0,
  "cat_1_IOPriority": 0,
  "cat_1_ChunkID": 0,
  "cat_1_CmdLine": "",
  "cat_1_StreamName": "",
  "cat_1_FilterBPF": "",
  "cat_1_CPUIdle": 0,
  "cat_1_CPUFetch": 0,
  "cat_1_CPUProcess": 0,
  "cat_1_CPUSend": 0,
  
  "cat_2_Enable": false,
  "cat_2_Mode": "",
  "cat_2_CPUMain": 0,
  "cat_2_TSPCAP": 0,
  "cat_2_ReadPkt": 0,
  "cat_2_ReadByte": 0,
  "cat_2_ReadTotalPkt": 0,
  "cat_2_ReadTotalByte": 0,
  "cat_2_ReadGbps": 0,
  "cat_2_ReadMpps": 0,
  "cat_2_WritePkt": 0,
  "cat_2_WriteByte": 0,
  "cat_2_WriteTotalPkt": 0,
  "cat_2_WriteTotalByte": 0,
  "cat_2_WriteGbps": 0,
  "cat_2_WriteMpps": 0,
  "cat_2_PendingByte": 0,
  "cat_2_PktDiscard": 0,
  "cat_2_PktDiscardTotal": 0,
  "cat_2_PktSlice": 0,
  "cat_2_IOPriority": 0,
  "cat_2_ChunkID": 0,
  "cat_2_CmdLine": "",
  "cat_2_StreamName": "",
  "cat_2_FilterBPF": "",
  "cat_2_CPUIdle": 0,
  "cat_2_CPUFetch": 0,
  "cat_2_CPUProcess": 0,
  "cat_2_CPUSend": 0,
  
  "cat_3_Enable": false,
  "cat_3_Mode": "",
  "cat_3_CPUMain": 0,
  "cat_3_TSPCAP": 0,
  "cat_3_ReadPkt": 0,
  "cat_3_ReadByte": 0,
  "cat_3_ReadTotalPkt": 0,
  "cat_3_ReadTotalByte": 0,
  "cat_3_ReadGbps": 0,
  "cat_3_ReadMpps": 0,
  "cat_3_WritePkt": 0,
  "cat_3_WriteByte": 0,
  "cat_3_WriteTotalPkt": 0,
  "cat_3_WriteTotalByte": 0,
  "cat_3_WriteGbps": 0,
  "cat_3_WriteMpps": 0,
  "cat_3_PendingByte": 0,
  "cat_3_PktDiscard": 0,
  "cat_3_PktDiscardTotal": 0,
  "cat_3_PktSlice": 0,
  "cat_3_IOPriority": 0,
  "cat_3_ChunkID": 0,
  "cat_3_CmdLine": "",
  "cat_3_StreamName": "",
  "cat_3_FilterBPF": "",
  "cat_3_CPUIdle": 0,
  "cat_3_CPUFetch": 0,
  "cat_3_CPUProcess": 0,
  "cat_3_CPUSend": 0,
  
  "cat_4_Enable": false,
  "cat_4_Mode": "",
  "cat_4_CPUMain": 0,
  "cat_4_TSPCAP": 0,
  "cat_4_ReadPkt": 0,
  "cat_4_ReadByte": 0,
  "cat_4_ReadTotalPkt": 0,
  "cat_4_ReadTotalByte": 0,
  "cat_4_ReadGbps": 0,
  "cat_4_ReadMpps": 0,
  "cat_4_WritePkt": 0,
  "cat_4_WriteByte": 0,
  "cat_4_WriteTotalPkt": 0,
  "cat_4_WriteTotalByte": 0,
  "cat_4_WriteGbps": 0,
  "cat_4_WriteMpps": 0,
  "cat_4_PendingByte": 0,
  "cat_4_PktDiscard": 0,
  "cat_4_PktDiscardTotal": 0,
  "cat_4_PktSlice": 0,
  "cat_4_IOPriority": 0,
  "cat_4_ChunkID": 0,
  "cat_4_CmdLine": "",
  "cat_4_StreamName": "",
  "cat_4_FilterBPF": "",
  "cat_4_CPUIdle": 0,
  "cat_4_CPUFetch": 0,
  "cat_4_CPUProcess": 0,
  "cat_4_CPUSend": 0,
  
  "cat_5_Enable": false,
  "cat_5_Mode": "",
  "cat_5_CPUMain": 0,
  "cat_5_TSPCAP": 0,
  "cat_5_ReadPkt": 0,
  "cat_5_ReadByte": 0,
  "cat_5_ReadTotalPkt": 0,
  "cat_5_ReadTotalByte": 0,
  "cat_5_ReadGbps": 0,
  "cat_5_ReadMpps": 0,
  "cat_5_WritePkt": 0,
  "cat_5_WriteByte": 0,
  "cat_5_WriteTotalPkt": 0,
  "cat_5_WriteTotalByte": 0,
  "cat_5_WriteGbps": 0,
  "cat_5_WriteMpps": 0,
  "cat_5_PendingByte": 0,
  "cat_5_PktDiscard": 0,
  "cat_5_PktDiscardTotal": 0,
  "cat_5_PktSlice": 0,
  "cat_5_IOPriority": 0,
  "cat_5_ChunkID": 0,
  "cat_5_CmdLine": "",
  "cat_5_StreamName": "",
  "cat_5_FilterBPF": "",
  "cat_5_CPUIdle": 0,
  "cat_5_CPUFetch": 0,
  "cat_5_CPUProcess": 0,
  "cat_5_CPUSend": 0,
  
  "cat_6_Enable": false,
  "cat_6_Mode": "",
  "cat_6_CPUMain": 0,
  "cat_6_TSPCAP": 0,
  "cat_6_ReadPkt": 0,
  "cat_6_ReadByte": 0,
  "cat_6_ReadTotalPkt": 0,
  "cat_6_ReadTotalByte": 0,
  "cat_6_ReadGbps": 0,
  "cat_6_ReadMpps": 0,
  "cat_6_WritePkt": 0,
  "cat_6_WriteByte": 0,
  "cat_6_WriteTotalPkt": 0,
  "cat_6_WriteTotalByte": 0,
  "cat_6_WriteGbps": 0,
  "cat_6_WriteMpps": 0,
  "cat_6_PendingByte": 0,
  "cat_6_PktDiscard": 0,
  "cat_6_PktDiscardTotal": 0,
  "cat_6_PktSlice": 0,
  "cat_6_IOPriority": 0,
  "cat_6_ChunkID": 0,
  "cat_6_CmdLine": "",
  "cat_6_StreamName": "",
  "cat_6_FilterBPF": "",
  "cat_6_CPUIdle": 0,
  "cat_6_CPUFetch": 0,
  "cat_6_CPUProcess": 0,
  "cat_6_CPUSend": 0,
  
  "cat_7_Enable": false,
  "cat_7_Mode": "",
  "cat_7_CPUMain": 0,
  "cat_7_TSPCAP": 0,
  "cat_7_ReadPkt": 0,
  "cat_7_ReadByte": 0,
  "cat_7_ReadTotalPkt": 0,
  "cat_7_ReadTotalByte": 0,
  "cat_7_ReadGbps": 0,
  "cat_7_ReadMpps": 0,
  "cat_7_WritePkt": 0,
  "cat_7_WriteByte": 0,
  "cat_7_WriteTotalPkt": 0,
  "cat_7_WriteTotalByte": 0,
  "cat_7_WriteGbps": 0,
  "cat_7_WriteMpps": 0,
  "cat_7_PendingByte": 0,
  "cat_7_PktDiscard": 0,
  "cat_7_PktDiscardTotal": 0,
  "cat_7_PktSlice": 0,
  "cat_7_IOPriority": 0,
  "cat_7_ChunkID": 0,
  "cat_7_CmdLine": "",
  "cat_7_StreamName": "",
  "cat_7_FilterBPF": "",
  "cat_7_CPUIdle": 0,
  "cat_7_CPUFetch": 0,
  "cat_7_CPUProcess": 0,
  "cat_7_CPUSend": 0,
  
  "cat_EnableCnt": 0,
  "cat_ReadPkt": 0,
  "cat_ReadByte": 0,
  "cat_ReadTotalPkt": 0,
  "cat_ReadTotalByte": 0,
  "cat_ReadGbps": 0,
  "cat_ReadMpps": 0,
  "cat_WritePkt": 0,
  "cat_WriteByte": 0,
  "cat_WriteTotalPkt": 0,
  "cat_WriteTotalByte": 0,
  "cat_WriteGbps": 0,
  "cat_WriteMpps": 0
}
```

## FMADIO Push PCAP Status

Provides status information of the currently active Push PCAP proceses

```
2023.06.14-16:11:44.342725 (+08:00) | fmadio20v2-149 | local7.info     | fmadio    | {"module":"push_pcap","subsystem":"status","timestamp":1686730304,"ver":"8769","Process":"voip-hk                       ","IsUp":  true,"Splits":57285,"TotalByte":     63860619926,"TotalPkt":       228209873,"TransferMbps":     18.00,"PCAPTS":1686730299890543104,"FilterBPF":"vlan 1804","FilterFrame":"nil","Target":"/mnt/remote0/pcap/20230614/voip-hk-"}
```

Pretty JSON

```json
{
  "module": "push_pcap",
  "subsystem": "status",
  "timestamp": 1686730363,
  "ver": "8769",
  "Process": "voip-hk                       ",
  "IsUp": true,
  "Splits": 57343,
  "TotalByte": 63955460513,
  "TotalPkt": 228548682,
  "TransferMbps": 12,
  "PCAPTS": 1686730357291535600,
  "FilterBPF": "vlan 1804",
  "FilterFrame": "nil",
  "Target": "/mnt/remote0/pcap/20230614/voip-hk-"
}
```

## FMADIO Push PCAP File

Called after completing a PCAP file split event. Helpful to monitoring each and every PCAP split leaving the system

```
2023.06.14-16:14:43.131050 (+08:00) | fmadio20v2-149 | user.notice     | fmadio    | {"module":"push_pcap","subsystem":"split","timestamp":1686730483,"ver":"8769","filename":"/mnt/remote0/pcap/20230614/icmp-20230614_161436.pcap.zstd","Byte":896,"Pkt":8,"TimeWall":0.670407,"TimePCAP":1.005172,"PCATSStart":1686730477000000000,"PCAPTSEnd":1686730476874467328}
```

Pretty JSON

```json
{
  "module": "push_pcap",
  "subsystem": "split",
  "timestamp": 1686730521,
  "ver": "8769",
  "filename": "/mnt/remote0/pcap/20230614/icmp-20230614_161514.pcap.zstd",
  "Byte": 896,
  "Pkt": 8,
  "TimeWall": 0.930522,
  "TimePCAP": 0.988352,
  "PCATSStart": 1686730515000000000,
  "PCAPTSEnd": 1686730514909767700
}

```

## FMADIO Alert

The system has capibility to generate system alerts based on configuration files. These alerts can be sent to SYSLOG for ingestion by a monitoring system

```
2023.06.14-16:17:59.032528 (+08:00) | fmadio100v2-228U | local7.info     | fmadio    | {"module":"system","subsystem":"alert","timestamp":1686730679,"ver":"8779","Subject":"CPU Temperature","Message":"CPU Temperature Over CPU0:39 CPU1:43 > 20"}
```

Pretty JSON

```json
{
  "module": "system",
  "subsystem": "alert",
  "timestamp": 1686730679,
  "ver": "8779",
  "Subject": "CPU Temperature",
  "Message": "CPU Temperature Over CPU0:39 CPU1:43 > 20"
}
```

