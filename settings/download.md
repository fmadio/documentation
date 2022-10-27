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

### Time Stamp Selection

FMADIO Devices use our internal FPGA NIC card for hardware timestamping on all packets. This may not be sufficient for some customers as Tap/Aggregation layers add additional non-deterministic latency due to egress port buffering.

FMADIO Packet Capture has the option to use Packet Brokers and Switch's metadata either in headers or footers. This allows ingress timestamping at the Tap/Agg layer plus gives additional port visibility and filtering capabilities.

By default FMADIO uses our internal hardware timestamps when downloading PCAPs. We support the following formats

* FMADIO Capture Card (Default)
* Cisco ERSPAN v3
* Arista 7130(Metamako)
* Arista 7150 (Overwrite mode)
* Arista 7150 (Insert Mode)

Use the Config page on the FMADIO Capture system to select as follows

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>FMADIO Timestamp Selection Mode</p></figcaption></figure>

NOTE: This can be done retro-actively. e.g. Downloaded PCAPs are generated based on the timestamp selection mode. This setting does not effect the raw captured data.



