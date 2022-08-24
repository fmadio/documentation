# fmadiocli

fmadiocli is a command line interface using a switch style interface to operate FMADIO Packet Capture devices.

Brief help description is provided using ? command

```
[Wed Apr 27 07:18:20 2022] ?                                                                                : Command Line Help Info
[Wed Apr 27 07:18:20 2022] config                                                                           : enter configuration mode
[Wed Apr 27 07:18:20 2022] config analytics schedule del <schedule row name>                                : Deletes a analtyics scheduler row
[Wed Apr 27 07:18:20 2022] config analytics schedule mod <schedule row name>                                : configures the analytics scheduler
[Wed Apr 27 07:18:20 2022] config analytics schedule new                                                    : Creates a new analytics scheduler new
[Wed Apr 27 07:18:20 2022] config capture schedule del <schedule row name>                                  : Deletes a capture scheduler row
[Wed Apr 27 07:18:20 2022] config capture schedule mod <schedule row name>                                  : configures the capture scheduler
[Wed Apr 27 07:18:20 2022] config capture schedule new                                                      : Creates a new capture scheduler row
[Wed Apr 27 07:18:20 2022] config capture start <capture name>                                              : Starts a capture
[Wed Apr 27 07:18:20 2022] config capture stop                                                              : Stops a capture
[Wed Apr 27 07:18:20 2022] config interface dns <interface name> <dns>                                      : sets the interfaces dns
[Wed Apr 27 07:18:20 2022] config interface gateway <interface name> <gateway>                              : sets the interfaces gateway
[Wed Apr 27 07:18:20 2022] config interface ip <interface name> <ip address>                                : sets the interfaces IP address
[Wed Apr 27 07:18:20 2022] config interface mode <interface name> <disabled|static>                         : sets the interfaces IP mode static or disabled
[Wed Apr 27 07:18:20 2022] config interface netmask <interface name> <netmask>                              : sets the interfaces netmask
.
.
.

```

Verbose help description is provided using ??? command

```
[Wed Apr 27 07:18:56 2022] > ???
[Wed Apr 27 07:18:56 2022] =====================================================================================
[Wed Apr 27 07:18:56 2022] ?
[Wed Apr 27 07:18:56 2022] -------------------------------------------------------------------------------------
[Wed Apr 27 07:18:56 2022] Command Line Help Info
[Wed Apr 27 07:18:56 2022]
[Wed Apr 27 07:18:56 2022] =====================================================================================
[Wed Apr 27 07:18:56 2022] config
[Wed Apr 27 07:18:56 2022] -------------------------------------------------------------------------------------
[Wed Apr 27 07:18:56 2022] enter configuration mode
[Wed Apr 27 07:18:56 2022]
[Wed Apr 27 07:18:56 2022] =====================================================================================
[Wed Apr 27 07:18:56 2022] config analytics schedule del
[Wed Apr 27 07:18:56 2022] -------------------------------------------------------------------------------------
[Wed Apr 27 07:18:56 2022] Deletes a analtyics scheduler row
[Wed Apr 27 07:18:56 2022]
[Wed Apr 27 07:18:56 2022] Example Usage:
[Wed Apr 27 07:18:56 2022] > config analytics schedule del <name>                 : deletes analytics scheduler entry
[Wed Apr 27 07:18:56 2022]
[Wed Apr 27 07:18:56 2022]
.
.
.

```

### Command History

Command history is stored in the file

```
/opt/fmadio/etc/fmadiocli.history
```

It can be deleted to clear the history

## Configure Interfaces

### config interface shutdown

FW: 7856+  support for 100Gv2 2x100G 2x40G

This shuts down a specific capture interface as specified, usually this is cap0 or cap1 and depends on the SKU and Port configuration on which ports can be shutdown

```
[Wed Apr 27 07:13:15 2022] > config interface shutdown cap0
[Wed Apr 27 07:13:15 2022]     Disable cycle calibration
[Wed Apr 27 07:13:15 2022]     PCIVersion: 50434930 50434930
[Wed Apr 27 07:13:15 2022]     PortMask:1 0 0 0  0 0 0 0
[Wed Apr 27 07:13:15 2022]     2x100G pcs shutdown 1
[Wed Apr 27 07:13:15 2022] set interface [cap0] mode () -> (disabled)
[Wed Apr 27 07:13:15 2022]
[Wed Apr 27 07:13:21 2022] > 
```

### config interface no shutdown

FW: 7856+  support for 100Gv2 2x100G 2x40G

Re-enables the specified capture interface from shutdown status. Depending on the link peer, the link peer might need to be bounced as it may be in a shutdown error state.

```
[Wed Apr 27 07:13:21 2022] > config interface no shutdown cap0
[Wed Apr 27 07:13:21 2022]
[Wed Apr 27 07:13:21 2022]     Disable cycle calibration
[Wed Apr 27 07:13:21 2022]     PCIVersion: 50434930 50434930
[Wed Apr 27 07:13:21 2022]     PortMask:1 0 0 0  0 0 0 0
[Wed Apr 27 07:13:21 2022]     2x100G pcs shutdown 0
[Wed Apr 27 07:13:21 2022] set interface [cap0] mode (disabled) -> ()
[Wed Apr 27 07:13:21 2022]
[Wed Apr 27 07:13:21 2022] >

```

```
```

### config interface fec

FW: 8224+ ( 2x100G only)

This forces FEC on the specific capture port. FEC Autoneg is disabled. This setting is persistent across reboots.

```
Wed Aug 24 05:48:53 2022] > config interface fec cap0
[Wed Aug 24 05:48:53 2022]     Disable cycle calibration
[Wed Aug 24 05:48:53 2022]     FEC Force
[Wed Aug 24 05:48:53 2022]     FECENable: 1 PortMask:0001
[Wed Aug 24 05:48:53 2022]     [0] FECEnable: 1 FECForce:1
[Wed Aug 24 05:48:53 2022]     [1] FECEnable: 1 FECForce:1
[Wed Aug 24 05:48:53 2022] set interface [cap0] fec (true) -> (true)
[Wed Aug 24 05:48:53 2022]
[Wed Aug 24 05:48:53 2022] >

```

### config interface no fec

FW: 8224+ ( 2x100G only)

This disables the forced FEC setting where the system will try autoneg if FEC is enabled or not. Setting is persistent across reboots.

```
[Wed Aug 24 05:50:39 2022] > config interface no fec cap0
[Wed Aug 24 05:50:39 2022]     Disable cycle calibration
[Wed Aug 24 05:50:39 2022]     no FEC Force
[Wed Aug 24 05:50:39 2022]     FECENable: 0 PortMask:0001
[Wed Aug 24 05:50:39 2022]     [0] FECEnable: 1 FECForce:0
[Wed Aug 24 05:50:39 2022]     [1] FECEnable: 1 FECForce:1
[Wed Aug 24 05:50:39 2022] set interface [cap0] fec (true) -> (false)
[Wed Aug 24 05:50:39 2022]
[Wed Aug 24 05:50:39 2022] >

```

### show interface status

FW: 7856+

Shows the current state of the interfaces

```
[Wed Apr 27 07:16:43 2022] > show interface status
[Wed Apr 27 07:16:43 2022] Port       Description Status          Speed         Transciver    RxPower    TxPower    Temperature      FEC     Vendor            Vendor PN
[Wed Apr 27 07:16:43 2022] ------------------------------------------------------------------------------------------------------------------------------------
[Wed Apr 27 07:16:43 2022] man0                  connected       1G
[Wed Apr 27 07:16:43 2022] man10                 connected       40G         40G Base-SR4  0.7565 mW  0.0000 mW        36.21 C               AVAGO          AFBR-79EQDZ
[Wed Apr 27 07:16:43 2022] cap0                  notconnected    100G             100G CR   0.000 mW   0.000 mA        0.000 C          Ar Networks             Q28-PC01
[Wed Apr 27 07:16:43 2022] cap1                  connected       100G             100G CR   0.000 mW   0.000 mA        0.000 C          Ar Networks             Q28-PC01
[Wed Apr 27 07:16:43 2022] >

```

## Automatic Push PCAP

### show push-pcap

FW: 7963+

Shows the currently configured automatic push pcaps

```
[Thu Jun  9 08:30:42 2022] > show push-pcap
[Thu Jun  9 08:30:42 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Thu Jun  9 08:30:42 2022] FollowStart : true
[Thu Jun  9 08:30:42 2022] Decap       : true
[Thu Jun  9 08:30:42 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Thu Jun  9 08:30:42 2022] [tcp] #1
[Thu Jun  9 08:30:42 2022]   Mode        : File
[Thu Jun  9 08:30:42 2022]   Path        : /mnt/remote0/push/ttest/
[Thu Jun  9 08:30:42 2022]   Split       : --split-time 3600e9
[Thu Jun  9 08:30:42 2022]   FileName    : --filename-tstr-HHMMSS
[Thu Jun  9 08:30:42 2022]   FilterBPF   : host 192.168.1.1
[Thu Jun  9 08:30:42 2022]   FilterFrame :
[Thu Jun  9 08:30:42 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Thu Jun  9 08:30:42 2022] >

```

### config push-pcap new \<push target>

FW: 7963+

Creates a new push-pcap target called \<push target>

NOTE: all target names should be unique

```
[Thu Jun  9 08:32:13 2022] > config push-pcap new udp-all
[Thu Jun  9 08:32:13 2022] Add Push PCAP target [udp-all]
[Thu Jun  9 08:32:13 2022] >

```

### config push-pcap del \<push target>

Deletes the current push pcap entry name \<push target>

```
[Thu Jun  9 08:33:17 2022] > config push-pcap del udp-all
[Thu Jun  9 08:33:18 2022] deleting: [udp-all] row 2
[Thu Jun  9 08:33:18 2022] >
```

### config push-pcap mod \<push target> name \<new name>

Renames the specified \<push target> entry with an updated one \<new name>

```
[Thu Jun  9 08:36:21 2022] > config push-pcap mod udp-all name udp-port-1900
[Thu Jun  9 08:36:23 2022] Rename [udp-all] -> [udp-port-1900]
[Thu Jun  9 08:36:23 2022] >

```

### config push-pcap mod \<push target> path \<new write path>

Updates the push write path to the specified \<new write path>. Typically this is the NFS remote path or rclone write path.

```
[Thu Jun  9 08:41:41 2022] > config push-pcap mod udp-port-1900 path /mnt/remote0/push/
[Thu Jun  9 08:41:44 2022] Set Path [] -> [/mnt/remote0/push/]
[Thu Jun  9 08:41:44 2022] >
```

### config push-pcap mod \<push target> split-time \<value>

Configure PCAPs to be split by the specified time value. By default \<value> is scientific notation in nanoseconds. In addition s (seconds) m (minutes) h (hours) suffix can be used also

|     | Description                     |
| --- | ------------------------------- |
| 1e9 | 1 second in scientific notation |
| 60s | 60 seconds                      |
| 1m  | 1 minute                        |
| 1h  | 1 hour                          |

Example configure to split every 1 minute

```
[Thu Jun  9 08:48:05 2022] > config push-pcap mod udp-port-1900 split-time 1m
[Thu Jun  9 08:48:06 2022] Set Split to  [--split-time 3600e9] -> [--split-time 60000000000]
[Thu Jun  9 08:48:06 2022] >

```

### config push-pcap mod \<push target> split-size \<value>

Configure PCAPs to be split by total byte size \<value>

|      | Description                                 |   |
| ---- | ------------------------------------------- | - |
| 1e9  | 1 Gigabyte specified in scientific notation |   |
| 100M | 100 Megbyte                                 |   |
| 5G   | 5 Gigabyte                                  |   |

Example below shows splitting on 1GB boundaries

```
[Thu Jun  9 08:53:32 2022] > config push-pcap mod udp-port-1900 split-size 1G
[Thu Jun  9 08:53:33 2022] Set Split to  [--split-time 60000000000] -> [--split-byte 1000000000]
[Thu Jun  9 08:53:33 2022] >

```

### config push-pcap mod \<push target> filename \<value>

Specifies the filename format for each individual split PCAP

| Value              | Example                           | Description              |
| ------------------ | --------------------------------- | ------------------------ |
| epoch-sec          | \_1654610221.pcap                 | Second Epoch             |
| epoch-sec-startend | \_1654610221-1654620221.pcap      | Epoch start and End      |
| epoch-msec         | \_1654610221012.pcap              | Epoch in msec            |
| epoch-usec         | \_1654610221012345.pcap           | Epoch is micro sec       |
| epoch-nsec         | \_1654610221012345678.pcap        | Epoch in Nano sec        |
| HHMM               | \_20200101\_1201.pcap             | Hour Min                 |
| HHMMSS             | \_20200101\_120159.pcap           | Hour Min Sec             |
| HHMMSS\_TZ         | 2020-01-01\_12:01:59+09:00.pcap   | House Min Sec + Timezone |
| HHMMSS\_NS         | \_20200101\_120159.012345678.pcap | House Min Sec Nanos      |

Example uses a simple Hour Min Sec format

```
[Thu Jun  9 09:02:52 2022] > config push-pcap mod udp-port-1900 filename HHMMSS
[Thu Jun  9 09:02:53 2022] Set Filename to  [--filename-tstr-HHMMSS_TZ] -> [--filename-tstr-HHMMSS]
[Thu Jun  9 09:02:53 2022] >

```

### config push-pcap mod \<push target> filter-bpf "\<bpf filter>"

Sets the specified push with a BPF filter.&#x20;

**NOTE: the BPF filter must be enclosed in double quotes**

Example sets for udp and port 1900

```
[Thu Jun  9 09:07:05 2022] > config push-pcap mod udp-port-1900 filter-bpf "udp and port 1900"
[Thu Jun  9 09:07:05 2022] Set FilterBPF [] -> [udp and port 1900]
[Thu Jun  9 09:07:05 2022] >

```
