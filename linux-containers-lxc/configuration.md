# Configuration

### System Boot

In many cases a LXC Container needs to be started on boot. To enable this add in the following configuration to

```
/opt/fmadio/etc/time.lua
```

In the "Container Section" as follows, below is our example FMADIO Shark container being started on boot

```
["Container"] =
{
        ["Enable"]      = true,
        ["List"]        =
        {
        [1] = { Name = "fshark", OnBoot = true },
        }
}

```

The containers start in numerical order, if the OnBoot setting is set to "true"

### LXC RingCnt

By default a single LXC ring will be created if the container is enabled. This can be modified to enable eg 4 rings to be created on boot.

Example below creates 4 rings on system boot

```
["Container"] =
{
        ["Enable"]      = true,
        ["RingCnt"]     = 4,
        ["List"]        =
        {
        }
}
```

LXC RingList

**FW: 8307+**

Sometimes its easier to use named LXC Rings to make the system self documenting on what its doing. This is configured in the same file

```
/opt/fmadio/etc/time.lua
```

In the \["Container"] section there is a list \["RingList"]. Add in as many named queues as you need. Example shows 2 queues, one for EUREX the other for CME

```
.
.
["Container"] =
{
        ["Enable"]      = true,
        ["RingCnt"]     = 4,
        ["RingList"]    =
        {
                ["market_eurex"]        = { ["Desc"] = "EUREX market data" },
                ["market_cme"]          = { ["Desc"] = "CME Market Data" },
        },
        ["List"]        =
        {
        [1] = { Name = "fshark", OnBoot = true},
        }
},
.
.

```

System requires a reboot after the change for the rings to be created.

