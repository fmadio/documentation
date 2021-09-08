# SSH Customization

FMADIO devices run exclusively from pseudo-ROM where any changes on the file system between reboots is lost. This ROM approach provides consistency and system predictability making maintenance simpler.

##  **Shell Environment**

One problem with this approach is shell customization becomes quite difficult. To allow small modifications in the shell environment when a user logs into the system it can run the shell script for each SSH session. Configuration file is:

```text
/mnt/store0/etc/fmadio.rc

```

Please do not use this excessively, typically its used for setting ENV variables.

Example:

```text
$ cat /mnt/store0/etc/fmadio.rc
# local shell prompt configuration (ash) ran on at boot time

export TEST="random test variable"

```

## Persistent **authorized\_keys**

This file is usually located in ~/.ssh/ directory. As that is part of the volatile file system, the persistent version of this is placed into

```text
/opt/fmadio/etc/authorized_keys
```

This allows SSH keys to be used in a persistent way across reboots and power cycles. Note the file in /opt/fmadio/etc/authorized\_keys is only copied during bootup. Updates made after reboot are not copied to the user .ssh directory.

## **Custom SSHD config**

A customized sshd configuration file can be used by placing the customized configuration into

```text
/opt/fmadio/etc/sshd_config 
```

This is helpful for example to force exclusive RSA based login / disable password login. Which is a good practice if the device is on a public network.  


## Custom SSH RSA ID

In many cases using the default fmadio SSH RSA ID is not a good security practice. As such custom SSH RSA keys both public and private can be copied into

```text
/opt/fmadio/etc/fmadio_id_rsa
/opt/fmadio/etc/fmadio_id_rsa.pub
```

These will copied into ~/.ssh/idrsa and idrsa\_pub on boot

