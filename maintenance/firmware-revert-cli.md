# Firmware Revert (CLI)

FMADIO Systems use a RAM based linux distribution, where all persistent files have to be explicitly specified. This approach allows easy forward and backward changing of the system firmware.

Forward updates to new FW is documented  below

{% content-ref url="firmware-update-cli.md" %}
[firmware-update-cli.md](firmware-update-cli.md)
{% endcontent-ref %}

Below is how to revert to a previous image.

### Step 1

Check is the reverted firware is already uploaded on the system

```
ls -al /mnt/system/firmware/*.bin
```

Example output shown below

<pre class="language-bash"><code class="lang-bash"><strong>fmadio@fmadio100v2-228U:~$ ls -al /mnt/system/firmware/*.bin
</strong>-rwxr-xr-x    1 root     root     197094389 Mar  1 12:13 /mnt/system/firmware/fmadio100v2_20191006_1657.bin
-rwxr-xr-x    1 root     root     459983681 Feb 21 15:00 /mnt/system/firmware/fmadio100v2_20220909_0340.bin
-rwxr-xr-x    1 root     root     488447559 Feb 21 15:51 /mnt/system/firmware/fmadio100v2_20230221_1549.bin-rwxr-xr-x    1 root     root     491472527 Mar  3 11:55 /mnt/system/firmware/fmadio100v2_20230303_1154.bin
-rwxr-xr-x    1 root     root     491484052 Mar  3 13:10 /mnt/system/firmware/fmadio100v2_20230303_1307.bin
-rwxr-xr-x    1 root     root     471198901 Mar  7 19:26 /mnt/system/firmware/fmadio100v2_20230307_1925.bin
-rwxr-xr-x    1 root     root     491383334 Mar  9 16:50 /mnt/system/firmware/fmadio100v2_20230309_1649.bin
fmadio@fmadio100v2-228U:~$
</code></pre>

If the firmware file is shown with correct MD5 sum skip to Step 3

### Step 2

Upload the target firmware onto the system. There is no harm in overwriting an existing firmware file.  In the below example using firmware FW:8475  fmadio100v2\_20230307\_1925.bin

```
sudo firmware_install.lua --upload fmadio100v2_20230307_1925.bin
```

Example output similar to this

```
fmadio@fmadio100v2-228U:~$ sudo firmware_install.lua --upload fmadio100v2_20230307_1925.bin
fmad fmadlua Mar  9 2023 (/usr/local/bin/fmadiolua /opt/fmadio/bin/firmware_install.lua --upload fmadio100v2_20230307_1925.bin )
Upload Firmware [fmadio100v2_20230307_1925.bin]
mkdir: can't create directory '/mnt/store0/tmp/fw/': File exists
Filename [fmadio100v2_20230307_1925]
unpack FW
unpack: [fmadio100v2_20230307_1925.core]
unpack: [fmadio100v2_20230307_1925.kernel]
unpack: [fmadio100v2_20230307_1925.mydata]
unpack: [fmadio100v2_20230307_1925.post.lua]
unpack: [fmadio100v2_20230307_1925.pre.lua]
unpack: [fmadio100v2_20230307_1925.rom.2x100G]
unpack: [fmadio100v2_20230307_1925.rom.2x100G_replay]
unpack: [fmadio100v2_20230307_1925.rom.2x40G]
unpack: [fmadio100v2_20230307_1925.rom.8x10G]
unpack: [fmadio100v2_20230307_1925.sign]
unpack: [fmadio100v2_20230307_1925.syslinux.cfg]
unpack: [fmadio100v2_20230307_1925.syslinux.cfg.analytics]
unpack: [fmadio100v2_20230307_1925.tcz]
check TCZ
check sign
verify signature
sign: gpg: WARNING: unsafe ownership on homedir `/home/fmadio/.gnupg/'
sign: fe54748ff985ec1ebc118a985d459b1a  fmadio100v2_20230307_1925.tcz
sign: b6cad8c08e51fd80b412b6c1602af4b4  fmadio100v2_20230307_1925.pre.lua
sign: 180fe4e475b521f7bf36f7f9d2acc6c8  fmadio100v2_20230307_1925.post.lua
sign: 7f1b7577ab5d836e870bb7045586efd0  fmadio100v2_20230307_1925.core
sign: ed6276d1815c6b8ce808dd1ec6f14d5d  fmadio100v2_20230307_1925.kernel
sign: 3d1a0e22186d43592c7718ef6a1eb0d0  fmadio100v2_20230307_1925.mydata
sign: gpg: Signature made Tue Mar  7 19:25:09 2023 SGT using RSA key ID 35173534
sign: gpg: checking the trustdb
sign: gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
sign: gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
sign: gpg: Good signature from "fmadio <support@fmad.io>" [ultimate]
Found Signatures:
fmadio100v2_20230307_1925.tcz : fe54748ff985ec1ebc118a985d459b1a  fmadio100v2_20230307_1925.tcz
fmadio100v2_20230307_1925.pre.lua : b6cad8c08e51fd80b412b6c1602af4b4  fmadio100v2_20230307_1925.pre.lua
fmadio100v2_20230307_1925.post.lua : 180fe4e475b521f7bf36f7f9d2acc6c8  fmadio100v2_20230307_1925.post.lua
fmadio100v2_20230307_1925.mydata : 3d1a0e22186d43592c7718ef6a1eb0d0  fmadio100v2_20230307_1925.mydata
fmadio100v2_20230307_1925.kernel : ed6276d1815c6b8ce808dd1ec6f14d5d  fmadio100v2_20230307_1925.kernel
fmadio100v2_20230307_1925.core : 7f1b7577ab5d836e870bb7045586efd0  fmadio100v2_20230307_1925.core
Signatures good
[fmadio100v2_20230307_1925.tcz ] Expect:(fe54748ff985ec1ebc118a985d459b1a  fmadio100v2_20230307_1925.tcz) Calc:(fe54748ff985ec1ebc118a985d459b1a  fmadio100v2_20230307_1925.tcz)
[fmadio100v2_20230307_1925.pre.lua] Expect:(b6cad8c08e51fd80b412b6c1602af4b4  fmadio100v2_20230307_1925.pre.lua) Calc:(b6cad8c08e51fd80b412b6c1602af4b4  fmadio100v2_20230307_1925.pre.lua)
[fmadio100v2_20230307_1925.post.lua] Expect:(180fe4e475b521f7bf36f7f9d2acc6c8  fmadio100v2_20230307_1925.post.lua) Calc:(180fe4e475b521f7bf36f7f9d2acc6c8  fmadio100v2_20230307_1925.post.lua)
[fmadio100v2_20230307_1925.mydata] Expect:(3d1a0e22186d43592c7718ef6a1eb0d0  fmadio100v2_20230307_1925.mydata) Calc:(3d1a0e22186d43592c7718ef6a1eb0d0  fmadio100v2_20230307_1925.mydata)
[fmadio100v2_20230307_1925.kernel] Expect:(ed6276d1815c6b8ce808dd1ec6f14d5d  fmadio100v2_20230307_1925.kernel) Calc:(ed6276d1815c6b8ce808dd1ec6f14d5d  fmadio100v2_20230307_1925.kernel)
[fmadio100v2_20230307_1925.core] Expect:(7f1b7577ab5d836e870bb7045586efd0  fmadio100v2_20230307_1925.core) Calc:(7f1b7577ab5d836e870bb7045586efd0  fmadio100v2_20230307_1925.core)
Firmware is valid
Firmware Copy Took 1.845648 sec
Check fmadio100v2_20230307_1925.tcz                      Expect:       120856576 Found:       120856576
Check fmadio100v2_20230307_1925.pre.lua                  Expect:              87 Found:              87
Check fmadio100v2_20230307_1925.post.lua                 Expect:             754 Found:             754
Check fmadio100v2_20230307_1925.mydata                   Expect:           21573 Found:           21573
Check fmadio100v2_20230307_1925.kernel                   Expect:         4943968 Found:         4943968
Check fmadio100v2_20230307_1925.core                     Expect:       314395612 Found:       314395612
Firmware Update Complete
done 9.954817Sec 0.165914Min
fmadio@fmadio100v2-228U:~$

```

### Step 3

Next install the firmware image as follows

```
sudo firmware_install.lua --install fmadio100v2_20230307_1925.bin
```

Example output looks similar to this

```
fmadio@fmadio100v2-228U:~$ sudo firmware_install.lua --install fmadio100v2_20230307_1925.bin
fmad fmadlua Mar  9 2023 (/usr/local/bin/fmadiolua /opt/fmadio/bin/firmware_install.lua --install fmadio100v2_20230307_1925.bin )
Open fSysCapture_t* [/opt/fmadio/status/capture:0/100]
FW [fmadio100v2] System[fmadio100v2]
PreInstall script [/mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.pre.lua]
fmad fmadlua Mar  9 2023 (/opt/fmadio/bin/fmadiolua /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.pre.lua )
calibrating...
0 : 2095079878           2.0951 cycles/nsec offset:4.920 Mhz
Cycles/Sec 2095079878.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
done 0.000041Sec 0.000001Min
PreInstall: pre install script
PreInstall: PREINSTALL_GOOD
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.tcz /mnt/nvme0n1p1//tce/optional/fmadio100v2_current.tcz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.core /mnt/nvme0n1p1//boot/fmadio100v2-corepure64.gz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.kernel /mnt/nvme0n1p1//boot/vmlinuz64]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.mydata /mnt/nvme0n1p1//tce/mydata.tgz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.syslinux.cfg /mnt/nvme0n1p1//boot/syslinux/syslinux.cfg]
Bitstream Config [2x40G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.rom.2x100G /mnt/nvme0n1p1//boot/bitstream.rom.2x100G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.rom.2x40G /mnt/nvme0n1p1//boot/bitstream.rom.2x40G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.rom.8x10G /mnt/nvme0n1p1//boot/bitstream.rom.8x10G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.rom.2x40G /mnt/nvme0n1p1//boot/bitstream.rom]
os[sudo /opt/fmadio/bin/bitstream_update.lua --noreboot --write /mnt/nvme0n1p1//boot/bitstream.rom]
fmad fmadlua Mar  9 2023 (/opt/fmadio/bin/fmadiolua /opt/fmadio/bin/bitstream_update.lua --noreboot --write /mnt/nvme0n1p1//boot/bitstream.rom )
calibrating...
0 : 2095079660           2.0951 cycles/nsec offset:4.920 Mhz
Cycles/Sec 2095079660.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
cp: '/mnt/nvme0n1p1//boot/bitstream.rom' and '/mnt/nvme0n1p1//boot/bitstream.rom' are the same file
done 0.136745Sec 0.002279Min
/mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.post.lua
fmad fmadlua Mar  9 2023 (/opt/fmadio/bin/fmadiolua /mnt/nvme0n1p1//firmware/fmadio100v2_20230307_1925.post.lua )
calibrating...
0 : 2095082226           2.0951 cycles/nsec offset:4.918 Mhz
Cycles/Sec 2095082226.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
PostInstall: post install script
done 0.001404Sec 0.000023Min
PostInstall: POSTINSTALL_GOOD
Firmware Install Complete
done 4.144247Sec 0.069071Min
fmadio@fmadio100v2-228U:~$
```

### Step 4

Reboot the system, it make take up to 5 minutes. The system will reboot 2 times, once for  fpga update, and again for the final pass.

### Step 5

Verify the firmware version is correct

```
cat /opt/fmadio/version
```

Example output

```
fmadio@fmadio100v2-228U:~$ cat /opt/fmadio/version
fmadio100v2
8545
Thu Mar  9 16:49:20 2023
fmadio@fmadio100v2-228U:~$

```

### Finished

Its possible when differrence in firmware version numbers is extremely large there may be additional steps. Please contact support@fmad.io for assistance in such cases.
