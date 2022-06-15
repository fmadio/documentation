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

### Capturing Across Midnight

In some cases capturing across midnight and rolling at a different time has advantages, such as Futures/Options exchanges who typically run 23H a day restarting very early in the morning.

This is achieved using the ManualOffset to offset when the capture rolls. To roll the capture as 5AM every day, setting 5 H \* 60min \* 60sec \* 1e9 nanoseconds

```
    ["ManualOffset"]    = 5 * 60 * 60 * 1e9,
```

Then setting the scheduler to start/stop at 00:00:59 and 23:59:59 as follows

![](<../.gitbook/assets/image (125).png>)

This will create captures from 05:00AM until 04:59:59 the next day

StartTime = ManualOffset (5H) + Scheduler Start Time 00:00  = 05:00 AM

StopTime = ManualOffset (5H) + Scheduler Stop Time 23:59:59 = 04:59AM + 1 Day
