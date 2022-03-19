# Custom CRON Job

FMADIO System uses a default cronjob that is located as below

```
/etc/cronjob.root
```

Direct edits to this file will be lost every reboot, as the directory is on the RAMDisk and not persistant.

This can be customized and made persistent across reboots, for anything such as scheduled weekly reboots. By creating the cronjob file as bellow.

```
cp /etc/crontab.root /opt/fmadio/etc/
```

This puts the crontab.root file into persistent storage at

```
/opt/fmadio/etc/
```

Allowing custom cronjobs to run on the system.

### Scheduled Reboot

One example of this is to schedule a system reboot every Sunday night. To do this please see the below example.

```
fmadio@fmadio20v2-149:~$ cat /opt/fmadio/etc/crontab.root
* * * * * /opt/fmadio/bin/watchdog.lua --nocal >> /mnt/store0/log/watchdog.log
59 23 * * 0 /sbin/reboot
fmadio@fmadio20v2-149:~$
```

Adding in the last line in the cronjob will schedule the reboot at midnight every Sunday.

**NOTE**: Please reboot the system or manually reload cron for the new setting to take effect
