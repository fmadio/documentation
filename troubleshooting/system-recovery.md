# System Recovery

In the unlikely event of a complete boot failure, system can be recovered by booting via the Virtual CDROM interface over a HTML BMC connection

Start by going to the BMC interface (default IP is 192.168.0.93) contact us for default login/password

![](<../.gitbook/assets/image (5) (1) (1).png>)

Start the Remote HTML KVM

![](<../.gitbook/assets/image (10).png>)

Will look like this. Select Brose Files, selecting an ISO image + Start the Media

![](<../.gitbook/assets/image (6) (2) (1).png>)



System will boot Ubuntu (for example), we are using ( systemrescue 8.01 amd64)

[https://sourceforge.net/projects/systemrescuecd/files/sysresccd-x86/8.01/systemrescue-8.01-amd64.iso/download](https://sourceforge.net/projects/systemrescuecd/files/sysresccd-x86/8.01/systemrescue-8.01-amd64.iso/download)

![](<../.gitbook/assets/image (7) (1).png>)

System will boot as follows, it may take several minutes depending on the speed of the HTML <-> FMADIO System connection. Recommend the closer the HTML instance is to the FMAD device the better.

If a particular boot stage is taking too long Ctrl-C can skip it

![](<../.gitbook/assets/image (1) (2).png>)



After SystemRescue CD has booted, the above is seen. Note the total number of bytes transfered over the Virtual ISO.

First step is to find the FMADIO OS and Persistant storage devices, Use the "lsblk" tool&#x20;

![](<../.gitbook/assets/image (8) (1).png>)



Looking foor a small (15GB) partition as the OS boot disk. In this case its sda1 and a large (224GB or larger) partition for the Persistent storage

Sometimes its easier to work over SSH. To access the system find or assign an IP address to the a reachable interface

SystemRescuelCD by default has iptables setup. Disable all iptables as follows

```
iptables -F
iptables -X
systemctl stop iptables
```

Then setup a password for the root account

```
passwd
```

Then ssh access to the system is possible

```
aaron@ingress:~$ ssh root@192.168.2.121
The authenticity of host '192.168.2.121 (192.168.2.121)' can't be established.
ECDSA key fingerprint is SHA256:v2CQjmUL70YpMJh39GWhcyqanKUU4eqLXxjTg/2i35Q.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.2.121' (ECDSA) to the list of known hosts.
root@192.168.2.121's password:
[root@sysrescue ~]#

```



Next mount the FMAD OS and Persistant storage disks. They may be sda\* or nvme0n1p\* in this example its mapped to sda

```
[root@sysrescue ~]# cd /mnt
[root@sysrescue /mnt]# mkdir system
[root@sysrescue /mnt]# mkdir store0
[root@sysrescue /mnt]# mount /dev/sda1 system/
[root@sysrescue /mnt]# mount /dev/sda2 store0/
[root@sysrescue /mnt]#

```

Next check the contents, it should look roughtly like this

```
[root@sysrescue /mnt]# ls -al /mnt/system/
total 64
drwxr-xr-x 5 root root  8192 Jan  1  1970 .
drwxr-xr-x 1 root root    80 May 22 08:53 ..
drwxr-xr-x 3 root root  8192 Apr 18 15:16 boot
drwxr-xr-x 2 root root 40960 Apr 18 15:16 firmware
drwxr-xr-x 5 root root  8192 May 11 10:13 tce
[root@sysrescue /mnt]# ls -al /mnt/store0
total 17244
drwxrwxrwx 32 root   root      4096 May 11 08:32 .
drwxr-xr-x  1 root   root        80 May 22 08:53 ..
drwxr-xr-x  3 root   root      4096 May 11 11:29 etc
drwxr-xr-x  2 root   root      4096 Dec 17  2019 filter
drwxrwxrwx  4 root   root  17477632 May 22 08:37 log
drwx------  2 root   root     16384 Dec 16  2019 lost+found
drwxr-xr-x  6   1002 games     4096 Oct 12  2019 lxc
drwxr-xr-x  3   1002 games     4096 Aug  5  2020 plugin_data
drwxr-xr-x  2 root   root      4096 Dec 17  2019 stream
drwx------  4 nobody root      4096 Dec 29 03:23 tmp
drwxrwxrwx 10 root   root      4096 Mar 23 10:25 tmp2
[root@sysrescue /mnt]#

```















