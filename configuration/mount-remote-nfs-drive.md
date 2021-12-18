# Mount Remote NFS (Linux) Drive

Mounting a remote NFS file system on the FMADIO capture device can be extremely useful in many cases.&#x20;

It provides a simple way to process PCAPs on the Capture Device then writing the results out to a remove storage system.\


Provides a simple and robust way to push PCAP data automatically from the capture device to an NFS share

### NFS Remote Mount Config

Please edit the configuration file in

```
/opt/fmadio/etc/disk.lua 
```

Create an entry named "NFSDisk" as shown below, be careful all punctuation is correct. The mount point should be "remote0" or "remote1" or "remote2" which maps to /mnt/remote0 /mnt/remote1 on the capture device.&#x20;

```
["NFSDisk"] =
{
        ["192.168.2.131:/home"] = "remote0",
},
```

Mount options can be specified after a double colon : e.g. to mount using NFSv3 or specify locking behavior

```
        ["192.168.1.200:/home"] = "remote0:-o vers=3 -o nolock",
```

After configuration file has been updated, run the following command to confirm the config file has no syntax errors.

```
fmadio@fmadio20v3-287:/mnt/store0/etc$ fmadiolua /opt/fmadio/etc/disk.lua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/disk.lua]
done 0.000065Sec 0.000001Min
fmadio@fmadio20v3-287:/mnt/store0/etc$
```

Above is the correct operation. Once complete please reboot the system to confirm the new mount points.

**NOTE:**  if the NFS mount fails it may take 5 min to boot the system. This is the time it takes for the NFS client timeout and give up on mounting the remote partition. If long boot delays are seen please check the log messages in /mnt/store0/log/messages for reasons the NFS mount failed**.**

### NFS Remote Mount Example

Below is a a full disk.lua example configuration file for reference

```
return
{
    CacheDisk =
    {
        ["S3EWNX0K116564W"] = "ssd0",
        ["S3EWNX0K116582F"] = "ssd1",
        ["S3EWNX0K116574D"] = "ssd2",
        ["S3EWNX0K116592K"] = "ssd3",
    }
    ,
    RaidDisk =
    {
        ["6522KJS3FSAA"] = "hdd0",
        ["25Q8K2SXFSAA"] = "hdd1",
        ["357FK7NVFSAA"] = "hdd2",
    }
    ,
    ParDisk =
    {
        ["16PFKD4QFSAA"] = "par0",
    },
    OSDisk =
    {
        ["D8E107781F8400012443"] = "os0",
    }
    ,
    ["NFSDisk"] =
    {
        ["192.168.1.100:/home"] = "remote0",
    }
    ,
    IndexDisk = "ssd",
    CacheLevel = "full",
    RaidLevel = "raid0",
}

```

\
