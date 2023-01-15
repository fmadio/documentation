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

### show interface status

FW: 7856+

Shows the current state of the interfaces

```
show interface status
```

Example below shows port status on an FMADIO100Gv2 Analytics system

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

### show interface ip

FW: 8336+

Shows the currently configured IP address information for the management and BMC/IPMI/Capture ports

```
show interface ip
```

Example below shows the status on an FMADIO20Gv3 system

```
[Tue Dec 13 04:18:36 2022] > show interface ip
[Tue Dec 13 04:18:37 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:18:37 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:18:38 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:18:38 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:18:38 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:18:38 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:18:38 2022] cap0
[Tue Dec 13 04:18:38 2022] cap1
[Tue Dec 13 04:18:38 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:18:38 2022] >
```

### show interface counter

Shows RMON1 counter information on each capture port.

```
show interface counter
```

Example below is FMADIO20Gv3 system output

```
[Tue Dec 13 04:40:01 2022] > show interface counter
[Tue Dec 13 04:40:01 2022] Port                         Packet             Byte
[Tue Dec 13 04:40:01 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:01 2022]   cap0     Total :                0                0
[Tue Dec 13 04:40:01 2022]            Under :                0                0
[Tue Dec 13 04:40:01 2022]               64 :                0                0
[Tue Dec 13 04:40:01 2022]           65-127 :                0                0
[Tue Dec 13 04:40:01 2022]          128-255 :                0                0
[Tue Dec 13 04:40:01 2022]          256-511 :                0                0
[Tue Dec 13 04:40:01 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:01 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:01 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:01 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:01 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:01 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:01 2022]             Over :                0                0
[Tue Dec 13 04:40:01 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:01 2022]   cap1     Total :           303349        125359723
[Tue Dec 13 04:40:01 2022]            Under :                0                0
[Tue Dec 13 04:40:01 2022]               64 :            58765                0
[Tue Dec 13 04:40:01 2022]           65-127 :           146476                0
[Tue Dec 13 04:40:01 2022]          128-255 :            11891                0
[Tue Dec 13 04:40:01 2022]          256-511 :             8283                0
[Tue Dec 13 04:40:01 2022]         512-1023 :            15793                0
[Tue Dec 13 04:40:01 2022]        1024-1518 :            62140                0
[Tue Dec 13 04:40:01 2022]        1024-2047 :            62141                0
[Tue Dec 13 04:40:01 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:01 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:01 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:01 2022]             Over :                0                0
[Tue Dec 13 04:40:01 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:01 2022] >

```

Example below shows FMADIO100v2 in 8x10G port output

```
[Tue Dec 13 04:40:57 2022] > show interface counter
[Tue Dec 13 04:40:58 2022] Port                         Packet             Byte
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap0     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap1     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap2     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap3     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap4     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap5     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap6     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------
[Tue Dec 13 04:40:58 2022]   cap7     Total :                0                0
[Tue Dec 13 04:40:58 2022]            Under :                0                0
[Tue Dec 13 04:40:58 2022]               64 :                0                0
[Tue Dec 13 04:40:58 2022]           65-127 :                0                0
[Tue Dec 13 04:40:58 2022]          128-255 :                0                0
[Tue Dec 13 04:40:58 2022]          256-511 :                0                0
[Tue Dec 13 04:40:58 2022]         512-1023 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0
[Tue Dec 13 04:40:58 2022]        1024-2047 :                0                0
[Tue Dec 13 04:40:58 2022]        2048_4095 :                0                0
[Tue Dec 13 04:40:58 2022]        4096_8191 :                0                0
[Tue Dec 13 04:40:58 2022]        8192_9216 :                0                0
[Tue Dec 13 04:40:58 2022]             Over :                0                0
[Tue Dec 13 04:40:58 2022] -----------------+----------------------------------------------------------

```

### config interface shutdown

FW: 7856+  support for 100Gv2 2x100G 2x40G

This shuts down a specific capture interface as specified, usually this is cap0 or cap1 and depends on the SKU and Port configuration on which ports can be shutdown

```
config interface shutdown <interface>
```

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
config interface no shutdown <interface>
```

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

### config interface fec

FW: 8224+ ( 2x100G only)

This forces FEC on the specific capture port. FEC Autoneg is disabled. This setting is persistent across reboots.

```
config interface fec <interface>
```

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
config interface no fec <interface>
```

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

### config interface ip

Configures the IP address of the specified network port. Typically this is used for setting the management/BMC IP address of the system

```
config interface ip <interface name> <IPv4 address>
```

Example below sets the man0 port to IP address 192.168.187.10. The exact output may vary between SKUs

```
config interface ip man0 192.168.187.10
```

```
[Tue Dec 13 04:22:47 2022] > config interface ip man0 192.168.187.10
[Tue Dec 13 04:22:48 2022] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
.
.
<snip>
.
.
[Tue Dec 13 04:22:51 2022] set interface [man0] ip (192.168.187.10) -> (192.168.187.10)
[Tue Dec 13 04:22:51 2022]
[Tue Dec 13 04:22:51 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:22:51 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:22:53 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:22:53 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:22:53 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:22:53 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:22:53 2022] cap0
[Tue Dec 13 04:22:53 2022] cap1
[Tue Dec 13 04:22:53 2022] ---------------------------------------------------------------------------------------------------------------------------------

```

Example below shows setting the IP address of the BMC/IPMI port

```
config interface ip bmc 192.168.187.2
```

```
[Tue Dec 13 04:24:49 2022] > config interface ip bmc 192.168.187.2
[Tue Dec 13 04:24:50 2022] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal --updatebmc
.
.
<snip>
.
.
[Tue Dec 13 04:26:00 2022] set interface [bmc] ip (192.168.187.2) -> (192.168.187.2)
[Tue Dec 13 04:26:00 2022]
[Tue Dec 13 04:26:00 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:26:00 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:26:04 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:26:04 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:26:04 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:26:04 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:26:04 2022] cap0
[Tue Dec 13 04:26:04 2022] cap1
[Tue Dec 13 04:26:04 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:26:04 2022] >

```

### config interface netmask

Sets the netmask of the specified interface

```
config interface netmask man0 255.255.255.0
```

<pre><code>[Tue Dec 13 04:28:02 2022] > config interface netmask man0 255.255.255.0
[Tue Dec 13 04:28:03 2022] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
<strong>.
</strong><strong>.
</strong><strong>&#x3C;snip>
</strong><strong>.
</strong><strong>.
</strong>[Tue Dec 13 04:28:06 2022] set interface [man0] netmask (255.255.255.0) -> (255.255.255.0)
[Tue Dec 13 04:28:06 2022]
[Tue Dec 13 04:28:06 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:28:06 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:28:07 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:28:07 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:28:07 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:28:07 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:28:07 2022] cap0
[Tue Dec 13 04:28:07 2022] cap1
[Tue Dec 13 04:28:07 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:28:07 2022] >

</code></pre>

### config interface gateway

Sets the default gateway for the specified interface

```
config interface gateway <interface> <IPv4 gateway address>
```

Example below sets the man0 management interfaces default gateway address

```
[Tue Dec 13 04:33:11 2022] > config interface gateway man0 192.168.187.30
[Tue Dec 13 04:33:11 2022] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
.
.
<snip>
.
.
[Tue Dec 13 04:33:14 2022] set interface [man0] Gateway (192.168.187.30) -> (192.168.187.30)
[Tue Dec 13 04:33:14 2022]
[Tue Dec 13 04:33:14 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:33:14 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:33:17 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:33:17 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:33:17 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:33:17 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:33:17 2022] cap0
[Tue Dec 13 04:33:17 2022] cap1
[Tue Dec 13 04:33:17 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:33:17 2022] >
```

### config interface dns

Sets the DNS server for the specified interface

```
config interface <interface> <IPv4 DNS address>
```

Example below sets the DNS server for man0 interface to be 1.1.1.1

```
config interface dns man0 1.1.1.1
```

```
[Tue Dec 13 04:35:33 2022] > config interface dns man0 1.1.1.1
[Tue Dec 13 04:35:34 2022] UPDATING: sudo /opt/fmadio/bin/setup_network.lua --nocal
.
.
<snip>
.
.
[Tue Dec 13 04:35:37 2022] set interface [man0] DNS (nil) -> (1.1.1.1)
[Tue Dec 13 04:35:37 2022]
[Tue Dec 13 04:35:37 2022] Port       Mode       IP              Netmask         Gateway         DNS0            DNS1
[Tue Dec 13 04:35:37 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:35:40 2022] bmc        static     192.168.187.2   255.255.255.0   192.168.187.30
[Tue Dec 13 04:35:40 2022] man0       static     192.168.187.10  255.255.255.0   192.168.187.30  1.1.1.1
[Tue Dec 13 04:35:40 2022] man1       disabled   192.168.1.2     255.255.255.0   192.168.1.1
[Tue Dec 13 04:35:40 2022] man10      static     192.168.91.50   255.255.255.0
[Tue Dec 13 04:35:40 2022] cap0
[Tue Dec 13 04:35:40 2022] cap1
[Tue Dec 13 04:35:40 2022] ---------------------------------------------------------------------------------------------------------------------------------
[Tue Dec 13 04:35:40 2022] >

```

## Configure Capture

### show capture status

Shows the current capture status

```
Sun Jan 15 11:21:17 2023] > show capture status
[Sun Jan 15 11:21:17 2023]
[Sun Jan 15 11:21:17 2023] Current Capture Status
[Sun Jan 15 11:21:17 2023] ------------------+--------------------
[Sun Jan 15 11:21:17 2023] Capture Running   | true
[Sun Jan 15 11:21:17 2023] Capture Name      | asdf_20230115_1041
[Sun Jan 15 11:21:17 2023] Capture Bytes     |                0
[Sun Jan 15 11:21:17 2023] Capture Packets   |                0
[Sun Jan 15 11:21:17 2023] Capture Drop      |                0
[Sun Jan 15 11:21:17 2023] Capture FCS Error |                0
[Sun Jan 15 11:21:17 2023] Capture Rate      |     0.000000 Gbps
[Sun Jan 15 11:21:17 2023]                   |     0.000000 MPps
[Sun Jan 15 11:21:17 2023] Capture Start     |
[Sun Jan 15 11:21:17 2023] Capture Duration  |
[Sun Jan 15 11:21:17 2023] ------------------+--------------------
[Sun Jan 15 11:21:17 2023] >

```

### show capture schedule

Shows the current capture schedule

```
[Sun Jan 15 11:22:07 2023] > show capture schedule
[Sun Jan 15 11:22:07 2023]
[Sun Jan 15 11:22:07 2023] Scheduled Capture Status
[Sun Jan 15 11:22:07 2023]
[Sun Jan 15 11:22:07 2023]                                          |   24/7 |  Start   |   Stop   |Mon|Tue|Wed|Thu|Fri|Sat|Sun|
[Sun Jan 15 11:22:07 2023] -----------------------------------------+--------+----------+----------+---+---+---+----+--+---+---+
[Sun Jan 15 11:22:07 2023] wan_colo0                                |  false | 00:00:00 | 24:00:00 |   |   |   |   |   |   |   |
[Sun Jan 15 11:22:07 2023] -----------------------------------------+--------+----------+----------+---+---+---+----+--+---+---+
[Sun Jan 15 11:22:07 2023] >
```

### show capture list

Displays list of all captures on the system

```
[Sun Jan 15 11:23:21 2023] > show capture list
[Sun Jan 15 11:23:21 2023] Showing captures
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221230_0000]          1310720 B (Wed . 18:04:08 . 28-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221228_0000]      29613621248 B (Wed . 18:03:58 . 28-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221227_0000]       3442999296 B (Tue . 23:59:35 . 27-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221226_0000]      16513236992 B (Mon . 23:59:38 . 26-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221225_0000]       1869873152 B (Sun . 23:59:42 . 25-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221224_0000]       6031671296 B (Sat . 23:59:39 . 24-12-2022)
[Sun Jan 15 11:23:21 2023] [wan_colo0_20221223_0000]      33109311488 B (Fri . 23:59:36 . 23-12-2022)
[Sun Jan 15 11:23:21 2023] >
```

### show capture roll

FW: 8367+

Shows the current capture roll setting

```
[Sun Jan 15 11:25:18 2023] > show capture roll
[Sun Jan 15 11:25:18 2023] Capture Roll Setting:
[Sun Jan 15 11:25:18 2023]    Roll every 1.00 Hour
[Sun Jan 15 11:25:18 2023] >
```

### show capture flush

FW: 8367+

Shows the current capture flushing behaviour

```
[Sun Jan 15 11:25:55 2023] > show capture flush
[Sun Jan 15 11:25:56 2023] Capture Flush Setting:
[Sun Jan 15 11:25:56 2023]    Periodic Flush: 10 Sec
[Sun Jan 15 11:25:56 2023] >
```

### config capture flush

FW: 8367+

Sets the capture flushing behavior.&#x20;

Default setting is flush 1sec after capture is idle

Flush always every 1 second. NOTE: 1sec is very aggressive mode and will consume additional storage. However it does provide low latency when watching low bandwidth captures.

```
// Some code
[Sun Jan 15 11:27:34 2023] > config capture flush period 1
[Sun Jan 15 11:27:35 2023] Setting Flush Mode[period] Timeout 1 sec
[Sun Jan 15 11:27:35 2023]
[Sun Jan 15 11:27:35 2023] **** requires restarting of capture to take effect ****
[Sun Jan 15 11:27:35 2023]
```

Flush when capture is idle for >= 1sec

```
[Sun Jan 15 11:27:44 2023] > config capture flush idle 10
[Sun Jan 15 11:27:44 2023] Setting Flush Mode[idle] Timeout 10 sec
[Sun Jan 15 11:27:44 2023]
[Sun Jan 15 11:27:44 2023] **** requires restarting of capture to take effect ****
[Sun Jan 15 11:27:44 2023]
[Sun Jan 15 11:27:44 2023] >
```

### config capture roll

FW: 8367+

Configures the capture rolling behavior. Default (0) is roll at midnight.

Example configures capture to roll every 1 hour.

```
[Sun Jan 15 11:31:46 2023] > config capture roll 1
[Sun Jan 15 11:31:47 2023] Setting Capture Roll Every 1 Hour
[Sun Jan 15 11:31:47 2023]
[Sun Jan 15 11:31:47 2023] **** requires restarting of capture to take effect ****
[Sun Jan 15 11:31:47 2023]
[Sun Jan 15 11:31:47 2023] >
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

<table><thead><tr><th></th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>1e9</td><td>1 Gigabyte specified in scientific notation</td><td></td></tr><tr><td>100M</td><td>100 Megbyte</td><td></td></tr><tr><td>5G</td><td>5 Gigabyte</td><td></td></tr></tbody></table>

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

## User Management

**FW: 8336+**

FMADIO Web GUI supports multiple users with 2 levels of access

| Permission | Description                                                          |
| ---------- | -------------------------------------------------------------------- |
| full       | Provides full admin level access to all functions                    |
| user       | User level only, no ability to modify config and start/stop captures |

Using fmadiocli to setup and configure is shown below

### show userlist

This shows the currently configured list of users on the system

```
show userlist
```

Example output, it shows 2 users fmadio (full access), bob (user access)

```
[Tue Dec 13 04:05:54 2022] > show userlist
[Tue Dec 13 04:05:55 2022] Showing User List
[Tue Dec 13 04:05:55 2022]
[Tue Dec 13 04:05:55 2022] UserList Enable: true
[Tue Dec 13 04:05:55 2022]
[Tue Dec 13 04:05:55 2022] --------------------------------------------------
[Tue Dec 13 04:05:55 2022]
[Tue Dec 13 04:05:55 2022]   UserName   : fmadio
[Tue Dec 13 04:05:55 2022]   Permission : full
[Tue Dec 13 04:05:55 2022]   SecBPF     :
[Tue Dec 13 04:05:55 2022]
[Tue Dec 13 04:05:55 2022]   UserName   : bob
[Tue Dec 13 04:05:55 2022]   Permission : user
[Tue Dec 13 04:05:55 2022]   SecBPF     :
[Tue Dec 13 04:05:55 2022]
[Tue Dec 13 04:05:55 2022] --------------------------------------------------
[Tue Dec 13 04:05:55 2022] >

```

### config userlist add&#x20;

Adds a new user with default permissions and no password

```
config userlist add <username>
```

Example adds the username "bob" to the system

```
[Tue Dec 13 04:08:14 2022] > config userlist add bob
[Tue Dec 13 04:08:17 2022] Created new User [bob]
[Tue Dec 13 04:08:17 2022] >
```

### config userlist del

Deletes the specified username

```
config userlist del <username>
```

Example below deletes the username "bob"

```
[Tue Dec 13 04:09:15 2022] > config userlist del bob
[Tue Dec 13 04:09:16 2022] deleted username [bob]
[Tue Dec 13 04:09:16 2022] >
```

### config userlist password

Sets the WEB user password. This has no effect on SSH access to the system

```
config userlist password <username>
```

Example below sets the web password for user "bob"

```
[Tue Dec 13 04:12:19 2022] > config userlist password bob
[Tue Dec 13 04:12:20 2022] New Password     : ***********
[Tue Dec 13 04:12:22 2022] Re-enter Password: ***********
[Tue Dec 13 04:12:24 2022] web password for username [bob] set
[Tue Dec 13 04:12:24 2022] >
```

### config userlist permission

Sets the userlevel permission for the specified username

```
config userlist permission <username> <level>
```

Level types are 2

* full  - provides full unrestricted GUI access
* user - provides download and analysis only access (no configuration or capture state change)&#x20;

Example below shows setting the username "bob" to be a "user" level (e.g. can not change system configuration or capture states)

```
[Tue Dec 13 04:13:55 2022] > config userlist permission bob user
[Tue Dec 13 04:13:55 2022] modified username [bob] to permission level [user]
[Tue Dec 13 04:13:55 2022] >

```

Example below shows setting username "bob" to be a full access user (e.g. can change any configuration using the GUI)

```
[Tue Dec 13 04:14:42 2022] > config userlist permission bob full
[Tue Dec 13 04:14:42 2022] modified username [bob] to permission level [full]
[Tue Dec 13 04:14:42 2022] >
```

