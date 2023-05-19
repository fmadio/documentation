# SSH Customization

FMADIO devices run exclusively from pseudo-ROM where any changes on the file system between reboots is lost. This ROM approach provides consistency and system predictability making maintenance simpler.

## &#x20;**Shell Environment**

One problem with this approach is shell customization becomes quite difficult. To allow small modifications in the shell environment when a user logs into the system it can run the shell script for each SSH session. Configuration file is:

```
/mnt/store0/etc/fmadio.rc

```

Please do not use this excessively, typically its used for setting ENV variables.

Example:

```
$ cat /mnt/store0/etc/fmadio.rc
# local shell prompt configuration (ash) ran on at boot time

export TEST="random test variable"

```

## Persistent **authorized\_keys**

This file is usually located in \~/.ssh/ directory. As that is part of the volatile file system, the persistent version of this is placed into

```
/opt/fmadio/etc/authorized_keys
```

This allows SSH keys to be used in a persistent way across reboots and power cycles. Note the file in /opt/fmadio/etc/authorized\_keys is only copied during bootup. Updates made after reboot are not copied to the user .ssh directory.

## **Custom SSHD config**

A customized sshd configuration file can be used by placing the customized configuration into

```
/opt/fmadio/etc/sshd_config 
```

This is helpful for example to force exclusive RSA based login / disable password login. Which is a good practice if the device is on a public network.\


## Custom SSH RSA ID

In many cases using the default fmadio SSH RSA ID is not a good security practice. As such custom SSH RSA keys both public and private can be copied into

```
/opt/fmadio/etc/fmadio_id_rsa
/opt/fmadio/etc/fmadio_id_rsa.pub
```

These will copied into \~/.ssh/idrsa and idrsa\_pub on boot

## Persistent SSH Tunnel

FW: 7974+

The FMADIO Packet Capture device may not be conveniently accessible on  a network. Ability to form persistent SSH tunnels both to and from the FMADIO Packet Capture Device is important.

We use autossh for this feature.

One typical example is pushing rsyslog traffic to a centralized location for ingest and processing. In this example we show how to configure such a tunnel that remains persistent across reboots.

#### 1) Create a new RSA keys on the FMADIO device

Storing the key in /mnt/store0/etc/sshtunnel.id without any password

```
fmadio@fmadio40v3SM-455:/mnt/store0/etc$ ssh-keygen -t rsa  -f /mnt/store0/etc/sshtunnel.id
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /mnt/store0/etc/sshtunnel.id.
Your public key has been saved in /mnt/store0/etc/sshtunnel.id.pub.
The key fingerprint is:
0b:9f:0e:eb:2f:00:1b:ca:01:b2:ff:17:2e:9f:97:2e fmadio@fmadio40v3SM-455
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|o                |
|o.               |
|..o              |
|.o.+  . S        |
|..o . .o o       |
|   . o..+.       |
|    o E=o        |
|     ==*+        |
+-----------------+
fmadio@fmadio40v3SM-455:/mnt/store0/etc$ ls -al sshtunnel.id*
-rw-------    1 fmadio   staff         1679 Jun 11 19:12 sshtunnel.id
-rw-r--r--    1 fmadio   staff          405 Jun 11 19:12 sshtunnel.id.pub
fmadio@fmadio40v3SM-455:/mnt/store0/etc$

```

#### 2) Add the public key to the remote servers&#x20;

Add the sshtunnel.id.pub key to the remote servers authorized\_keys. This allows password-less ssh access to the remote server.

#### 3) Test the connection by ssh to the remote server

To ensure it logs in correctly manually.

```
fmadio@fmadio40v3SM-455:~$ ssh -i /mnt/store0/etc/sshtunnel.id ubuntu@192.168.1.100

Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.100' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-1008-aws aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Jun 11 10:16:26 UTC 2022

  System load:  0.11376953125     Processes:             176
  Usage of /:   98.1% of 7.59GB   Users logged in:       1
  Memory usage: 89%              
  Swap usage:   23%

  => / is using 98.1% of 7.59GB

 * Ubuntu Pro delivers the most comprehensive open source security and
   compliance features.

   https://ubuntu.com/aws/pro

5 updates can be applied immediately.
To see these additional updates run: apt list --upgradable
$ exit
logout
Connection to closed.
fmadio@fmadio40v3SM-455:~$
```

#### 4) Create an on boot file that uses autossh

This gets run automatically on system boot. Because autossh handles reconnects, no cronjob or monitoring is required.

Create the file

```
/opt/fmadio/etc/boot.lua
```

Paste the contents as follows. This is a standard lua script, in this case its running autossh with some command line arguments. Anything can be run in the script for boot time setup

```
-- setup autossh tunnel to a remote syslog server
local Cmd = "/usr/bin/autossh -f -M 0 -N  -o StrictHostKeyChecking=false -i /mnt/store0/etc/autossh -L :5514:192.168.1.100:514 ubuntu@192.168.1.100"
print(Cmd)
os.execute(Cmd)


```

#### 5) Test the boot script

Can test the functionality of the boot script as follows. Correct output is similar to this

```
fmadio@fmadio40v3SM-455:~$ fmadiolua /mnt/store0/etc/boot.lua

fmad fmadlua Jun 11 2022 (fmadiolua /mnt/store0/etc/boot.lua )
calibrating...
0 : 2399987938           2.4000 cycles/nsec offset:0.012 Mhz
Cycles/Sec 2399987938.0000 Std:       0 cycle std(  0.00000000) Target:2.40 Ghz
failed to open self? [fmadiolua]
/usr/bin/autossh -f -M 0 -N  -o StrictHostKeyChecking=false -i /mnt/store0/etc/sshtunnel.id .....
done 0.001740Sec 0.000029Min
fmadio@fmadio40v3SM-455:~$

```

#### 6) Reboot the system

On reboot the boot.lua script is executed and a persistent ssh tunnel to the remote system is formed.

## Custom sudoers File

A customized persistant /etc/sudoers file can be created at the following location&#x20;

```
/opt/fmadio/etc/sudoers
```

On boot the system will override the default /etc/sudoers file with the file created per above

```
/opt/fmadio/etc/sudoers
```
