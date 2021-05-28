---
description: A guide for developers to access information from a FMADIO device.
---

# Summary

### Original API

The FMADIO API is simple and designed for easy scripting integration.

Note: Replace the IP 1.1.1.1 with the host IP of your FMADIO device.

| Description | Url |
| :--- | :--- |
| **DEVICE OPERATION** |  |
| Start Capture on the device | `http://1.1.1.1/sysmaster/capture_start?StreamName=<capture name>` |
| Stop the current Capture | `http://1.1.1.1/sysmaster/capture_stop` |
| Get current Capture Status | `http://1.1.1.1/sysmaster/status` |
| **DOWNLOADING PCAP**  |  |
| List all captures on the device | `http://1.1.1.1/stream/list` |
| Split a capture by file size | `http://1.1.1.1/stream/ssize?StreamName=<capture sname>&StreamView=<split mode>` |
| Split a capture by time | `http://1.1.1.1/stream/stime?StreamName=<capture sname>&StreamView=<split mode>` |
| Download full capture as single PCAP | `http://1.1.1.1/pcap/single?StreamName=<capture name>` |
| Download capture as single PCAP with gz compression | `http://1.1.1.1/pcap/single?StreamName=<capture name>?Compression=fast` |
| Download capture within a specific time | `http://1.1.1.1/pcap/splittime?StreamName=<capture name>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |
| Download capture with BPF Filter | `http://1.1.1.1/pcap/single?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>` |
| Download capture with BPF Filter and time range | `http://1.1.1.1/pcap/splittime?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |
| Download capture with RegEx DPI Filter | `http://1.1.1.1/pcap/splittime?StreamName=<capture name>&FilterRE=<escape encoded RegEx expression>` |
| Download capture based on Capture Port number | `http://1.1.1.1/pcap/splittime?StreamName=<capture name>&FilterPort=<numeric port number>` |
| **DEVICE MANAGEMENT** |  |
| Get system status information | `http://1.1.1.1/sysmaster/stats_summary` |



### V1 API

The FMADIO V1 API uses endpoints with parameters. All V1 versions of the API endpoints shall have the format:

`/api/v1/<operation>/<task>?<params=x>`

The V1 API has advantages of the previous API. These are:

* Multiple downloads can occur at the same time on the same device \(up to 4 concurrently\).
* Improved download performance
* Ability to use compressed and bpf filter  parameters on most downloads.

Note: All original API url's shall be available as well as the new V1 endpoints.

| Description | Url |
| :--- | :--- |
| **DOWNLOADING PCAP**  |  |
| Download full capture as single PCAP | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>` |
| Download capture as single PCAP with gz compression | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>?Compression=fast` |
| Download a time range of pcaps | `http://1.1.1.1/api/v1/pcap/timerange?TSBegin=<nano second epoch start time>&TSEnd=<nano second epoch stop time>&TSMax=<nano second size>&TSMode=<nanos or msecs>` |
| Download capture within a specific time | `http://1.1.1.1/api/v1/pcap/splittime?StreamName=<capture name>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |
| Download capture with BPF Filter | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>` |
| Download capture with BPF Filter and time range | `http://1.1.1.1/api/v1/pcap/splittime?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |

