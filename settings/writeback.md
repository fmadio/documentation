# HDD Writeback

**FW: 7219+**

Configuration options are in the specified config file. Please note all options requires capture to be stopped and started before settings are applied.

```
/opt/fmadio/etc/time.lua
```

Specifically the Writeback block, example as follows

```
["Writeback"] =
{
        ["IOPriority"]       = 11,
        ["EnableCompress"]   = true,
        ["CheckBlockCRC"]    = true,
        ["EnableECC"]        = true,
},
```

### IOPriority

This setting changes the default writeback IO Priority, allows changing preference for faster Downloads(default) or faster sustained Writeback to magnetic storage.

| Setting | Description                                                                    |
| ------- | ------------------------------------------------------------------------------ |
| 11      | <p>Writeback to Magnetic storage is lowest priority </p><p>(Default Value)</p> |
| 30      | Writeback to disk has higher priority than Download or Push speed              |

### EnableCompress

Setting enables or disables disk compression. For faster sustained writeback to disk speeds, disable compression.&#x20;

For 1U systems disabling compression makes little difference due to lower of HDD write bandwidth.&#x20;

For 2U systems disabling compression improves sustained writeback to HDD performance. As the system has 12 spinning disks with an aggregate 10Gbps-20Gbps (depending on spindle position) write throughput.

| Setting | Description                                                                    |
| ------- | ------------------------------------------------------------------------------ |
| true    | <p>Enable Compression </p><p>(Default setting)</p>                             |
| false   | Disable disk compression. Faster sustained writeback performance on 2U systems |

### CheckBlockCRC

This function checks the CRC when reading data from the SSDs. It calculates the CRC and checks for a match against the orignial captured CRC value, before writing the block to magnetic storage. This adds additional CPU overhead.&#x20;

Disabling this improves the sustained write performance on 2U systems. On 1U systems there is little performance advantage

| Setting | Description                                                                              |
| ------- | ---------------------------------------------------------------------------------------- |
| true    | <p>Enable SSD CRC Checks before writing to Magnetic Storage </p><p>(Default setting)</p> |
| false   | Do not check SSD CRC data check. This improves sustained writeback performance           |

### EnableECC

This is mostly a debug setting. by disabling ECC it removes the RAID5 calculation and additional IO writeback. Turning the system into a RAID0 configuration. This is mostly for debug testing and not recommend for production systems

| Setting | Description                                                |
| ------- | ---------------------------------------------------------- |
| true    | <p>Calculate ECC RAID5 Parity </p><p>(Default setting)</p> |
| false   | No ECC calculation (RAID0 mode)                            |

## Recommended Settings

The default settings are recommended unless there is specific use cases.

### Maximum Sustained Capture  Rate

For Maximum sustained capture rate we recommended the following settings. This disables the compression and priorities Magnetic Storage writeback over download performance.

```
["Capture"] =
{
    ["Inline"]             = false,
    ["PortMode"]           = "2x10G",
    ["FlushPktCnt"]        = 2000,
    ["FlushPeriod"]        = 0,
    ["FlushIdle"]          = 1000000000,

    ["IOPriorityLevel"]    = 40,
    ["IOPriorityHighByte"] = 128*1024*1024,

    ["CaptureSizeMax"]     = 0,
    ["CaptureTimeMax"]     = 0,
    
    ["DisableCPUPriority"] = true,
},
["Writeback"] =
{
    ["IOPriority"]         = 50,
    ["EnableCompress"]     = false,
    ["EnableECC"]          = true,
    ["CheckBlockCRC"]      = false,
},
```
