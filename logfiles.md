# Logfiles

Debugging pcap2json can be tricky, logfiles are key to understanding what has happened.

The key files are as follows all  in the /mnt/store0/log/ directory

```text
fmadio@fmadio100v2-228U:/mnt/store0/log$ ls *pcap2json* -altr

analytics_pcap2json_realtime.cur -> /mnt/store0/log/analytics_pcap2json_realtime_20210517_1119
pcap2json_backend.cur -> /mnt/store0/log/pcap2json_backend_20210723_2131
pcap2json_3.cur -> /mnt/store0/log/pcap2json_3_20210723_2131
pcap2json_2.cur -> /mnt/store0/log/pcap2json_2_20210723_2131
pcap2json_1.cur -> /mnt/store0/log/pcap2json_1_20210723_2131
pcap2json_0.cur -> /mnt/store0/log/pcap2json_0_20210723_2131

fmadio@fmadio100v2-228U:/mnt/store0/log$

```

Troubleshooting ES problems please check

```text
/mnt/store0/log/pcap2json_backend.cur
```

NOTE: These files are symbolic links and change each time pcap2json is run.

