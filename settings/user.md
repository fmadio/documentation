# User

## Advanced User Settings

Advanced capture settings require changing the configuration file directly as follows. As many of these options are customizations, please discuss with support@fmad.io any issues or side effects seen

```
/opt/fmadio/etc/time.lua
```

### Validate Config file

After modifying time.lua configuration file, please confirm syntax is correct by running the following

```
fmadiolua /opt/fmadio/etc/time.lua
```

The correct output looks like the following

```
fmadio@fmadio100v2-228U:~$ fmadiolua /opt/fmadio/etc/time.lua
fmad fmadlua Jun  3 2022 (fmadiolua /opt/fmadio/etc/time.lua )
calibrating...
0 : 2095078948           2.0951 cycles/nsec offset:4.921 Mhz
Cycles/Sec 2095078948.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
failed to open self? [fmadiolua]
done 0.000110Sec 0.000002Min
fmadio@fmadio100v2-228U:~$

```

### Disable Web Admin Permission

By default the Web user account has full permissions. There may be cases where Web access is for download and analyzing PCAP data only. Where configuration can be not modified.

To disable Web Admin mode SSH into the system and change the setting in

```
/opt/fmadio/etc/time.lua
```

In the `["Security"]` section &#x20;

```
["Security"] =
{
    ["HTTPAccess"] = "enabled",
    ["Auth"] = "BASIC",
    ["ConfigAccess"] = "full",
    ["GUIMode"] = "full",
    ["EnableWebDAV"] = false,
    ["RADIUS_Secret"] = "nil",
    ["RADIUS_Host"] = "nil",
    ["RADIUS_Protocol"] = "nil",
    ["RADIUS_Timeout"] = nil,
},

```

Change ConfigAccess setting to be "user"

```
["ConfigAccess"]   = "user",
```

The config file should be checked for syntax errors. Then restart the system. Once restarted the Web GUI no longer has Admin privileges'.&#x20;

