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

### FlushPktCnt

This is the number of packets to send \(per pipeline\) when the Capture pipline has to be flushed. Default is 2000

```text
    ["FlushPktCnt"] = 2000,
```

### FlushPeriod

When in continuous output flush mode this is the period \(in nanoseconds\) between flushes. Disabling constant period flushing set this to 0. Default is 0 \(disable\)

```text
    ["FlushPeriod"] = 0,
```

### FlushIdle

This is the idle packet activity timeout. If no _**new**_ packets are received within this period, the pipeline gets flushed. Default value is 1e9 \(1 second\)

```text
    ["FlushIdle"]   = 1e9,
```

### MidnightRollDisable

Disables captures midnight roll. Typically used when capture roll Time is different to the local system clocks Midnight

```text
    ["MidnightRollDisable"]   = true,
```

