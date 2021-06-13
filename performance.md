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



### No Compression, No CRC, No Download \(Writeback Performance Mode\)

This number shows stock FMADIO20G-2U-120TB systems Capture and Writeback performance. As shown both Burst Capture and Sustained Capture rates @ 10Gbps are possible across the entire storage. However above 10Gbps Burst Capture is limited to SSD size \(4TB\) as there is not enough IO Bandwidth on the SSDs for total of 40Gbps \(20Gbs write and 20Gbps Read\) thus the HDD Writeback performance suffers.

Tests using FMADIO20Gv3-2U-48TB System

![](.gitbook/assets/image%20%2845%29.png)

