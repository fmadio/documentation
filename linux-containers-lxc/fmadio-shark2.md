# FMADIO Shark2

FMADIO FShark2 is a full Ubuntu desktop accessiable via RDP or HTTP client. This has the latest Wireshark binary plus additional utilis enabling full packet investigations on the system.



## Port Forward Access

In many enviroments creating an additional IP for FShark2 is problematic. Instead port fowarding ports on the FMADIO Capture Appliance to the FShark2 device is a simpler apporach.

### Step 1) Install FShark2 package

Download latest fshark2 release

```
curl -O https://firmware.fmad.io/download/container/fshark2-current.tar.gz .
```

Example

```
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$ curl -O https://firmware.fmad.io/download/container/fshark2-current.tar.gz .
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1292M  100 1292M    0     0  14.7M      0  0:01:27  0:01:27 --:--:-- 15.8M
fmadio@fmadio100v2-228U:/mnt/store0/tmp2$

```

Extract to LXC directory

```
sudo tar xfzv fshark2-current.tar.gz  -C /opt/fmadio/lxc/
```

Example output:

```
fshark2-202310251136/
fshark2-202310251136/fmadiocli-start.lua
fshark2-202310251136/install.lua
fshark2-202310251136/fmadiocli-del.lua
fshark2-202310251136/config
fshark2-202310251136/vm-install.lua
fshark2-202310251136/fmadiocli-stop.lua
fshark2-202310251136/rootfs/
fshark2-202310251136/rootfs/sys/
fshark2-202310251136/rootfs/proc/
fshark2-202310251136/rootfs/home/
fshark2-202310251136/rootfs/home/fmadio/
fshark2-202310251136/rootfs/home/fmadio/.bash_logout
fshark2-202310251136/rootfs/home/fmadio/.vimrc
fshark2-202310251136/rootfs/home/fmadio/.bashrc
fshark2-202310251136/rootfs/home/fmadio/.config/
fshark2-202310251136/rootfs/home/fmadio/.config/tint2/
fshark2-202310251136/rootfs/home/fmadio/.config/tint2/tint2rc
fshark2-202310251136/rootfs/home/fmadio/.config/openbox/
fshark2-202310251136/rootfs/home/fmadio/.config/openbox/autostart
fshark2-202310251136/rootfs/home/fmadio/fmadio-background.png
fshark2-202310251136/rootfs/home/fmadio/.mozilla/
fshark2-202310251136/rootfs/home/fmadio/.mozilla/extensions/
fshark2-202310251136/rootfs/home/fmadio/.mozilla/firefox/
.
.
```

Or download an extract at the same time

```
curl  -s https://firmware.fmad.io/download/container/fshark2-current.tar.gz | sudo tar xfzv - -C /opt/fmadio/lxc/
```

### Step 2) Configure LXC

Change directory to the /opt/fmadio/lxc/fshark2-\<insert version>/

Run the install script. If no IP address for the container is used (e.g. fully NATed / port forward) leave the IP info blank

```
./install.lua
```

Example output

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136# ./install.lua
fmad fmadlua Nov 26 2023 (/opt/fmadio/bin/fmadiolua --nocal ./install.lua )
Disable cycle calibration
man0 imported

man0 ipv4 addr    [nil]
man0 ipv4 netmask [nil]
man0 ipv4 gw      [nil]
man0 ipv4 dns     [nil]
lxcname           [nil]
fshark2-202310251136
rm config
mkdir /mnt/store0/log/lxc/fshark2-202310251136 -p
mkdir /mnt/store0/lxc/data/fshark2-202310251136 -p
Container IP Address? (e.g. 192.168.1.100)

Container Netmask? (e.g. 24 for  255.255.255.0)

Container Gateway? (e.g. 192.168.1.1)

Container DNS? (e.g. 192.168.1.1)

----------------------
IP     :
CIDR   :
GW     :
DNS    :
rm /opt/fmadio/lxc/fshark2-202310251136/rootfs/etc/resolv.conf
touch /opt/fmadio/lxc/fshark2-202310251136/rootfs/etc/resolv.conf
echo nameserver  > /opt/fmadio/lxc/fshark2-202310251136/rootfs/etc/resolv.conf
echo fmadio100v2-228U-fshark2 > /opt/fmadio/lxc/fshark2-202310251136/rootfs/etc/hostname
echo 127.0.0.1 fmadio100v2-228U-fshark2 >> /opt/fmadio/lxc/fshark2-202310251136/rootfs/etc/hosts
/opt/fmadio/bin/fmadiocli "config lxc add fshark2-202310251136"
fmad fmadlua Nov 26 2023 (/opt/fmadio/bin/fmadiolua --nocal /opt/fmadio/bin/fmadiocli config lxc add fshark2-202310251136 )
Disable cycle calibration
[Tue Nov 28 15:07:13 2023] CmdLine [config lxc add fshark2-202310251136]
[Tue Nov 28 15:07:13 2023] Cmd [config lxc add fshark2-202310251136]
[Tue Nov 28 15:07:13 2023]**ERROR** Container named [fshark2-202310251136] already exists
[Tue Nov 28 15:07:13 2023]
[Tue Nov 28 15:07:13 2023] Example Usage:
[Tue Nov 28 15:07:13 2023] > config lxc add <container name>                 : adds the lxc container to the configuration
[Tue Nov 28 15:07:13 2023]
done 0.015017Sec 0.000250Min
done
done 2.942201Sec 0.049037Min
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136#
```

Step 3) Configure for NAT / Port forwarding

Comment out the lxc.net.1 (bridged interface) in the Config and set the default gateway to 192.168.255.2 (hosts internal interface)

```
lxc.net.0.ipv4.gateway = 192.168.255.2
```

Example Config

```
# lxc config generate by ubuntu install.lua
# set cpu to fmadio100v2 analytic
lxc.cgroup.cpuset.cpus=73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95

lxc.include = /usr/share/lxc/config/ubuntu.common.conf
lxc.arch = x86_64

lxc.rootfs.path = dir:/opt/fmadio/lxc/fshark2-202310251136/rootfs
lxc.uts.name = fmadio100v2-228U-fshark2

lxc.net.0.type = veth
lxc.net.0.link = fmad0
lxc.net.0.flags = up
lxc.net.0.ipv4.address = 192.168.255.11/24
lxc.net.0.ipv4.gateway = 192.168.255.2

#lxc.net.1.type = veth
#lxc.net.1.link = man0
#lxc.net.1.flags = up
#lxc.net.1.ipv4.address = /
#lxc.net.1.ipv4.gateway =

# Mount Data Directory
lxc.mount.entry = /mnt/store0/lxc/data/fshark2-202310251136  mnt/data/ none bind,create=dir 0 0

# Mount Log Directory
lxc.mount.entry = /mnt/store0/log/lxc/fshark2-202310251136  mnt/log/ none bind,create=dir 0 0

# map passthru queue
#lxc.mount.entry = /opt/fmadio/queue/lxc_ring_fshark2 opt/fmadio/queue/lxc_ring_fshark2 none bind,create=file 0 0

lxc.prlimit.nofile = 65535
lxc.prlimit.memlock = unlimited
```

### Step 4) AutoStart FSHARK2 on system boot

To enable automatic starting of the FSHAK2 container on system boot

```
fmadiocli  "config lxc boot fshark2-202310251136"
```

Example output:

```
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ fmadiocli  "config lxc boot fshark2-202310251136"
fmad fmadlua Nov 26 2023 (/opt/fmadio/bin/fmadiolua --nocal /opt/fmadio/bin/fmadiocli config lxc boot fshark2-202310251136 )
Disable cycle calibration
[Tue Nov 28 15:34:09 2023] CmdLine [config lxc boot fshark2-202310251136]
[Tue Nov 28 15:34:09 2023] Cmd [config lxc boot fshark2-202310251136]
[Tue Nov 28 15:34:10 2023] Set container [fshark2-202310251136] to boot on system start
done 0.165707Sec 0.002762Min
fmadio@fmadio100v2-228U:/opt/fmadio/lxc$ 
```

### Step 5) Start the Container manually

To start the container

```
sudo lxc-start -n fshark2-202310251136
```

Example output

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136# sudo lxc-start -n fshark2-202310251136
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136#
```

If it prints any messages it means there is a configuration error somewhere

### Step 6) Confirm FSHAK2 is running

Check the port 3000 (HTTP browser) and 3389 (RDP) are open

```
sudo lxc-attach -n fshark2-202310251136 -- netstat -antl
```

Example output, can see both ports are listed

```
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136# sudo lxc-attach -n fshark2-202310251136 -- netstat -antl
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:4822            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 127.0.0.1:3350          :::*                    LISTEN
tcp6       0      0 :::3000                 :::*                    LISTEN
tcp6       0      0 :::3389                 :::*                    LISTEN
root@fmadio100v2-228U:/mnt/store0/lxc/lib/lxc/fshark2-202310251136#

```

### Step 6) Configure IP Port forwarding

Copy the following iptables to the configuration directory

```
cp /opt/fmadio/etc_ro/iptables_fshark2_portfwd.conf  iptables.conf
```

Example output:

```
fmadio@fmadio100v2-228U:/opt/fmadio/etc$ cp /opt/fmadio/etc_ro/iptables_fshark2_portfwd.conf  iptables.conf
fmadio@fmadio100v2-228U:/opt/fmadio/etc$
```

Manually load the iptables setting

```
sudo iptables-restore < ./iptables.conf
```

Example output:

```
fmadio@fmadio100v2-228U:/opt/fmadio/etc$ sudo iptables-restore < ./iptables.conf
fmadio@fmadio100v2-228U:/opt/fmadio/etc$
```

For reference the /opt/fmadio/etc\_ro/iptables\_fshark2\_portfwd.conf file looks like the following

```
# Generated by iptables-save v1.6.1 on Tue Nov 28 15:30:44 2023
*nat
:PREROUTING ACCEPT [307:49051]
:INPUT ACCEPT [3:156]
:OUTPUT ACCEPT [112:11328]
:POSTROUTING ACCEPT [110:11210]
-A PREROUTING -p tcp -m tcp --dport 7000 -j DNAT --to-destination 192.168.255.11:3000
-A PREROUTING -p tcp -m tcp --dport 7001 -j DNAT --to-destination 192.168.255.11:3389
-A POSTROUTING -o man0 -j MASQUERADE
COMMIT
# Completed on Tue Nov 28 15:30:44 2023
# Generated by iptables-save v1.6.1 on Tue Nov 28 15:30:44 2023
*filter
:INPUT ACCEPT [15028:3079580]
:FORWARD ACCEPT [1651:509287]
:OUTPUT ACCEPT [14446:4545136]
COMMIT
# Completed on Tue Nov 28 15:30:44 2023

```

### Step 7) Confirm IP Tables setting is correct

Output the iptables information

```
sudo iptables -L -n -v -t nat
```

Example output:

```
fmadio@fmadio100v2-228U:/opt/fmadio/etc$ sudo iptables -L -n -v -t nat
Chain PREROUTING (policy ACCEPT 254 packets, 39578 bytes)
 pkts bytes target     prot opt in     out     source               destination
    8   416 DNAT       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:7000 to:192.168.255.11:3000
    0     0 DNAT       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:7001 to:192.168.255.11:3389

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 71 packets, 8550 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain POSTROUTING (policy ACCEPT 79 packets, 8966 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 MASQUERADE  all  --  *      man0    0.0.0.0/0            0.0.0.0/0
fmadio@fmadio100v2-228U:/opt/fmadio/etc$

```

### Step 8) Confirm access

Point the browser to port 7000  or RDP to port 7001 to confirm FSHARK2 is accessible

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>FSHARK2 in Browser</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>FSHARK2 via RDP</p></figcaption></figure>

