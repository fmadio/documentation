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

#### Temp (CPU0:34.00 CPU1:34.00 PCH:34.00 SYS:32.00 PER:24.00 NIC:44.00 AirIn:24.00 AirOut:0.00 Transciver:0.00 0.00)

* CPU0/1&#x20;
  * CPU Temperature
* PCH
  * Chipset Temperature
* SYS
  * Chipset Temperature
* PER
  * Peripheral Temperature
* NIC
  * FPGA Temperature
* AirIn
  * Ambient Air In temperature. Sensor located **after** the U.2 SSDs
* Transceiver
  * Temperature of each QSFP Transceiver

