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
[Tue Dec 13 04:40:58 2022]        1024-1518 :                0                0config capture
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
[Tue Dec 13 04:40:58 2022]        1024-1518 :       conf         0                0
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

### config capture start \<name>

Starts a capture  with the specified name

```
[Sat Jan 21 01:39:12 2023] > config capture start test-capture
[Sat Jan 21 01:39:12 2023]
[Sat Jan 21 01:39:12 2023] Starting Quick Capture [test-capture]
[Sat Jan 21 01:39:12 2023] OK: [Sat Jan 21 01:39:12 2023] successfully started capture [test-capture]
[Sat Jan 21 01:39:12 2023] >

```

Use `config capture status` to verify the current state

### config capture stop

Stops the currently active capture

NOTE: This will only stop captures manually started, for scheduled captures please disable the schedule entry to stop the capture

```
[Sat Jan 21 01:40:33 2023] > config capture stop
[Sat Jan 21 01:40:33 2023]
[Sat Jan 21 01:40:33 2023] Stopping Capture
[Sat Jan 21 01:40:33 2023] OK: [Sat Jan 21 01:40:33 2023] successfully stopped capture [test-capture]
[Sat Jan 21 01:40:33 2023] >
```

Use `config capture status` to verify the current state



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

## Configure PCAP Download

### show pcap timestamp

Shows the current PCAP timestamp mode. e.g. from the FMADIO FPGA or extract timing information from a packet broker

Example below shows the PCAP timestamp uses the Arista 7130 (Metamako) footer timestamp

```
show pcap timestamp
```

```
[Mon Jun 12 16:48:09 2023] > show pcap timestamp
[Mon Jun 12 16:48:10 2023] TimeStamp Mode: arista7130 : (Arista 7130 (Metamako))
[Mon Jun 12 16:48:10 2023]
[Mon Jun 12 16:48:10 2023] >

```

### config pcap timestamp \<tsmode>

Configures the default PCAP timestamp mode when downloading PCAP data. this value can be overidden by URI option TSMode.

Supported Timestamp Modes

* nic - FMADIO FPGA timestamp
* arista7130 - Arista 7130 (Metamako) Footer
* arista7150\_overwrite - Arista 7150 Overwite FCS + Keyframes
* arista7150\_insert - Arista 7150 Insert 32bit + Keyframes
* arista7280\_mac48 - Arista 7280 Source MAC 48bit Overwrite
* arista7280\_eth64 - Arista 7280 Ethernet Insert 64bit
* erspanv3 - Cisco ERSPANv3 Encapsulation
* cisco3550 - Cisco 3550 (Exablaze) Footer

Help command

```
[Mon Jun 12 16:50:30 2023] > config pcap timestamp
[Mon Jun 12 16:50:31 2023] Example Usage:
[Mon Jun 12 16:50:31 2023] > config pcap timestamp <mode>                           : configure default PCAP timestamp mode
[Mon Jun 12 16:50:31 2023]                         arista7280_mac48                 : Arista 7280 (Source MAC 48bit)
[Mon Jun 12 16:50:31 2023]                         arista7130                       : Arista 7130 (Metamako)
[Mon Jun 12 16:50:31 2023]                         arista7280_eth64                 : Arista 7280 (Ethernet 64bit)
[Mon Jun 12 16:50:31 2023]                         arista7150_overwrite             : Arista 7150 (Overwrite FCS)
[Mon Jun 12 16:50:31 2023]                         erspanv3                         : CISCO ERSPANv3 Timestamp
[Mon Jun 12 16:50:31 2023]                         arista7150_insert                : Arista 7150 (Insert FCS)
[Mon Jun 12 16:50:31 2023]                         nic                              : Timestamp is the FMADIO FPGAs internal timestamp
[Mon Jun 12 16:50:31 2023]                         cisco3550                        : CISCO 3550 Timestamp (Exablaze)
[Mon Jun 12 16:50:31 2023]
[Mon Jun 12 16:50:31 2023] ERROR:  Unknown Command [config pcap timestamp]
[Mon Jun 12 16:50:31 2023] >

```

Example to set the default behavior to use Arista 7130 footer. It takes 60sec to restart the processes after the setting.

```
config pcap timestamp arista7130
```

```
[Mon Jun 12 16:54:04 2023] > config pcap timestamp arista7130
[Mon Jun 12 16:54:04 2023] TimeStamp Mode set to [arista7130] : (Arista 7130 (Metamako))
[Mon Jun 12 16:54:04 2023]
[Mon Jun 12 16:54:04 2023] Restarting Processes
[Mon Jun 12 16:54:14 2023] Wait 60sec for processes to restart
[Mon Jun 12 16:54:14 2023]       0/60
[Mon Jun 12 16:54:15 2023]       1/60
[Mon Jun 12 16:54:16 2023]       2/60
[Mon Jun 12 16:54:17 2023]       3/60
[Mon Jun 12 16:54:18 2023]       4/60
.
.

[Mon Jun 12 16:55:07 2023]       53/60
[Mon Jun 12 16:55:08 2023]       54/60
[Mon Jun 12 16:55:09 2023]       55/60
[Mon Jun 12 16:55:10 2023]       56/60
[Mon Jun 12 16:55:11 2023]       57/60
[Mon Jun 12 16:55:12 2023]       58/60
[Mon Jun 12 16:55:13 2023]       59/60
[Mon Jun 12 16:55:14 2023]       60/60
[Mon Jun 12 16:55:15 2023] done
[Mon Jun 12 16:55:15 2023] >
```



## Automatic Push PCAP

### show push pcap status

FW: 7963+

Shows the currently configured automatic push pcaps

```
[Thu Jun  9 08:30:42 2022] > show push pcap status
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

### config push pcap add \<push target>

FW: 7963+

Creates a new push pcap target called \<push target>

NOTE: all target names should be unique

```
[Thu Jun  9 08:32:13 2022] > config push pcap add udp-all
[Thu Jun  9 08:32:13 2022] Add Push PCAP target [udp-all]
[Thu Jun  9 08:32:13 2022] >

```

### config push pcap del \<push target>

Deletes the current push pcap entry name \<push target>

```
[Thu Jun  9 08:33:17 2022] > config push pcap del udp-all
[Thu Jun  9 08:33:18 2022] deleting: [udp-all] row 2
[Thu Jun  9 08:33:18 2022] >
```

### config push pcap name \<push target> \<new name>

Renames the specified \<push target> entry with an updated one \<new name>

```
[Thu Jun  9 08:36:21 2022] > config push pcap name udp-all udp-port-1900
[Thu Jun  9 08:36:23 2022] Rename [udp-all] -> [udp-port-1900]
[Thu Jun  9 08:36:23 2022] >

```

### config push pcap path \<push target> \<new write path>

Updates the push write path to the specified \<new write path>. Typically this is the NFS remote path or rclone write path.

```
[Thu Jun  9 08:41:41 2022] > config push pcap path udp-port-1900 /mnt/remote0/push/
[Thu Jun  9 08:41:44 2022] Set Path [] -> [/mnt/remote0/push/]
[Thu Jun  9 08:41:44 2022] >
```

### config push pcap split-time \<push target> \<value>

Configure PCAPs to be split by the specified time value. By default \<value> is scientific notation in nanoseconds. In addition s (seconds) m (minutes) h (hours) suffix can be used also

|     | Description                     |
| --- | ------------------------------- |
| 1e9 | 1 second in scientific notation |
| 60s | 60 seconds                      |
| 1m  | 1 minute                        |
| 1h  | 1 hour                          |

Example configure to split every 1 minute

```
[Thu Jun  9 08:48:05 2022] > config push pcap split-time udp-port-1900 1m
[Thu Jun  9 08:48:06 2022] Set Split to  [--split-time 3600e9] -> [--split-time 60000000000]
[Thu Jun  9 08:48:06 2022] >

```

### config push pcap split-size \<push target> \<value>

Configure PCAPs to be split by total byte size \<value>

<table><thead><tr><th></th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>1e9</td><td>1 Gigabyte specified in scientific notation</td><td></td></tr><tr><td>100M</td><td>100 Megbyte</td><td></td></tr><tr><td>5G</td><td>5 Gigabyte</td><td></td></tr></tbody></table>

Example below shows splitting on 1GB boundaries

```
[Thu Jun  9 08:53:32 2022] > config push pcap split-size udp-port-1900 1G
[Thu Jun  9 08:53:33 2022] Set Split to  [--split-time 60000000000] -> [--split-byte 1000000000]
[Thu Jun  9 08:53:33 2022] >

```

### config push pcap filename \<push target> \<value>

Specifies the filename format for each individual split PCAP

<table><thead><tr><th width="290.37282229965155">Value</th><th width="301.23912015287374">Example</th><th>Description</th></tr></thead><tbody><tr><td>epoch-sec</td><td>_1654610221.pcap</td><td>Second Epoch</td></tr><tr><td>epoch-sec-startend</td><td>_1654610221-1654620221.pcap</td><td>Epoch start and End</td></tr><tr><td>epoch-msec</td><td>_1654610221012.pcap</td><td>Epoch in msec</td></tr><tr><td>epoch-usec</td><td>_1654610221012345.pcap</td><td>Epoch is micro sec</td></tr><tr><td>epoch-nsec</td><td>_1654610221012345678.pcap</td><td>Epoch in Nano sec</td></tr><tr><td>HHMM</td><td>_20200101_1201.pcap</td><td>Hour Min</td></tr><tr><td>HHMMSS</td><td>_20200101_120159.pcap</td><td>Hour Min Sec</td></tr><tr><td>HHMMSS_TZ</td><td>2020-01-01_12:01:59+09:00.pcap</td><td>House Min Sec + Timezone</td></tr><tr><td>HHMMSS_NS</td><td>_20200101_120159.012345678.pcap</td><td>House Min Sec Nanos</td></tr></tbody></table>

Example uses a simple Hour Min Sec format

```
[Thu Jun  9 09:02:52 2022] > config push pcap filename udp-port-1900 HHMMSS
[Thu Jun  9 09:02:53 2022] Set Filename to  [--filename-tstr-HHMMSS_TZ] -> [--filename-tstr-HHMMSS]
[Thu Jun  9 09:02:53 2022] >

```

### config push pcap filter-bpf \<push target> "\<bpf filter>"

Sets the specified push with a BPF filter.&#x20;

**NOTE: the BPF filter must be enclosed in double quotes**

Example sets for udp and port 1900

```
[Thu Jun  9 09:07:05 2022] > config push pcap filter-bpf udp-port-1900 "udp and port 1900"
[Thu Jun  9 09:07:05 2022] Set FilterBPF [] -> [udp and port 1900]
[Thu Jun  9 09:07:05 2022] >
```

## Automatic Push to LXC (Container)

The system can push automatically into a lxc\_ring enabling a container to consume the data. These functions are to add/delete/modify these push functions.

NOTE this requires the push\_lxc analytics script to be running

### show push lxc

Shows the current push lxc targets configured on the system

```
show push lxc
```

Example shown below, indicates a single suricata ring is enabled with a BPF filter to remove all traffic from subnet 192.168.100.0/24

```
[Sat Jun 24 14:22:12 2023] > show push lxc
[Sat Jun 24 14:22:12 2023]
[Sat Jun 24 14:22:12 2023] Ring name                                       : Enable :       From : Description                     : Filter Frame         : Filter BPF
[Sat Jun 24 14:22:12 2023] ------------------------------------------------+--------+------------+---------------------------------+----------------------+-----------------------------------------------------------------
[Sat Jun 24 14:22:12 2023] /opt/fmadio/queue/lxc_ring_suricata             :   true :      start : suricata ids feed               :                      : not net 192.168.100.0/24
[Sat Jun 24 14:22:12 2023] ------------------------------------------------+--------+------------+---------------------------------+----------------------+-----------------------------------------------------------------
[Sat Jun 24 14:22:12 2023] >
```

### config push lxc add \<ring name>

This adds a new LXC push to the ring named \<ring name>.&#x20;

By default the push is disabled when created.

```
config push lxc add <ring name>
```

Example below shows adding a push to the ring named "general"

```
[Sat Jun 24 14:25:26 2023] > config push lxc add general
[Sat Jun 24 14:25:26 2023] New Push LXC target [/opt/fmadio/queue/lxc_ring_general]
[Sat Jun 24 14:25:26 2023] >
```

NOTE this does not create the ring, it only creates the push to the specified ring

### config push lxc del \<ring name>

Removes the specified LXC push target

```
config push lxc del <ring name>
```

Example removes the push lxc target named "general"

```
[Sat Jun 24 14:32:31 2023] > config push lxc del general
[Sat Jun 24 14:32:32 2023] Delete Push LXC target [/opt/fmadio/queue/lxc_ring_general]
[Sat Jun 24 14:32:32 2023] >
```

### config push lxc enable \<ring name>

Enables the specified lxc ring push target. By default when adding a new target the state is disabled.

```
config push lxc enable <ring name>
```

Example enables the push lxc ring target named "general"

```
[Sat Jun 24 14:36:29 2023] > config push lxc enable general
[Sat Jun 24 14:36:29 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] Enable
[Sat Jun 24 14:36:29 2023] >
```

### config push lxc disable \<ring name>

Disables the specified lxc push \<ring name>&#x20;

```
config push lxc disable <ring name>
```

Example disables the lxc ring named "general"

```
[Sat Jun 24 14:39:44 2023] > config push lxc disable general
[Sat Jun 24 14:39:45 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] to Disable
[Sat Jun 24 14:39:45 2023] >
```

### config push lxc filter-bpf \<ring name> "\<filter bpf>"

Adds the specified BPF filter to the LXC push to the ring.&#x20;

NOTE The filter must be enclosed in double quotes ""

```
config push lxc filter-bpf <ring name> "<filter bpf>"
```

Example adds a subnet "192.168.0.0/24" filter to the ring named "general"

```
[Sat Jun 24 14:44:11 2023] > config push lxc filter-bpf general "net 192.168.0.0/24"
[Sat Jun 24 14:44:12 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] filter bpf to (net 192.168.0.0/24)
[Sat Jun 24 14:44:12 2023] >
```

### config push lxc filter-frame \<ring name> "\<filter bpf>"

Adds the specified frame filter to the lxc push to the ring.

NOTE the filter must be enclosed in double quotes ""

```
config push lxc filter-frame <ring name> "<filter frame>"
```

Example add a Frame filter of capture port 0 only to the ring named "general"

```
[Sat Jun 24 14:46:56 2023] > config push lxc filter-frame general "capture.port=0"
[Sat Jun 24 14:46:56 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] filter frame to (capture.port=0)
[Sat Jun 24 14:46:56 2023] >
```

### config push lxc from-now \<ring name>

Sets the push to start from the current capture position into the lxc ring.

This is the default behaviour

```
config push lxc from-now <ring name>
```

Example sets the ring "general" to push data into the ring from now.

```
[Sat Jun 24 14:50:03 2023] > config push lxc from-now general
[Sat Jun 24 14:50:04 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] fetch from current capture position
[Sat Jun 24 14:50:04 2023] >
```

### config push lxc from-start \<ring name>

Sets the push to start from the beginning of the capture.

```
config push lxc from-start <ring name>
```

Example sets the ring "general" to start from the begnining of the capture

```
[Sat Jun 24 14:51:48 2023] > config push lxc from-start general
[Sat Jun 24 14:51:48 2023] Set LXC target [/opt/fmadio/queue/lxc_ring_general] fetch from start of capture
[Sat Jun 24 14:51:48 2023] >
```

## Ring management

Various functions for monitoring the ring status both push pcap and push lxc

### show ring status

Shows all rings status information

```
show ring status
```

Example, this can be helpful for monitoring data is being produced and consumed correctly.

```
[Sat Jun 24 15:23:25 2023] > show ring status
[Sat Jun 24 15:23:25 2023] Name                 : Path                                               :     Status :          Pkt Put :          Pkt Get : Pkt Queued : Desc
[Sat Jun 24 15:23:25 2023] ---------------------+----------------------------------------------------+------------+------------------+------------------+------------+------------------------------------
[Sat Jun 24 15:23:25 2023] suricata             : /opt/fmadio/queue/lxc_ring_suricata                :       full :            1,023 :                0 :       1023 :
[Sat Jun 24 15:23:25 2023] general              : /opt/fmadio/queue/lxc_ring_general                 :     online :                0 :                0 :          0 :
[Sat Jun 24 15:23:25 2023] ---------------------+----------------------------------------------------+------------+------------------+------------------+------------+------------------------------------
[Sat Jun 24 15:23:25 2023] >
```

## Time

Various functions for configuration and monitoring time

### show timezone

Shows the current timezone the system is configured&#x20;

```
show timezone
```

Example showing the current timezone

```
[Sat Jun 24 15:25:29 2023] > show timezone
[Sat Jun 24 15:25:30 2023] Timezone: Asia/Singapore
[Sat Jun 24 15:25:30 2023]     UTC +08:00 (SGT)
[Sat Jun 24 15:25:32 2023] >
```

### config timezone "\<city>"

Configures the timezone by searching the timezone list for the location named "\<city>"

System uses the first found match

For cities with spaces in the name, ensure to use double quotes around the city name

```
config timezone "<city>"
```

Example set the timezone to New York

```
[Sat Jun 24 15:32:00 2023] > config timezone "New York"
[Sat Jun 24 15:32:01 2023] setting timezone to [/usr/share/zoneinfo/America/New_York]
[Sat Jun 24 15:32:01 2023]     UTC -04:00 M (EDT)
[Sat Jun 24 15:32:01 2023] *** System requires a reboot to take effect ****
[Sat Jun 24 15:32:01 2023] >
```

NOTE change only takes effect on next reboot

## User Management

**FW: 8336+**

FMADIO Web GUI supports multiple users with 2 levels of access

<table><thead><tr><th width="243">Permission</th><th>Description</th></tr></thead><tbody><tr><td>full</td><td>Provides full admin level access to all functions</td></tr><tr><td>user</td><td>User level only, no ability to modify config and start/stop captures</td></tr></tbody></table>

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

## Security Management

Various commands to set and modify the security settings of the system

### show security

Shows the current security settings

```
show security
```

```
[Wed May 17 13:56:44 2023] > show security
[Wed May 17 13:56:44 2023] Authentication: PAM-LDAP
[Wed May 17 13:56:44 2023] HTTP Access   : enabled
[Wed May 17 13:56:44 2023] Timeout SSH   : 0.166667min (idle)
[Wed May 17 13:56:44 2023] Timeout WWW   : 1.000000min (session
[Wed May 17 13:56:44 2023] >
```

### config security auth&#x20;

This sets the authentication method of the system. Number of options as follows

* BASIC - this is basic authencation, low security level
* OAUTH - OAUTH 2.0  includding Active Directory, Google, Ping Identity
* RADIUS - Use Radius based authentication
* PAM-LDAP - Use the linux PAM system with an LDAP authentication mode

Example to set PAM-LDAP as follows

```
config security auth pam-ldap
```

Output as follows

```
[Fri Mar  3 14:24:34 2023] > config security auth pam-ldap
[Fri Mar  3 14:24:35 2023] Authentication [BASIC] -> [PAM-LDAP]
[Fri Mar  3 14:24:35 2023] rebooting the system may be required
[Fri Mar  3 14:24:35 2023] >

```

For some authentication methods it requires a system reboot. In this case a reboot is required as the system needs to start LDAP client daemons.

### config security http

This enables/disables HTTP as a mode of access to the device. HTTP is plain clear text transport protocol, meaning all private data such as username and password are sent in the clear.

For private and secure networks this is ok(ish) for most situations HTTP should be disabled, allowing only HTTPS as the mode of access.

To disable HTTP access (HTTPS only)

```
config security http false
```

Example output

```
[Fri Mar  3 14:27:34 2023] > config security http false
[Fri Mar  3 14:27:34 2023] HTTP Access [enable] -> [false]
[Fri Mar  3 14:27:35 2023] please wait 60sec for web access to restart
[Fri Mar  3 14:27:35 2023] >
```

### config security timeoutSSH

This sets the SSH idle timeout timeout value. Use "show security" to validate the value is correct.

Time units supported are

```
s - second
m - minute
h - hour
disable - disable timeout
```

An example of setting a 30 sec idle timeout as follows

```
config security timeoutSSH 30s
```

With the following output

```
[Wed May 17 13:57:23 2023] > config security timeoutSSH 30s
[Wed May 17 13:57:24 2023] SSH Timeout [10000000000] -> [30000000000]
[Wed May 17 13:57:24 2023] please reboot for new setting to take effect
[Wed May 17 13:57:24 2023] >
```

NOTE: the system requires a reboot for the changes to take effect.

### config security timeoutWWW

This sets the WWW session timeout value. This is the maximum session duration. Once the session duration is reached the web interface will require a re-login.

Time units supported are

```
// Some code
s - second
m - minute
h - hour
disable - disable timeout
```

An example setting a 1 hour maximum session timeout

```
config security timeoutWWW 1h
```

Example output

```
[Wed May 17 14:00:35 2023] > config security timeoutWWW 1h
[Wed May 17 14:00:35 2023] WWW Timeout [60000000000] -> [3600000000000]
[Wed May 17 14:00:35 2023] please reboot for new setting to take effect
[Wed May 17 14:00:35 2023] >
```

NOTE: a the system requires a reboot for the changes to take effect.
