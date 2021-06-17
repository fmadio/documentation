# Capture

There are various tunable available only via configuration file editing the fie

```text
/opt/fmadio/etc/time.lua
```

Then in the \["capture"\] section editing each field, an example is shown below

```text
["Capture"] =
{
        ["Inline"]      = false,
        ["PortMode"]    = "2x10G",
        ["FlushPktCnt"] = 2000,
        ["FlushPeriod"] = 0,
        ["FlushIdle"]   = 1000000000,
        ["IOPriorityLevel"]   = 90,
        ["IOPriorityHighByte"]   = 1048576,
        ["CaptureSizeMax"]   = 0,
        ["CaptureTimeMax"]   = 0,
},
```

### CaptureSizeMax

When the total daily capture sizes start exceeding 10TB / day file sizes can get bit too large and difficult to work with. This setting sets the maximum size of a single capture, then rolling \(losslessly\) to a new Capture when the limit is reached. 

Example below rolls the capture file every 1TB

```text
 ["CaptureSizeMax"]   = 1e12,
```

### CaptureTimeMax

In addition to maximum size, for large \(\(10TB+ daily\) capture rates a more simpler approach is to roll the capture every 1H. This reduces the size of each capture to something more manageable.

Example below rolls the capture every 1H \(units are in nano seconds\)

```text
["CaptureTimeMax"]   = 60*60*1e9,
```

### DisableCPUPriority

This setting a debug only as it \(potentially\) reduces Capture performance, specifically on 100G and higher capture systems. Only enable this if directed by FMADIO Support

```text
 ["DisableCPUPriority"]   = true, 
```

Confirm the setting by checking log file /mnt/store0/log/stream\_capture f20.cur where the following log entries will be seen. 

```text
[20210617_175614] fNIC100_RxPollLoop  : 2319 | Disable RxPoll Loop SCHED_RR
[20210617_175614] fNIC100_RxIndexLoop : 2575 | Disable RxIndex 0 Loop SCHED_RR
[20210617_175614] fNIC100_RxIndexLoop : 2575 | Disable RxIndex 0 Loop SCHED_RR
[20210617_175614] fNIC100_RxIndexLoop : 2575 | Disable RxIndex 1 Loop SCHED_RR
```

