# CPU Mapping

**FW: 8325+**

FMADIO Packet Capture system have 16 to 96 CPUs, this can make configuration and allocation of all these CPU resources tedious and error prone.

To assist the configuration file located in

```
/opt/fmadio/etc/time.lua
```

There is a section name CPUMap, an example is shown below

```
["CPUMap"] =
{
    ["Enable"] = true,
    [  0] = "fmadio",                        --  CPU:  0 Core:  0 Thread:0 Node: 0 system
    [  1] = "",                              --  CPU:  1 Core:  1 Thread:0 Node: 0
    [  2] = "fmadio",                        --  CPU:  2 Core:  2 Thread:0 Node: 0 system
    [  3] = "fmadio",                        --  CPU:  3 Core:  3 Thread:0 Node: 0 system
    [  4] = "",                              --  CPU:  4 Core:  4 Thread:0 Node: 0
    [  5] = "",                              --  CPU:  5 Core:  5 Thread:0 Node: 0
    [  6] = "",                              --  CPU:  6 Core:  6 Thread:0 Node: 0
    [  7] = "",                              --  CPU:  7 Core:  7 Thread:0 Node: 0
    [  8] = "",                              --  CPU:  8 Core:  8 Thread:0 Node: 0
    [  9] = "",                              --  CPU:  9 Core:  9 Thread:0 Node: 0
    [ 10] = "",                              --  CPU: 10 Core: 10 Thread:0 Node: 0
    [ 11] = "",                              --  CPU: 11 Core: 11 Thread:0 Node: 0
    [ 12] = "",                              --  CPU: 12 Core: 12 Thread:0 Node: 0
    [ 13] = "",                              --  CPU: 13 Core: 13 Thread:0 Node: 0
    [ 14] = "",                              --  CPU: 14 Core: 14 Thread:0 Node: 0
    [ 15] = "",                              --  CPU: 15 Core: 15 Thread:0 Node: 0
    [ 16] = "",                              --  CPU: 16 Core: 16 Thread:0 Node: 0
    [ 17] = "",                              --  CPU: 17 Core: 17 Thread:0 Node: 0
    [ 18] = "",                              --  CPU: 18 Core: 18 Thread:0 Node: 0
    [ 19] = "",                              --  CPU: 19 Core: 19 Thread:0 Node: 0
    [ 20] = "",                              --  CPU: 20 Core: 20 Thread:0 Node: 0
    [ 21] = "",                              --  CPU: 21 Core: 21 Thread:0 Node: 0
    [ 22] = "",                              --  CPU: 22 Core: 22 Thread:0 Node: 0
    [ 23] = "",                              --  CPU: 23 Core: 23 Thread:0 Node: 0
    [ 24] = "fmadio",                        --  CPU: 24 Core:  0 Thread:0 Node: 1 system
    [ 25] = "fmadio",                        --  CPU: 25 Core:  1 Thread:0 Node: 1 system
    [ 26] = "fmadio",                        --  CPU: 26 Core:  2 Thread:0 Node: 1 system
    [ 27] = "fmadio",                        --  CPU: 27 Core:  3 Thread:0 Node: 1 system
    [ 28] = "fmadio",                        --  CPU: 28 Core:  4 Thread:0 Node: 1 system
    [ 29] = "fmadio",                        --  CPU: 29 Core:  5 Thread:0 Node: 1 system
    [ 30] = "fmadio",                        --  CPU: 30 Core:  6 Thread:0 Node: 1 system
    [ 31] = "fmadio",                        --  CPU: 31 Core:  7 Thread:0 Node: 1 system
    [ 32] = "",                              --  CPU: 32 Core:  8 Thread:0 Node: 1
    [ 33] = "",                              --  CPU: 33 Core:  9 Thread:0 Node: 1
    [ 34] = "",                              --  CPU: 34 Core: 10 Thread:0 Node: 1
    [ 35] = "",                              --  CPU: 35 Core: 11 Thread:0 Node: 1
    [ 36] = "",                              --  CPU: 36 Core: 12 Thread:0 Node: 1
    [ 37] = "",                              --  CPU: 37 Core: 13 Thread:0 Node: 1
    [ 38] = "",                              --  CPU: 38 Core: 14 Thread:0 Node: 1
    [ 39] = "",                              --  CPU: 39 Core: 15 Thread:0 Node: 1
    [ 40] = "",                              --  CPU: 40 Core: 16 Thread:0 Node: 1
    [ 41] = "",                              --  CPU: 41 Core: 17 Thread:0 Node: 1
    [ 42] = "",                              --  CPU: 42 Core: 18 Thread:0 Node: 1
    [ 43] = "",                              --  CPU: 43 Core: 19 Thread:0 Node: 1
    [ 44] = "",                              --  CPU: 44 Core: 20 Thread:0 Node: 1
    [ 45] = "",                              --  CPU: 45 Core: 21 Thread:0 Node: 1
    [ 46] = "",                              --  CPU: 46 Core: 22 Thread:0 Node: 1
    [ 47] = "",                              --  CPU: 47 Core: 23 Thread:0 Node: 1
    [ 48] = "",                              --  CPU: 48 Core:  0 Thread:1 Node: 0
    [ 49] = "",                              --  CPU: 49 Core:  1 Thread:1 Node: 0
    [ 50] = "",                              --  CPU: 50 Core:  2 Thread:1 Node: 0
    [ 51] = "",                              --  CPU: 51 Core:  3 Thread:1 Node: 0
    [ 52] = "",                              --  CPU: 52 Core:  4 Thread:1 Node: 0
    [ 53] = "",                              --  CPU: 53 Core:  5 Thread:1 Node: 0
    [ 54] = "",                              --  CPU: 54 Core:  6 Thread:1 Node: 0
    [ 55] = "",                              --  CPU: 55 Core:  7 Thread:1 Node: 0
    [ 56] = "",                              --  CPU: 56 Core:  8 Thread:1 Node: 0
    [ 57] = "",                              --  CPU: 57 Core:  9 Thread:1 Node: 0
    [ 58] = "",                              --  CPU: 58 Core: 10 Thread:1 Node: 0
    [ 59] = "",                              --  CPU: 59 Core: 11 Thread:1 Node: 0
    [ 60] = "",                              --  CPU: 60 Core: 12 Thread:1 Node: 0
    [ 61] = "",                              --  CPU: 61 Core: 13 Thread:1 Node: 0
    [ 62] = "",                              --  CPU: 62 Core: 14 Thread:1 Node: 0
    [ 63] = "",                              --  CPU: 63 Core: 15 Thread:1 Node: 0
    [ 64] = "",                              --  CPU: 64 Core: 16 Thread:1 Node: 0
    [ 65] = "",                              --  CPU: 65 Core: 17 Thread:1 Node: 0
    [ 66] = "",                              --  CPU: 66 Core: 18 Thread:1 Node: 0
    [ 67] = "",                              --  CPU: 67 Core: 19 Thread:1 Node: 0
    [ 68] = "",                              --  CPU: 68 Core: 20 Thread:1 Node: 0
    [ 69] = "",                              --  CPU: 69 Core: 21 Thread:1 Node: 0
    [ 70] = "",                              --  CPU: 70 Core: 22 Thread:1 Node: 0
    [ 71] = "",                              --  CPU: 71 Core: 23 Thread:1 Node: 0
    [ 72] = "fmadio",                        --  CPU: 72 Core:  0 Thread:1 Node: 1 system
    [ 73] = "",                              --  CPU: 73 Core:  1 Thread:1 Node: 1
    [ 74] = "",                              --  CPU: 74 Core:  2 Thread:1 Node: 1
    [ 75] = "",                              --  CPU: 75 Core:  3 Thread:1 Node: 1
    [ 76] = "fmadio",                        --  CPU: 76 Core:  4 Thread:1 Node: 1 system
    [ 77] = "fmadio",                        --  CPU: 77 Core:  5 Thread:1 Node: 1 system
    [ 78] = "fmadio",                        --  CPU: 78 Core:  6 Thread:1 Node: 1 system
    [ 79] = "fmadio",                        --  CPU: 79 Core:  7 Thread:1 Node: 1 system
    [ 80] = "",                              --  CPU: 80 Core:  8 Thread:1 Node: 1
    [ 81] = "",                              --  CPU: 81 Core:  9 Thread:1 Node: 1
    [ 82] = "",                              --  CPU: 82 Core: 10 Thread:1 Node: 1
    [ 83] = "",                              --  CPU: 83 Core: 11 Thread:1 Node: 1
    [ 84] = "",                              --  CPU: 84 Core: 12 Thread:1 Node: 1
    [ 85] = "",                              --  CPU: 85 Core: 13 Thread:1 Node: 1
    [ 86] = "",                              --  CPU: 86 Core: 14 Thread:1 Node: 1
    [ 87] = "",                              --  CPU: 87 Core: 15 Thread:1 Node: 1
    [ 88] = "",                              --  CPU: 88 Core: 16 Thread:1 Node: 1
    [ 89] = "",                              --  CPU: 89 Core: 17 Thread:1 Node: 1
    [ 90] = "",                              --  CPU: 90 Core: 18 Thread:1 Node: 1
    [ 91] = "",                              --  CPU: 91 Core: 19 Thread:1 Node: 1
    [ 92] = "",                              --  CPU: 92 Core: 20 Thread:1 Node: 1
    [ 93] = "",                              --  CPU: 93 Core: 21 Thread:1 Node: 1
    [ 94] = "",                              --  CPU: 94 Core: 22 Thread:1 Node: 1
    [ 95] = "",                              --  CPU: 95 Core: 23 Thread:1 Node: 1
},

```

CPUS which are dedicated to FMADIO Packet Capture system can not modified, these are marked "System" in the comments section per below

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>FMADIO System CPUs</p></figcaption></figure>

Any change to system CPUs will be overwritten.

### Enable Custom CPU Mapping

By default the system allocates all CPUs to the FMADIO Capture System. To enable a customer CPU Mapping change Enable = true per the below setting

```
["Enable"] = true,
```

After enabling a FW update is required. As the enable re-writes the isolcpu Linux kernel boot parameters.

NOTE: after Enable = true is set, all subsequent firmware updates will modify the isolcpu setting. To return to stock default setting. Set to false, and update the firmware again.

### Assign a CPU

To assign a CPU to a specific LXC container, use the naming convention

```
container.process.instance
```

For example Market Data Gap detector LXC (mdgap) for CME is allocated to CPU 84

```
    .
    .
    [ 77] = "fmadio",                        --  CPU: 77 Core:  5 Thread:1 Node: 1 system
    [ 78] = "fmadio",                        --  CPU: 78 Core:  6 Thread:1 Node: 1 system
    [ 79] = "fmadio",                        --  CPU: 79 Core:  7 Thread:1 Node: 1 system
    [ 80] = "",                              --  CPU: 80 Core:  8 Thread:1 Node: 1
    [ 81] = "",                              --  CPU: 81 Core:  9 Thread:1 Node: 1
    [ 82] = "",                              --  CPU: 82 Core: 10 Thread:1 Node: 1
    [ 83] = "",                              --  CPU: 83 Core: 11 Thread:1 Node: 1
    [ 84] = "mdgap.cme.0",                   --  CPU: 84 Core: 12 Thread:1 Node: 1
    [ 85] = "",                              --  CPU: 85 Core: 13 Thread:1 Node: 1
    [ 86] = "",                              --  CPU: 86 Core: 14 Thread:1 Node: 1
    [ 87] = "",                              --  CPU: 87 Core: 15 Thread:1 Node: 1
    .
    .
```

This also requires re-running the LXC install script to correctly allocate the CPUs in the container.
