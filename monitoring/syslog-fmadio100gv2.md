# Syslog FMADIO100Gv2

Example SYSLOG for FMADIO 100G Gen2 output

```
2021.05.24-03:45:38.472722 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Temp      (CPU0:34.00 CPU1:34.00 PCH:34.00 SYS:32.00 PER:24.00 NIC:44.00 AirIn:24.00 AirOut:0.00 Transciver:0.00 0.00)
2021.05.24-03:45:38.475398 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Fan       (SYS0:33600 SYS1:33600 SYS2:33300 SYS3:33600 SYS4:33600 SYS5:33300 SYS6:33600 SYS7:33450)
2021.05.24-03:45:38.477993 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Disk      (29 28 24 24 27 28 26 27 27 26 S: 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 R: 0 )
2021.05.24-03:45:38.481084 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Link      Capture (1 1 0 0 0 0 0 0 ) Man (1G 1 10G 1 )
2021.05.24-03:45:38.483460 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | DiskIO    (Rd:  0.00Gbps Wr:  0.00Gbps)
2021.05.24-03:45:38.485569 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Capture   (Enb: 0 Pkt: 86781352 Drop: 0 FCSError: 0) CaptureRateRate 0.00000000 Gbps CaptureName: ()
2021.05.24-03:45:38.488089 (+02:00) | fmadio100v2-228U | local7.info     | fmadio    | Mem       (0.00GB ECC 0) Writeback (0.00GB) Dropped (0.00GB)

```

## Temperature

Example line below

```
Temp (CPU0:34.00 CPU1:34.00 PCH:34.00 SYS:32.00 PER:24.00 NIC:44.00 AirIn:24.00 AirOut:0.00 Transciver:0.00 0.00)
```

### CPU 0/1

CPU Temperature

### PCH

Chipset Temperature

### SYS

System Temperature

### PER

Peripheral Temperature

### NIC

FPGA Capture Temperature

### AirIn

Ambient Air in Temperature. Sensor located AFTER the U.2 SSDs

### Transceiver

Temperature of each QSFP transceiver



## Disk

Example line&#x20;

```
Disk (32 34 34 34 35 36 35 36 36 36 S: 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 16777216 R: 0 ) 
```

### Temperature

First items as above are the disk temperatures shown in degrees Celsius&#x20;

(32 34 34 34 35 36 35 36 36 36)

### SMART Errors

Total smart error counts are shown in the next section starting with "S:"

(S: 0 0 0 0 0 0 0 0 0 0)

### RAID Controller Temperature

Finally the RAID hardware card temperature (if present)

(R: 0 )

## Memory

Example line as follows

```
Mem (0.00GB ECC 0) Writeback (0.00GB) Dropped (0.00GB) 
```

### Mem

Free memory (0.00GB)

Total ECC Memory errors (ECC 0)

### Writeback

For systems with HDD writeback total GB written to HDD

### Dropped

Total amount of packets dropped due to bytes overflow (lack of bandwidth to HDD)

