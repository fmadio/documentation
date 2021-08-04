# Global Time Synchronization

FMADIO Packet captures systems have the ability to get &lt; 100nsec global world time synchronization using PTPv2 + PPS signal.

## Pulse Per Second \(PPS\) Clock 

PPS time synchronization is a one pule per second signal, typically from a PTP Grand Master, or GPS based global time master. Its connected over a SMA connector directly to the FMADIO Capture FPGA.

The PPS signal disciplines the start of the global time second, it does not set the actual world time, e.g. its used to set when the start of a second begins.

```text
Pulese 0 | YYYYY-MM-DD HH:MM:DD:(SS + 0) 000.000.000  nanos
Pulese 1 | YYYYY-MM-DD HH:MM:DD:(SS + 1) 000.000.000  nanos
Pulese 2 | YYYYY-MM-DD HH:MM:DD:(SS + 2) 000.000.000  nanos
Pulese 3 | YYYYY-MM-DD HH:MM:DD:(SS + 3) 000.000.000  nanos
Pulese 4 | YYYYY-MM-DD HH:MM:DD:(SS + 4) 000.000.000  nanos
```

FMADIO system uses PTPv2 to set the YYYY-MM-DD HH:MM:SS part of the time.

To confirm PPS signal is active please check the log file

```text
/mnt/store0/log/fnic_clock.cur
```

The following screenshot shows an example PPS signal incrementing every 1 second

![PPS Signal Incrementing](../.gitbook/assets/image%20%2895%29.png)

If the PPS Cnt is not incrementing, there may be a problem with the SMA connection or PPS voltage/pulse specification. Please contact support for further assistance.

