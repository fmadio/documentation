# Logfile Report \(CLI\)

Sometimes GUI logfile generation is more difficult than a CLI version. As the GUI calls a script on the system to generate the logfile report, SSHing into the fmadio packet capture system and running the script directly is possible. The command to run in as follows



```text
fmadio@fmadio20-049:/mnt/store0/upload$ sudo /opt/fmadio/bin/syslog_report.lua
```



```text
fmadio@fmadio20-049:/mnt/store0/upload$ sudo /opt/fmadio/bin/syslog_report.lua
fmad fmadlua Dec 22 2015
calibrating...
0 : 00000000d09dad48           3.5000 cycles/nsec
Cycles/Sec 3499994440.0000 Std:       0cycle std(  0.00000000)
loading filename [/opt/fmadio/bin/syslog_report.lua]
Cmd [/opt/fmadio/bin/system_dump.lua > /mnt/store0/log/system_dump_20151229_132103]
loading filename [/opt/fmadio/bin/system_dump.lua]
[       iosched_direct.stdouterr_20151229]      1283855            1 MB
[            iosched_direct_20151229_1205]      1365723            2 MB
[               monitor_gps_20151229_1205]      9834318           12 MB
[            monitor_memory_20151229_1205]       809724           13 MB
[               monitor_nic_20151229_1205]      1179945           14 MB
[      statusqueue_20151229_132103.tar.gz]        40916           14 MB
[       stream_capture_sf20_20151229_1205]       288414           14 MB
[               monitor_cpu_20151229_1205]       642415           15 MB
[                 scheduler_20151229_1205]       404614           15 MB
[                             sfptp_stats]      3276884           19 MB
[     stream_writeback.stdouterr_20151229]       973105           20 MB
[          stream_writeback_20151229_1205]      1054488           21 MB
[             system_dump_20151229_132103]      1089180           22 MB
[      monitor_ptp.lua.stdouterr_20151229]        22197           22 MB
[               monitor_ptp_20151229_1205]       676222           23 MB
[        analytics.lua.stdouterr_20151229]        30954           23 MB
.
.
.
.
.
.
```

After the report has completed, the final log file is located at

```text
/mnt/store0/upload/report_*.tar.gz
```

Please scp off the device and send to support,

