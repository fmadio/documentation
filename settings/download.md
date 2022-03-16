# Download

Various settings related to downloading PCAP.

## API Version 1 Enable

Depending on the packet capture system the Download interface may be in Legacy mode. This modes functionality is limited in the range of API calls, specifically /api/\* calls can not be used

To enable API Version 1, edit the configuration file

```
/opt/fmadio/etc/time.lua
```

Find the section \["PCAP"] example as below

```
["PCAP"] =
{
    ["TimeStampMode"]       = "nic",
    ["TimeResolution"]      = "nsec",
    ["TimeSortDepth"]       = 256,
    ["Decap"]               = false,
    ["Slice"]               = 0,
    ["DownloadIdleTimeout"] = 30000000000,
    ["DownloadAPI"]         = "legacy",
    ["FShark"]              = true,
},
```

Please change \["DownloadAPI"]= "legacy"   to "v1" as below. If theres no entry, create the entry.

```
["DownloadAPI"]         = "v1",
```

It requires a browser refresh for the setting to become active.
