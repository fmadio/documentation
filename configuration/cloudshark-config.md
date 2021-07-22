# Cloudshark Config

## \*\***Work in progress\*\*, contact support**

Start by untaring the LXC container into the directory

```text
/mnt/store0/lxc/lib/lxc/
```

As follows \(Contact support for the tarball\)

```text
fmadio@fmadio20v3-287:/mnt/store0/lxc/lib/lxc$ sudo tar xf cloudshark5-1.tar.gz
fmadio@fmadio20v3-287:/mnt/store0/lxc/lib/lxc$ sudo ls -al cloudshark5
total 16
drwxrwx---    3 root     root          4096 Mar  4 12:20 .
drwxr-xr-x   12 fmadio   staff         4096 Jul 22 16:59 ..
-rw-r--r--    1 root     root           744 Jul 17 09:37 config
drwxr-xr-x   18 root     root          4096 Mar  4 12:30 rootfs
fmadio@fmadio20v3-287:/mnt/store0/lxc/lib/lxc$

```

Next Edit the LXC network config to allocate a specific IP address for CloudShark to run on. In the example below change the IPv4 address from 192.168.1.00/24 to the assigned IP/Prefix + update the IPv4 Gateway to be assigned correctly

```text
root@fmadio20v3-287:/mnt/store0/lxc/lib/lxc# cat cloudshark5/config

# Distribution configuration
lxc.include = /usr/share/lxc/config/centos.common.conf
lxc.arch = x86_64

# Container specific configuration
lxc.rootfs.path = dir:/opt/fmadio/lxc/lib/lxc/cloudshark5/rootfs
lxc.uts.name = cloudshark5

# Network configuration
lxc.net.0.type = veth
lxc.net.0.link = man0
lxc.net.0.flags = up
lxc.net.0.ipv4.address = 192.168.1.100/24
#lxc.net.0.ipv4.gateway = 192.168.1.1
root@fmadio20v3-287:/mnt/store0/lxc/lib/lxc#

```

Next update the Same IPv4 address to RHEL7 network configuration file

```text
root@fmadio20v3-287:/mnt/store0/lxc/lib/lxc# cat cloudshark5/rootfs/etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
HOSTNAME=cloudshark5
NM_CONTROLLED=no
TYPE=Ethernet
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
PREFIX=24
DNS1=192.168.1.1
root@fmadio20v3-287:/mnt/store0/lxc/lib/lxc#

```

FMADIO Configuration, edit config file

```text
/mnt/store0/etc/time.lua
```

Specifically enable API v1 and **Cloudshark** as follows, specifically focusing on the changed items.

Please set

CloudShark = true

DownloadAPI = "v1"

as Shown below, if the fields do not exist, please create them

```text
fmadio@fmadio20v3-287:/mnt/store0/etc$ cat time.lua

local Time=
{
.
.
.

["PCAP"] =
{
        .
        .
        
        ["CloudShark"]     = true,
        ["DownloadAPI"]     = "v1",
        .
        .
        
},
.
.
.
return Time
fmadio@fmadio20v3-287:/mnt/store0/etc$

```

Reboot the system or 

sudo killall stream\_http 

sudo killall stream\_command



TODO, deal with the hardcoded cloudshark ip. Not sure where to put that, probably in time.lua "CloudSharkIP"

