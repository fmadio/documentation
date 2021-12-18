# Timestamp Accuracy

FMADIO devices timestamp all incoming packets on the **first byte of payload data** received at the PCS layer. The granularity of the timestamp depends on the port speed which selects the PCS clock frequency. The table below shows the granuality of the timestamp based on Port Speed

| Port Speed | Timestamp Granularity (Nanoseconds) |
| ---------- | ----------------------------------- |
| 2x100G     | 3.103ns                             |
| 2x40G      | 3.2ns                               |
| 8x10G      | 6.4ns                               |

Timestamps are always written in nanoseconds, which requires the system to constantly discipline the FPGA internal clock. This provides highly accurate timestamps against global world time down to the nanosecond level.&#x20;

Below is a summary on the accuracy that can be achieved using FMADIO packet capture systems

| Time Synchroniation | Global Time Accuracy |
| ------------------- | -------------------- |
| NTP                 | 100 milliseconds     |
| PTPv2               | 100 nanoseconds      |
| Pulse Per Second    | 10 nanoseconds       |

### Verification Environment

The following is FMADIO internal test setup on how to measure the accuracy of our hardware timestamps.

![Time accuracy Testing setup](<../.gitbook/assets/image (80) (1) (1).png>)

The picture above shows we are verifying the time accuracy of the system by comparing it against FMADIO Gen1 system that uses Solarflare NIC for its capture and timestamping. As the Solarflare also has a TXCO clock and running the full PTPv2 and PPS output master the Solarflare is timestamping packets directly with the TXCO master clock. This creates an excellent test.

### PTPv2 Global Time Accuracy

PTPv2 time accuracy of PTPv2 only is is about +/- 100 nanoseconds. This is the measured results of the above testing setup, the resulting histogram below shows the time delta in nanoseconds between the FMADIO Gen1 (Solarflare master clock timestamp) and the FMADIO Gen3 (8x10G) timestamp as the slave clock.

![FMADIO PTPv2 Only Global Time Accuracy](<../.gitbook/assets/image (118) (1) (1).png>)

Per the above time histogram, the support width is about +/- 100nsec this is the expectation of PTPv2 synchronized clocks. Its pretty good considering it requires no dedicated clock hardware, just time synchronization over 10G ethernet

### PPS Global Time Accuracy

For the ultimate global time accuracy, Pulse Per Second timing gives the most accurate clock synchronization results. This requires dedicated Clock hardware typically using Coax cable and SMA/BNC connectors. Typical setup is a PTPv2 Grandmaster synchronized using a GPS antenna, this Grandmaster has PTPv2 master to sync against but it also has PPS Coax output for slave clocks to achieve the absolute best global time accuracy.

Below is a time histogram of our FMADIO Gen1 (Solarflare NIC as the master clock) compared against FMADIO 40G Gen3 (8x10G) system as the slave clock. The clock are synchronized using PTPv2 and PPS coax cable. The result is impressive

![PPS Global Time Accuracy (Nanoseconds)](<../.gitbook/assets/image (90) (1) (1) (1).png>)

In the above histogram the time bins are 1 nanosecond. As you can see the support width of the histogram is +/- 10ns. This means the global time accuracy is within 10 nanoseconds.&#x20;

This the extreme level of clock synchronization as required by extreme customers. This is particle physics level accuracy as required by the ultimate most high end applications.&#x20;
