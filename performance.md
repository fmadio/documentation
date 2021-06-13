# Performance

The performance of a capture system can be characterized in a number of different ways. We provide the following performance dimensions

## Microburst Capture Speed \( &lt; 500msec \)

This is the short time burst capture rate of the system. For the 100G Gen2 system this is burst capture rate that fills up the DDR buffer on the FMAD FPGA Capture card.

## Burst Capture Speed \( &lt; 10min\)

For the 100G Gen2 FMADIO Packet Capture system, all storage is on high speed NVMe SSDs, so the Burst Capture Speed is the same as the Sustained Capture Speed.

FMADIO 20G and 40G systems use a mixture of SSD and magnetic disk storage, so for those systems the Burst Capture Speed is higher than the Sustained Capture Speed.

## Sustained Capture Speed \(24/7\)

We indicate this as the sustained capture rate, i.e. the capture rate that a system can sustain 24/7 without any packet loss. As mixing capture with downloads effects the capture speed, this performance metric is _Capture Only_ with no simultaneous/concurrent downloads.

## Capture and Download Speed

Performance metric is assuming no bottlenecks on the egress \(download client\) what is the capture performance while simultaneously downloading.

## Download Only speed

The other metric is Download only speed. This metric is used to calculate the maximum rate data can be moved off the device over 10G or 40G ethernet.

## FMADIO 20G 2U System

FMADIO 20G 2U Packet Capture system has 4TB of SSD Cache and 48TB-216TB worth of HDD Magnetic storage.

### Compression Enabled, CRC Check, No Download \(Default\)

The default setting has Compression and CRC checks enabled. Its designed to get maximum total storage capacity via the use of compression and CRC Checks for data integrity. This specific dataset is incompressible, thus the writeback performance is the raw hardware performance.

![FMADIO20G 2U Default Performance](.gitbook/assets/image%20%2869%29.png)

Testing: FMADIO20Gv3-2U-48TB System

### Compression + CRC Check + Download \(SSD\)

Compression and CRC checks enabled and downloads that hit the SSD cache. e.g Download data is on SSDs and does not require access to the HDD. Download is using localhost to remove network performance from the test.

![FMADIO20G 2U Default Performance Capture + Download\(SSD\)](.gitbook/assets/image%20%2854%29.png)

Testing: FMADIO20Gv3-2U-48TB System

### Compression + CRC Check + Download \(HDD\)

There is a difference when downloading from SSD cache vs HDD storage, as seen below. When a download has to fetch data from HDD magnetic storage, it dramatically effects the throughput of the HDD writeback. This is physical limitation of magnetic storage, as its a physical spinning disk which has poor random IO access performance. This is clearly seen in the significantly lower writeback and download speed, as shown below.

![FMADIO20G 2U Default Performance Capture + Download\(HDD\)](.gitbook/assets/image%20%2850%29.png)

Testing: FMADIO20Gv3-2U-48TB System

### No Compression, No CRC Check, No Download \(Writeback Performance Mode\)

This setting shows Writeback performance optimized mode per

[https://docs.fmad.io/fmadio-documentation/v/fmadio20v3/settings/writeback\#maximum-sustained-capture-rate](https://docs.fmad.io/fmadio-documentation/v/fmadio20v3/settings/writeback#maximum-sustained-capture-rate)\).

This setup both Burst Capture and Sustained Capture rates @ 10Gbps are possible across the entire storage. However above 10Gbps Burst Capture is limited to SSD size \(4TB\) as beyond that the magnetic storage performance becomes a bottleneck.

![FMADIO20G 2U Writeback Performance Mode](.gitbook/assets/image%20%2845%29.png)

Testing: FMADIO20Gv3-2U-48TB System

