# Dashboard

The FMADIO 100G Packet capture dashboard provides a high level overview of the capture system. Example is shown below.

NOTE: Different hardware platforms 20G, 40G, 100G have slightly different dashboard settings.

![FMADIO 100G Gen2 Dashboard Status](../.gitbook/assets/image%20%2856%29.png)

Going over the above in more detail as follows

### Capture Received

Total number of packets received and successfully stored on the capture device

### Capture Bytes

Total number of bytes successfully captured and stored on the device.

### Capture Errors

Errors with packet data seen on the wire. This includes Frame Check Sequence \(FCS\) errors.

### Capture Dropped

Total number of packets dropped / unable to make it to the capture storage system

### Capture Days

Total number of days worth of capture on the system. Calculated as difference between the oldest packet on the system vs the newest packet on the system

### Generate Sent

Total number of packets generated / transmitted on the device. This includes packet blaster and packet replay counters

### Generate Bytes

Total number of bytes generated / transmitted on the device. 

### Generate Errors

Total number of errors occurred during packet generation

### Smart Errors

Total number of new SMART Disk errors

### RAID Status

Current RAID status, GOOD or DEGRADED or FAILED

### System Warning

Counter of system related warning or errors. Currently this is unused

### RAM ECC Errors

Total number of DDR4 System RAM ECC Errors

### Up Time

Total uptime of the system

## Counter Reset

Most of the counters can be reset by clicking on the small circle arrow highlighted in red below.

![](../.gitbook/assets/image%20%2859%29.png)



