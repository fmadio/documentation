# SSH Password Reset

To reset the SSH password system the system requires KVM either directly with a physical montior and keyboard of via the BMC HTML viewer. We recommend HTML viewer and will show the steps here. The process is the same for a physical monitor and keyboard

1\) Connect to the BMC login and launch the HTML5 KVM

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

2\) The KVM viewer looks similar to below. If the screen is blank press a few random keys to disable the screensaver

<figure><img src="../.gitbook/assets/image (6) (4).png" alt=""><figcaption></figcaption></figure>

3\) Reboot the system using the Power menu

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

4\) Need to catch the system boot prompt here. One way is to tap space key or letter "r" every few secconds. This will prevent the bootloader from automatically starting.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

5\) Enter system "rescue" mode at the prompt and hit enter. This will boot the system into rescue mode.

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

6\) Rescue mode drop to a default prompt as follows

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

7\) Set a new password for fmadio user  as follows

```
sudo passwd fmadio
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

8\) Mount the persistent storage volume as follows. The exact path depends on the system SKU

**FMADIO100v2 or FMADIO100p3:**

```
sudo mount /dev/nvme0n1p2 /mnt/store0
```

**FMADIO20v3 or FMADIO40v3:**

```
sudo mount /dev/sda2 /mnt/store0
```

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

9\) Copy the shadow password file to persistant storage

```
sudo cp /etc/shadow /mnt/store0/etc/shadow
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

10\) reboot the system

```
sudo reboot
```

11\) SSH login fmadio has the newly set password

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Web User Password Reset

In addition to the above web user password and access list can be configured using fmadiocli tool. Documentation is here

[https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-userlist-password](https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-userlist-password)
