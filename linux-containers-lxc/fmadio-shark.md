# FMADIO Shark

FMADIO Shark is a self contained LXC container that runs on the FMADIO Packet Capture systems. It provides a "Wireshark Lite" way to navigate thats fully compatible with Wireshark, as the backend is Wireshark.

To configure and run as follows

### Download Latest FShark Release

Copy the tarball to /tmp

### Unpack FShark

Unpack the tarball into the /opt/fmadio/lxc directory

```
/mnt/store0/lxc/lib/lxc$ sudo tar xfzv /tmp/fmadshark-release_20220317_164527-314-4436d67.tar.gz
./fshark_20220317_1646/
./fshark_20220317_1646/config
./fshark_20220317_1646/install.lua
./fshark_20220317_1646/rootfs/
./fshark_20220317_1646/rootfs/lib/
./fshark_20220317_1646/rootfs/lib/udev/
./fshark_20220317_1646/rootfs/lib/udev/ifupdown-hotplug

```

### Link the latest release

Symlink the latest release. Optionally removing the symlink of any older version

```
/mnt/store0/lxc/lib/lxc$ sudo ln -s fshark_20220317_1646/ fshark

```

### Run the Container Install Script

```
/mnt/store0/lxc/lib/lxc$ cd fshark

/mnt/store0/lxc/lib/lxc/fshark_20220317_1646$ sudo ./install.lua
fmad fmadlua Mar 17 2022
loading filename [./install.lua]
Host Address:192.168.2.225
sudo sed -i "s/.*CONFIG_HOST$/        ip = \"192.168.1.1\", -- (Thu Mar 17 20:52:41 2022) CONFIG_HOST/" ./rootfs/srv/fshark/fmadio/etc/host_config.lua
done 0.005622Sec 0.000094Min
/mnt/store0/lxc/lib/lxc/fshark_20220317_1646$

```

### Enable in the Config File

Edit the file&#x20;

```
/opt/fmadio/etc/time.lua
```

Setting the following \["FShark"] = true,  if the field does not exist then create it

```
["PCAP"] =
{
        ["TimeStampMode"] = "nic",
        ["TimeResolution"] = "nsec",
        ["TimeSortDepth"]  = 256,
        ["Decap"]          = false,
        ["Slice"]          = 0,
        ["DownloadIdleTimeout"] = 30000000000,
        ["FShark"]     = true,
        ["DownloadAPI"]     = "v1",
},

```

### Clear any Browser Cache

Optionally clear browser cache. Sometimes the browser will cache old Javascript files that need to be updated.

### Packet Browser

&#x20;PacketBrowser and PacketScope should be visible on the GUI as follows

![FShark Packet Viewing](<../.gitbook/assets/image (129).png>)
