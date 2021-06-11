# Performance

The performance of a capture system can be characterized in a number of different ways. We provide the following performance dimensions

### Microburst Capture Speed \( &lt; 500msec \)

This is the short time burst capture rate of the system. For the 100G Gen2 system this is burst capture rate that fills up the DDR buffer on the FMAD FPGA Capture card.

### Burst Capture Speed \( &lt; 10min\)

For the 100G Gen2 FMADIO Packet Capture system, all storage is on high speed NVMe SSDs, so the Burst Capture Speed is the same as the Sustained Capture Speed. 

FMADIO 20G and 40G systems use a mixture of SSD and magnetic disk storage, so for those systems the Burst Capture Speed is higher than the Sustained Capture Speed.

### Sustained Capture Speed \(24/7\)

We indicate this as the sustained capture rate, i.e. the capture rate that a system can sustain 24/7 without any packet loss. As mixing capture with downloads effects the capture speed, this performance metric is _Capture Only_ with no simultaneous/concurrent downloads.

### Capture and Download Speed

Performance metric is assuming no bottlenecks on the egress \(download client\) what is the capture performance while simultaneously downloading.

### Download Only speed

The other metric is Download only speed. This metric is used to calculate the maximum rate data can be moved off the device over 10G or 40G ethernet.

