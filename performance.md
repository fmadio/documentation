# Performance

The performance of a capture system can be characterized in a number of different ways. We provide the following performance dimensions

### Capture Microburst Speed \( &lt; 500msec \)

This is the short time burst capture rate of the system. For the 100G Gen2 system this is burst capture rate that fills up the DDR buffer on the FMAD FPGA Capture card.

### Capture Burst Speed \( &gt; 10min\)

For the 100G Gen2 FMADIO Packet Capture system, it has only 1 IO Storage performance level \(full 100Gbps\), thus the Capture Burst Speed is the same as the Capture Sustained Speed. 

Other FMADIO capture devices have 2 or 3 I/O Storage performance levels, for those SKUs the Capture Burst Speed and Capture Sustained Speed are different.

### Capture Sustained Speed \(24/7\)

We indicate this as the sustained capture rate, e.g. the capture rate we can sustain 24/7 without any packet loss. As mixing capture with downloads effects the capture speed, this performance metric is **Capture Only** with no simultaneous/concurrent downloads.

### Capture and Download Speed

Performance metric is assuming no bottlenecks on the egress \(download client\) what is the capture performance while simultaneously downloading.

### Download Only speed

The other metric is Download only speed. This metric is used to calculate the maximum rate data can be moved of the device over 10G or 40G ethernet.

