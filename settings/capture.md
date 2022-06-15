# Capture

## Advanced Capture Settings

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

### Midnight Roll Disable

By default the capture system will automatically stop all captures at midnight per the local timezone set on the capture device. This can be disabled which is helpful for capture periods which are < 24H but cross the midnight boundary.

In the `["Scheduler"]` section &#x20;

```
["Scheduler"] =
{
    ["ManualOffset"]    = 0,
},


```

Add the entry set to true, to disable the Midnight roll

```
["ManualOffset"]   = 1,
```

The config requires capture to be stopped and started for the the new setting to load.

