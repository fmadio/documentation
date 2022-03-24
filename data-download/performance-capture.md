# Performance Capture

## Firmware: **7768**

FMADIO100G Capture system is for 100% lossless half duplex 100G link.

The system can write 100Gbps+ to Disk and 148M Packets+ all without any loss. This assumes data is on a single 1x100G capture port

As the system has 2x100G capture ports there is a point where due to data rate or per packet rate, the system will drop packets, once it exceed 148M packets / sec or 100Gbps data rate.

The following graphs show the performance characteristics when exceeding 100Gbps line rate

### Microburst 2 x 100G  @ 100 Packets

Microburst of 100 packets at 2 x 100G line rate from 64B to 1500B packets. 100% full capture no loss.

![2x100G Microburst @ 100 Packets](<../.gitbook/assets/image (123).png>)

### Microburst 2 x 100G @ 200 Packets

Microbust of 200 packets per port 2 x 100G line rate from 64B to 1500B packets. Slight capture loss on the larger packet sizes.

![2x100G Microburst @ 200 Packets](<../.gitbook/assets/image (121).png>)

### Microburst 2 x 100G @ 500Packets

Microburst of 500packets per port 2 x 100G line rate from 64B to 1500B packets. Start to see more significant packet loss.

![](<../.gitbook/assets/image (129).png>)

### Microburst 2 x 100G @ 1,000 Packets

Microburst of 1K packets per port 2 x 100G line rate from 64B to 1500B packets.

![2x100G Microburst @ 1K Packets](<../.gitbook/assets/image (115).png>)

### Microburst 2 x 100G @ 10,000 Packets

Microburst of 10K packets per port 2 x 100G line rate 64B to 1500B packets

![2x100G Microburst @ 10K Packets](<../.gitbook/assets/image (118).png>)

### Microburst 2 x 100G @ 100,000 Packets

Microburst of 100K packets per port 2 x 100G line rate 64B to 1500B packets

![2x100G Microburst @ 100K Packets](<../.gitbook/assets/image (126).png>)

### Microburst 2 x 100G @ 1,000,000 Packets

Microburst of 1M Packets per port 2 x 100G line rate 64B to 1500B packets





