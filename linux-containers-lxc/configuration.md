# Configuration

## Install

The following are generalized steps to install setup and start the specified container. Please consult any container specific documentation in addition to this process.

### Step 1)

Download the LXC into the directory  below

```
/opt/fmadio/lxc/
```

Check the MD5 sum against the published value

Untar the tarball in that directory as root.

Example below shows the steps for the mdgap lxc container

```
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ sudo curl -O https://firmware.fmad.io/download/container/mdgap-20230512.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  824M  100  824M    0     0  14.2M      0  0:00:58  0:00:58 --:--:-- 14.9M
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ 

fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ md5sum mdgap-20230512.tar.gz
5da383edd150b6ac9b5cedc8550fdca3  mdgap-20230512.tar.g

fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ sudo tar xf mdgap-20230512.tar.gz
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$
```

### Step 2)

Most LXCs have an install script in the root directory, it will configure the container requiring some information such as IP Netmask. Gateway etc.

```
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ sudo su
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc# cd mdgap-20230512
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# ls -al
total 24
drwxrwx---    3 root     root          4096 May 12 08:16 ./
drwxr-xr-x   35 root     root          4096 Jun 24 05:41 ../
-rw-r--r--    1 root     root          1852 May 12 08:16 config
-rwxr-xr-x    1 root     root          5544 May 10 06:18 install.lua
drwxr-xr-x   28 root     root          4096 May  8 05:07 rootfs/
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512#

root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# sudo ./install.lua
rm config
mkdir /mnt/store0/log/lxc/mdgap/clickhouse -p
mkdir /mnt/store0/log/lxc/mdgap/grafana -p
mkdir /mnt/store0/log/lxc/mdgap/remote -p
mkdir /mnt/store0/lxc/data/mdgap/clickhouse -p
mkdir /mnt/store0/lxc/data/mdgap/grafana -p
Container IP Address? (e.g. 192.168.1.100)
192.168.2.178
Container Netmask? (e.g. 24 for  255.255.255.0)
24
Container Gateway? (e.g. 192.168.1.1)
192.168.2.254
Container DNS? (e.g. 192.168.1.1)
1.1.1.1
----------------------
IP     : 192.168.2.178
CIDR   : 24
GW     : 192.168.2.254
DNS    : 1.1.1.1
echo nameserver 1.1.1.1 > rootfs/etc/resolv.conf

echo fmadio100v2-228U-mdgap > rootfs/etc/hostname
echo 127.0.0.1 fmadio100v2-228U-mdgap >> rootfs/etc/hosts
done 13.161981Sec 0.219366Min
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512#
```

### Step 3)

Modify the containers configuration. such as adding rings or shared mount points

```
/opt/fmadio/lxc/<container>/config
```

Example is enabling the Eurex lxc ring to the container by uncommenting in the configuration file, the line

```
lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_eurex                opt/fmadio/queue/lxc_ring_mdgap_eurex           none bind,create=file 0 0
```

In the configuration below

```
.
.
.
#set log mount
lxc.mount.entry = /mnt/store0/log/lxc/mdgap mnt/store0/log none bind,create=dir 0 0

#set data path
lxc.mount.entry = /mnt/store0/lxc/data/mdgap mnt/store0/mdgap none bind,create=dir 0 0

#set lxc ring mount
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring0                  opt/fmadio/queue/lxc_ring0                      none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_cme                 opt/fmadio/queue/lxc_ring_mdgap_cme             none bind,create=file 0 0

lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_eurex                opt/fmadio/queue/lxc_ring_mdgap_eurex           none bind,create=file 0 0

#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_nasdaq      opt/fmadio/queue/lxc_ring_mdgap_nasdaq          none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_siac_cqs    opt/fmadio/queue/lxc_ring_mdgap_siac_cqs        none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_siac_cts    opt/fmadio/queue/lxc_ring_mdgap_siac_cts        none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_siac_opra   opt/fmadio/queue/lxc_ring_mdgap_siac_opra       none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_lse_mitch   opt/fmadio/queue/lxc_ring_mdgap_lse_mitch       none bind,create=file 0 0
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_mdgap_euronext      opt/fmadio/queue/lxc_ring_mdgap_euronext        none bind,create=file 0 0

```

Ensure to save the changes

### Step 3a)

Generally containers require LXC Rings to feed data to. In this example we create an lxc ring named `"mdgap_eurex"` as the container references this ring in step 3) it must be created for the container to start correctly.

```
fmadiocli "config push lxc add mdgap_eurex"
```

Example output as follows

```
/root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# fmadiocli "config push lxc add mdgap_eurex"

Disable cycle calibration
[Sat Jun 24 05:57:42 2023]   _____                    .___.__
[Sat Jun 24 05:57:42 2023] _/ ____\_____  _____     __| _/|__|  ____
[Sat Jun 24 05:57:42 2023] \   __\/     / \__  \   / __ | |  | /  _ \
[Sat Jun 24 05:57:42 2023]  |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
[Sat Jun 24 05:57:42 2023]  |__| |__|_|  /(____  /\____ | |__| \____/
[Sat Jun 24 05:57:42 2023]             \/      \/      \/
[Sat Jun 24 05:57:42 2023] ============================================
[Sat Jun 24 05:57:42 2023]   -+ Packets confiscated by Customs +-
[Sat Jun 24 05:57:42 2023]
[Sat Jun 24 05:57:42 2023]  type '?' for command information
[Sat Jun 24 05:57:42 2023]  type '???' for verbose information
[Sat Jun 24 05:57:42 2023]
[Sat Jun 24 05:57:42 2023] History: 1530
[Sat Jun 24 05:57:42 2023] CmdLine [config push lxc add mdgap_eurex]
[Sat Jun 24 05:57:42 2023] Cmd [config push lxc add mdgap_eurex]
[Sat Jun 24 05:57:42 2023] Created Ring named [mdgap_eurex]
cycle calibration disabled
null  output
RING reset
RING file [/opt/fmadio/queue/lxc_ring_mdgap_eurex]
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] Size missmatch 0 12595200
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] Size   : 12595200 16777216
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] Version:        0      100
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] version wrong force reset
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] Put:0 0 0x7f68002f0000
RING[/opt/fmadio/queue/lxc_ring_mdgap_eurex            ] Get:0 0 0x7f68002f1000
[Sat Jun 24 05:57:43 2023] created ring [/opt/fmadio/queue/lxc_ring_mdgap_eurex]
[Sat Jun 24 05:57:43 2023] New Push LXC target [/opt/fmadio/queue/lxc_ring_mdgap_eurex]
done 0.196703Sec 0.003278Min
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512#

```

### Step 4)

Add the container to the system configuration file using `fmadiocli` utility

Add the container using the command

```
fmadiocli "config lxc add <container name>
```

Example shown below

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# fmadiocli "config lxc add mdgap-20230512"
Disable cycle calibration
[Sat Jun 24 05:52:30 2023]   _____                    .___.__
[Sat Jun 24 05:52:30 2023] _/ ____\_____  _____     __| _/|__|  ____
[Sat Jun 24 05:52:30 2023] \   __\/     / \__  \   / __ | |  | /  _ \
[Sat Jun 24 05:52:30 2023]  |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
[Sat Jun 24 05:52:30 2023]  |__| |__|_|  /(____  /\____ | |__| \____/
[Sat Jun 24 05:52:30 2023]             \/      \/      \/
[Sat Jun 24 05:52:30 2023] ============================================
[Sat Jun 24 05:52:30 2023]   -+ Packets confiscated by Customs +-
[Sat Jun 24 05:52:30 2023]
[Sat Jun 24 05:52:30 2023]  type '?' for command information
[Sat Jun 24 05:52:30 2023]  type '???' for verbose information
[Sat Jun 24 05:52:30 2023]
[Sat Jun 24 05:52:30 2023] History: 1530
[Sat Jun 24 05:52:30 2023] CmdLine [config lxc add mdgap-20230512]
[Sat Jun 24 05:52:30 2023] Cmd [config lxc add mdgap-20230512]
[Sat Jun 24 05:52:30 2023] Added container [mdgap-20230512] to the configuration
done 0.195539Sec 0.003259Min
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512#
```

### Step 5)

Start the container using fmadiocli utlity

```
fmadiocli "config lxc start <container name>"
```

Example starting the MDGap container

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# fmadiocli "config lxc start mdgap-20230512"
fmad fmadlua Jun 24 2023 (/opt/fmadio/bin/fmadiolua --nocal /opt/fmadio/bin/fmadiocli config lxc start mdgap-20230512 )
Disable cycle calibration
[Sat Jun 24 05:59:14 2023]   _____                    .___.__
[Sat Jun 24 05:59:14 2023] _/ ____\_____  _____     __| _/|__|  ____
[Sat Jun 24 05:59:14 2023] \   __\/     / \__  \   / __ | |  | /  _ \
[Sat Jun 24 05:59:14 2023]  |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
[Sat Jun 24 05:59:14 2023]  |__| |__|_|  /(____  /\____ | |__| \____/
[Sat Jun 24 05:59:14 2023]             \/      \/      \/
[Sat Jun 24 05:59:14 2023] ============================================
[Sat Jun 24 05:59:14 2023]   -+ Packets confiscated by Customs +-
[Sat Jun 24 05:59:14 2023]
[Sat Jun 24 05:59:14 2023]  type '?' for command information
[Sat Jun 24 05:59:14 2023]  type '???' for verbose information
[Sat Jun 24 05:59:14 2023]
[Sat Jun 24 05:59:14 2023] History: 1530
[Sat Jun 24 05:59:14 2023] CmdLine [config lxc start mdgap-20230512]
[Sat Jun 24 05:59:14 2023] Cmd [config lxc start mdgap-20230512]
[Sat Jun 24 05:59:14 2023] sudo lxc-start -n mdgap-20230512 --logfile /tmp/lxc_mdgap-20230512_1687600754602326016
[Sat Jun 24 05:59:16 2023]
[Sat Jun 24 05:59:16 2023] use the following on a shell to attach to the conatiners console
[Sat Jun 24 05:59:16 2023] sudo lxc-attach -n mdgap-20230512
[Sat Jun 24 05:59:16 2023]
done 1.899527Sec 0.031659Min
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512#

```

### Step 6)&#x20;

Optionally attach to the containers console using the command

```
sudo lxc-attach -n <container name>
```

Example of MDGap container

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/mdgap-20230512# lxc-attach -n mdgap-20230512
root@fmadio100v2-228U-mdgap:/#
```

### Please see any container specific configuration and setup documentation
