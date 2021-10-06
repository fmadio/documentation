# Capture Analyze

In many scenarios there is a requirement for simultaneous capture and analysis of data in pseudo-realtime. The target performance is bursting \(up to 156TB worth\) of 100Gbps line rate traffic, with an average sustained data rate of ~ 50Gbps. We target 50Gbps as per the graphs below is the hardware limitation of simultaneous sustained 50Gbps write + sustained 50Gbps read.

### Capture Only / Analyze Only

In the below image shows Capture/Write only, or Analyze/Read only FMADIO 100G Gen2 system easily does 100Gbps worth of IO bandwidth

![Capture Only and Analyze Only Raw IO throughput](../.gitbook/assets/image%20%2887%29.png)

### Burst 100Gbps Capture + Sustained 50Gpbs Capture/Analyze

The ideal IO profile is an simultaneous sustained 50Gbps Capture/Write and 50Gbps Analyze/Read profile as shown below. This allows the capture to burst to 100Gbps without significantly effecting the Analyze/Read performance of the system.

![100Gbps Burst Capture + Sustained 50Gbps Capture/Analyze raw IO Thoughput ](../.gitbook/assets/image%20%2889%29.png)

### Sustained 100Gbps Capture / 26Gbps Analyze

Due to hardware IO limitations \(~ 150Gbps max aggregate bandwidth\) at a sustained 100Gbps Capture/Write only a 26Gbps sustained Analyze/Read performance is achievable.

![Sustained 100Gbps Capture + 26Gbps sustained Analyze raw IO Throughput](../.gitbook/assets/image%20%2888%29%20%281%29.png)

Please note all the above numbers, are the maximum limits. Typically the Analyze software performance is impacted more by the packet rate than the data rate, As such the Analysis software performance is usually the bottleneck, not the raw IO hardware limits.

