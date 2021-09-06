---
description: System Firmware Update
---

# Firmware Update \(CLI\)

Upload Firmware into the system is the following process

1. scp or cul the \*.bin firmware file to the system
2. upload the firmware into the system
3. install the firmware on the system
4. reboot the system

Its fairly straight forward to do, in many instances CLI based update is easier than a GUI 

### 1\)  Copy firmware to the system

This is either scp or curl -O the firmware to the home directory, example below uses curl directly from the webpage

### 2\) Upload the firmware into the system

While the .bin file may be on the system, It needs to be uploaded and processed by the system to make it accessible. Using the following command line on the FW file from 1\)

```text
fmadio@fmadio20v3-287:~$ sudo firmware_install.lua --upload <full firware filename>
```

NOTE: the firmware filename must be exactly as downloaded, e.g. no \(1\) or other suffix appended.

Below example filename is "fmadio20v3\_20210831\_1136.bin"

Example upload process

```text
fmadio@fmadio20v3-287:~$ sudo firmware_install.lua --upload fmadio20v3_20210831_1136.bin
fmad fmadlua Aug 31 2021
calibrating...
0 : 2095081812           2.0951 cycles/nsec offset:4.918 Mhz
Cycles/Sec 2095081812.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv /usr/local/bin/fmadiolua
argv fmadio20v3_20210831_1136.bin
loading filename [/opt/fmadio/bin/firmware_install.lua]
Upload Firmware [fmadio20v3_20210831_1136.bin]
mkdir: can't create directory '/mnt/store0/tmp/fw/': File exists
Filename [fmadio20v3_20210831_1136]
unpack FW
unpack: [fmadio20v3_20210831_1136.core]
unpack: [fmadio20v3_20210831_1136.kernel]
unpack: [fmadio20v3_20210831_1136.mydata]
unpack: [fmadio20v3_20210831_1136.post.lua]
unpack: [fmadio20v3_20210831_1136.pre.lua]
unpack: [fmadio20v3_20210831_1136.rom.2x10G]
unpack: [fmadio20v3_20210831_1136.rom.2x1G]
unpack: [fmadio20v3_20210831_1136.sign]
unpack: [fmadio20v3_20210831_1136.syslinux.cfg]
unpack: [fmadio20v3_20210831_1136.syslinux.cfg.analytics]
unpack: [fmadio20v3_20210831_1136.tcz]
check TCZ
check sign
verify signature
sign: gpg: WARNING: unsafe ownership on homedir `/home/fmadio/.gnupg/'
sign: 5c154ff9bc902ac6d4d44b1fa0d60f33  fmadio20v3_20210831_1136.tcz
sign: b6cad8c08e51fd80b412b6c1602af4b4  fmadio20v3_20210831_1136.pre.lua
sign: 180fe4e475b521f7bf36f7f9d2acc6c8  fmadio20v3_20210831_1136.post.lua
sign: 31d280c2c5585b25d0585a67a1021853  fmadio20v3_20210831_1136.core
sign: a98281879ffc8040dc0a78f1669cb647  fmadio20v3_20210831_1136.kernel
sign: 4fa3ff6d2087f9797cad6aee2df0aea9  fmadio20v3_20210831_1136.mydata
sign: gpg: Signature made Tue Aug 31 11:37:07 2021 JST using RSA key ID 35173534
sign: gpg: checking the trustdb
sign: gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
sign: gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
sign: gpg: Good signature from "fmadio <support@fmad.io>" [ultimate]
Found Signatures:
fmadio20v3_20210831_1136.tcz : 5c154ff9bc902ac6d4d44b1fa0d60f33  fmadio20v3_20210831_1136.tcz
fmadio20v3_20210831_1136.post.lua : 180fe4e475b521f7bf36f7f9d2acc6c8  fmadio20v3_20210831_1136.post.lua
fmadio20v3_20210831_1136.kernel : a98281879ffc8040dc0a78f1669cb647  fmadio20v3_20210831_1136.kernel
fmadio20v3_20210831_1136.mydata : 4fa3ff6d2087f9797cad6aee2df0aea9  fmadio20v3_20210831_1136.mydata
fmadio20v3_20210831_1136.core : 31d280c2c5585b25d0585a67a1021853  fmadio20v3_20210831_1136.core
fmadio20v3_20210831_1136.pre.lua : b6cad8c08e51fd80b412b6c1602af4b4  fmadio20v3_20210831_1136.pre.lua
Signatures good
[fmadio20v3_20210831_1136.tcz  ] Expect:(5c154ff9bc902ac6d4d44b1fa0d60f33  fmadio20v3_20210831_1136.tcz) Calc:(5c154ff9bc902ac6d4d44b1fa0d60f33  fmadio20v3_20210831_1136.tcz)
[fmadio20v3_20210831_1136.post.lua] Expect:(180fe4e475b521f7bf36f7f9d2acc6c8  fmadio20v3_20210831_1136.post.lua) Calc:(180fe4e475b521f7bf36f7f9d2acc6c8  fmadio20v3_20210831_1136.post.lua)
[fmadio20v3_20210831_1136.kernel] Expect:(a98281879ffc8040dc0a78f1669cb647  fmadio20v3_20210831_1136.kernel) Calc:(a98281879ffc8040dc0a78f1669cb647  fmadio20v3_20210831_1136.kernel)
[fmadio20v3_20210831_1136.mydata] Expect:(4fa3ff6d2087f9797cad6aee2df0aea9  fmadio20v3_20210831_1136.mydata) Calc:(4fa3ff6d2087f9797cad6aee2df0aea9  fmadio20v3_20210831_1136.mydata)
[fmadio20v3_20210831_1136.core ] Expect:(31d280c2c5585b25d0585a67a1021853  fmadio20v3_20210831_1136.core) Calc:(31d280c2c5585b25d0585a67a1021853  fmadio20v3_20210831_1136.core)
[fmadio20v3_20210831_1136.pre.lua] Expect:(b6cad8c08e51fd80b412b6c1602af4b4  fmadio20v3_20210831_1136.pre.lua) Calc:(b6cad8c08e51fd80b412b6c1602af4b4  fmadio20v3_20210831_1136.pre.lua)
Firmware is valid
Firmware Copy Took 1.473866 sec
Firmware Update Complete
done 18.199007Sec 0.303317Min
fmadio@fmadio20v3-287:~$ sudo reboot

```

### 3\) Firmware Install

Next the firmware needs to be installed using the command

```text
$ sudo firmware_install.lua --install <firmware filename.bin>
```

Example output is shown below. Note if the capture is running as shown, it will eventually timeout and complete the update.

```text
fmadio@fmadio20v2-149:$ sudo firmware_install.lua --install fmadio20v2_20210906_1606.bin
fmad fmadlua Jul 21 2021
calibrating...
0 : 2100015957           2.1000 cycles/nsec offset:0.016 Mhz
Cycles/Sec 2100015957.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv /usr/local/bin/fmadiolua
argv --install
argv fmadio20v2_20210906_1606.bin
loading filename [/opt/fmadio/bin/firmware_install.lua]
Install Firmware [fmadio20v2_20210906_1606.bin]
Open fSysCapture_t* [/opt/fmadio/status/capture:0/100]
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
Force capture to exit
FW [fmadio20v2] System[fmadio20v2]
PreInstall script [/mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.pre.lua]
fmad fmadlua Jul 21 2021
calibrating...
0 : 2100015531           2.1000 cycles/nsec offset:0.016 Mhz
Cycles/Sec 2100015531.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv /opt/fmadio/bin/fmadiolua
loading filename [/mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.pre.lua]
PreInstall: pre install script
PreInstall: PREINSTALL_GOOD
done 0.000086Sec 0.000001Min
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.tcz /mnt/nvme0n1p1//tce/optional/fmadio20v2_current.tcz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.core /mnt/nvme0n1p1//boot/fmadio20v2-corepure64.gz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.kernel /mnt/nvme0n1p1//boot/vmlinuz64]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.mydata /mnt/nvme0n1p1//tce/mydata.tgz]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.syslinux.cfg /mnt/nvme0n1p1//boot/syslinux/syslinux.cfg]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.rom.2x1G /mnt/nvme0n1p1//boot/bitstream.rom.2x1G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.rom.2x10G /mnt/nvme0n1p1//boot/bitstream.rom.2x10G]
Bitstream Config [2x10G]
Copy [cp /mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.rom.2x10G /mnt/nvme0n1p1//boot/bitstream.rom]
/mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.post.lua
fmad fmadlua Jul 21 2021
calibrating...
0 : 2100016641           2.1000 cycles/nsec offset:0.017 Mhz
Cycles/Sec 2100016641.0000 Std:       0 cycle std(  0.00000000) Target:2.10 Ghz
argv /opt/fmadio/bin/fmadiolua
loading filename [/mnt/nvme0n1p1//firmware/fmadio20v2_20210906_1606.post.lua]
PostInstall: post install script
PostInstall: POSTINSTALL_GOOD
done 0.003273Sec 0.000055Min
Firmware Install Complete
done 34.044320Sec 0.567405Min
fmadio@fmadio20v2-149:$ 
```

### 4\) Reboot 

System needs to be rebooted, It will power cycle also after the 1st reboot to complete the update process.

Example

```text
$ sudo reboot
Connection to 192.168.2.145 closed by remote host.

```

