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
