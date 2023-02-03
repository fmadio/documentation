# Syslog

#### Requires FW:6761+

In large server deployments using remote syslogd where syslog entries are written over UDP is quite helpful. This allows a central server to monitor a fleet of servers by receiving all log entries over the network. This is a standard linux feature set. FMADIO Packet Capture devices support this feature, as follows:

Copy the default syslogd.conf to /opt/fmadio/etc/

```
sudo cp /etc/syslogd.conf /opt/fmadio/etc/
```

Then edit the file as follows, replacing the destination IP with configuration specific to your environment

```yaml
# rsyslog configuration file (fmadio default)

#### MODULES ####

module(load="imuxsock") # provides support for local system logging (e.g. via logger command)
module(load="imklog")   # provides kernel logging support (previously done by rklogd)
module(load="immark")  # provides --MARK-- message capability

# Provides UDP syslog reception
# for parameters see https://www.rsyslog.com/doc/imudp.html
#module(load="imudp") # needs to be done just once
#input(type="imudp" port="514")

# Provides TCP syslog reception
# for parameters see https://www.rsyslog.com/doc/imtcp.html
#module(load="imtcp") # needs to be done just once
#input(type="imtcp" port="514")


#### GLOBAL DIRECTIVES ####

template(name="facility_priority" type="list") {
        property(name="syslogfacility-text")
        constant(value=".")
        property(name="syslogpriority-text")
}
set $!facility_priority = exec_template("facility_priority");


template(name="syslog_fmadio" type="list") {
        property(name="timereported" dateFormat="year")
        constant(value=".")
        property(name="timereported" dateFormat="month")
        constant(value=".")
        property(name="timereported" dateFormat="day")
        constant(value="-")
        property(name="timereported" dateFormat="hour")
        constant(value=":")
        property(name="timereported" dateFormat="minute")
        constant(value=":")
        property(name="timereported" dateFormat="second")
        constant(value=".")
        property(name="timereported" dateFormat="subseconds")
        constant(value=" ")
        constant(value="(")
        property(name="timereported" dateFormat="tzoffsdirection")
        property(name="timereported" dateFormat="tzoffshour")
        constant(value=":")
        property(name="timereported" dateFormat="tzoffsmin")
        constant(value=") | ")
        property(name="hostname")
        constant(value=" | ")

        property(name="$!facility_priority" position.to="16" fixedwidth="on")
        constant(value="| ")

        property(name="programname" position.to="10" fixedwidth="on")
        constant(value="|")
        property(name="msg" spifno1stsp="on")
        property(name="msg" droplastlf="on")
        constant(value="\n")
}

# Use default timestamp format
#$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
$ActionFileDefaultTemplate syslog_fmadio

# File syncing capability is disabled by default. This feature is usually not required,
# not useful and an extreme performance hit
#$ActionFileEnableSync on

# Include all config files in /etc/rsyslog.d/
#$IncludeConfig /etc/rsyslog.d/*.conf


#### RULES ####

# Log all kernel messages to the console.
# Logging much else clutters up the screen.
kern.err                                                /dev/console

# log everything to disk
*.*                                                     /mnt/store0/log/messages

# remote host is TCP: name/ip:port, e.g. 192.168.0.1:514, port optional
*.* @@192.168.1.100:514

```

In the above example all syslog log entries are also written to a server at 192.168.1.100 over TCP on port 514.&#x20;

For UDP on port 514 use the following setting

```
// remote host over UDP is name/ip;port
*.* @192.168.1.100:514
```

Its the standard syslogd from inted package additional customization can be done if required. Example syslog output as follows

```yaml
Aug 12 21:06:36 box local7.info fmadio: Capture   (Enb: 0 Pkt: 0 Drop: 0 FCSError: 0 CaptureRateRate 0.00000000 Gbps)
Aug 12 21:06:36 box local7.info fmadio: Mem       (0.00GB ECC 0) Writeback (0.00GB) Dropped (0.00GB)
Aug 12 21:06:46 box local7.info fmadio: Temp      (CPU0:33.00 CPU1:33.00 PCH:41.00 SYS:36.00 PER:24.00 NIC:57.00 AirIn:24.00 AirOut:0.00 Transciver:40.00 41.00)
Aug 12 21:06:46 box local7.info fmadio: Fan       (SYS0:13650 SYS1:13800 SYS2:13800 SYS3:13500 SYS4:13500 SYS5:13500 SYS6:13650 SYS7:13500)
Aug 12 21:06:46 box local7.info fmadio: Disk      OS (Temp:27 ERR: 0 ) SSD (Valid:1 1 1 0 Temp:33 33 32 0 ERR:0 0 0 0 ) HDD (Valid:1 1 1 1 Temp: 30 30 30 29 ERR: 0 0 0 0 )
Aug 12 21:06:46 box local7.info fmadio: Link      Capture (1 1 0 0 0 0 0 0 ) Man (1G 1 10G 0 )
Aug 12 21:06:46 box local7.info fmadio: DiskIO    (Rd:  0.00Gbps Wr:  0.00Gbps)
```
