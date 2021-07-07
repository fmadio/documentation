# Capture Pipeline Flush

FMADIO Packet Capture systems like all capture systems have multiple internal buffers. These internal buffers can sometimes cause problems for low bandwidth connections which requires Packets to be available on disk immediately for downstream processing.

One such example is Financial Order and Entry data, which can sometimes be extremely low bandwidth however downstream systems require packets to be available ASAP for further processing.

FMADIO Gen2 systems buffer between 2MB-4MB of data internally. To support multiple use cases the flushing mechanics can be tuned based on the customers requirements. By default the flushing occurs when there is no new packets in the last 1 second.

## Configuration

Please edit the configuration file in

```text
/opt/fmadio/etc/time.lua 
```

The relevant sections are \(there may be more or less entries in the \["Capture"\] config block\)

```text
["Capture"] =
{

        ["FlushPktCnt"] = 2000,
        ["FlushPeriod"] = 0,
        ["FlushIdle"]   = 1e9,
        
}
```

If these options are not visible in the config file, please go to the GUI Config page, change the PCAP Time Resolution to Micro Second, then back to Nano Second. This will write the default values into the config file. Alternatively you can paste the missing lines from the above example.

NOTE: After changing the settings capture must be stopped, and restarted for the new settings to take effect

## Settings

### **FLUSHPKTCNT**

Flushing works by injecting specially marked NOP packets into the system right at the capture port. Its as if the packets arrived on the ingress port, but are never visible or downloadable. This parameter sets the number of packets for each flush per port to be injected. The packets are 256B in length.

Default value is: 2000 pkts \* 256B = 512,000 bytes per port.

For usage models where quick Flush to disk is critical, its recommended to use 5,000 or 10,000 packets for a complete flush. Note this will directly effect how much storage is consumed by the flushing behavior

### **FLUSHPERIOD**

Flushing based on a pre-defined time interval. For example flush the entire pipeline every 1 minute regardless of how much data has been seen. For a 1 minute flush, the value here should be 60e9, scientific notation is accepted and the unit of time is nano seconds.

Default value is: 0 - this disables the periodic flushing

Lowest recommended setting is 1 minute, otherwise excessive flushing will consume disk space.

**FLUSHIDLE**

Flushing based on an in-activity idle timeout. This will flush the pipeline if no new packets are received within X amount of time. For example the default setting is 1 second, if no new packets are received after 1 second a SINGLE pipeline flush is issued. The next pipeline flush will only occur if new packets are received.

Default value is: 1e9 - flush after 1 second of inactivity, value in nano seconds To disable set to 0

This mode is the default configuration

## Recommended \(Aggressive Flushing\)

For Financial and other customers who require a constant flush to disk, the following setting is recommended

```text
        ["FlushPktCnt"] = 5000,
        ["FlushPeriod"] = 60e9,
        ["FlushIdle"]   = 0,
```

This will flush both ports every 1 minute continuously.

1 Hour / 1 Min = 60 flushes

1 Flush 2 x 5000 packets \* 256 Bytes = 2,560,000 Bytes per flush

Total of extra 153MB per hour for the continuous flushing. or 1.2GB for 8 Hours is fairly reasonable.

