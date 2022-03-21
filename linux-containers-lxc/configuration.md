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
