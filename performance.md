# Performance

The performance of a capture system can be characterized in a number of different ways. We provide the following performance dimensions

### Capture Bust Speed

This is the short time burst capture rate of the system. For the 100G Gen2 system this is burst capture rate that fills up the DDR buffer on the FMAD FPGA Capture card.

### Capture Sustained Speed

We indicate this as the sustained capture rate, e.g. the capture rate we can sustain 24/7 without any packet loss. As mixing capture with downloads effects the capture speed, this performance is **Capture Only** no simultaneous downloads.

### Capture and Download Speed

Performance metric is assuming no bottlenecks on the egress \(download client\) what is the capture performance while simultaneously downloading.

### Download Only speed

The other metric is Download only speed. This metric is used to calculate the maximum rate data can be moved of the device over 10G or 40G ethernet.

