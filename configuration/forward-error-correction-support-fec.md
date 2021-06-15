# Forward Error Correction Support \(FEC\)

#### FW Version: 7130+

Support for FEC is built into all 100G capable Packet Capture systems. By default its turned off and requires manual setting to enable the port to link up.

### Configuration

Enabling FEC support requires editing the file

```text
/opt/fmadio/etc/network.lua
```

Add in cap0 and cap1 interface settings into the file if they do not exist, such as the following

```lua
,
["cap0"] =
{
    ["Address"] = "192.168.15.170",
    ["MAC"]     = "00:af:a0:02:00:00",
    ["FEC"]     = true,
},
["cap1"] =
{
    ["Address"] = "192.168.15.171",
    ["MAC"]     = "00:af:a0:02:01:00",
    ["FEC"]     = true,
},

```

Address and MAC settings are not required, only if you need the capture port to have an IP/MAC address

After updating the file check the syntax is correct by running the following command

```bash
fmadiolua /opt/fmadio/etc/network.lua
```

The output should look like the following, without any errors or warnings about the syntax.

```bash
fmadio@fmadio100v2-228U:~$ fmadiolua /opt/fmadio/etc/network.lua
fmad fmadlua May 22 2021
calibrating...
0 : 2095072184           2.0951 cycles/nsec offset:4.928 Mhz
Cycles/Sec 2095072184.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv fmadiolua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/network.lua]
done 0.000143Sec 0.000002Min
fmadio@fmadio100v2-228U:~$

```

Once complete, please reboot the system and FEC should be enabled on boot.

### Manual FEC settings

FEC settings can be overridden and set manually per the following commands

Force FEC on all ports

```bash
fmadio@fmadio100v2-228U:~$ sudo fnic_test --fec-force
FEC Force
calibrating...
0 : 2095071400           2.0951 cycles/nsec offset:4.929 Mhz
Cycles/Sec 2095071400.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
PCIVersion: 50434930 50434930
FECENable: 1 PortMask:0003
[0] FECEnable: 1 FECForce:1
[1] FECEnable: 1 FECForce:1

```

Disable FEC on all ports

```bash
fmadio@fmadio100v2-228U:~$ sudo fnic_test --no-fec-force
no FEC Force
calibrating...
0 : 2095073970           2.0951 cycles/nsec offset:4.926 Mhz
Cycles/Sec 2095073970.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
PCIVersion: 50434930 50434930
FECENable: 0 PortMask:0003
[0] FECEnable: 0 FECForce:0
[1] FECEnable: 0 FECForce:0
fmadio@fmadio100v2-228U:~$

```

