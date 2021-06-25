# Management Port Layout

## Configure 40G Management Port  

**ANALYTICS SKU only**

40G management ports can run in 2 x 40G or 4 x 10G mode or 2 x 2 x 10G The utility is located in

```text
/opt/fmadio/drivers/current/qcu64e
```

### Get the current setting as follows

```text
fmadio@fmadio100v2-228U:/opt/fmadio/drivers/4.9.247-tinycore64$ sudo ./qcu64e
Intel(R) QSFP+ Configuration Utility

QCU version: v2.34.17.03
Copyright(C) 2014 - 2019 by Intel Corporation.
Software released under Intel Proprietary License.

NIC Seg:Bus Ven-Dev   Mode     Adapter Name
=== ======= ========= ======== =================================================
 1) 000:026 8086-1583 2x40     Intel(R) Ethernet Server Adapter XL710-Q2OCP

Warning: No adapter selected.
fmadio@fmadio100v2-228U:/opt/fmadio/drivers/4.9.247-tinycore64$

```

### To check the available port modes 

To query what port configuration modes are avaliable use

```text
 sudo ./qcu64e /nic=1 /info
```

 As shown below

```text
Intel(R) QSFP+ Configuration Utility

QCU version: v2.34.17.03
Copyright(C) 2014 - 2019 by Intel Corporation.
Software released under Intel Proprietary License.

Adapter supports QSFP+ Configuration modification.
Current Configuration: 2x40

Supported Configurations:
 1x40
 2x40
 4x10
 2x2x10A
 2x2x10B
fmadio@fmadio100v2-228U:/opt/fmadio/drivers/4.9.247-tinycore64$
```

### Set a specific Port mode

Changing the port is simply

```text
qcu64e /nic=1 /set=4x10
```

As follows

```text
$ sudo ./qcu64e /nic=1 /set=4x10
Intel(R) QSFP+ Configuration Utility

QCU version: v2.34.17.03
Copyright(C) 2014 - 2019 by Intel Corporation.
Software released under Intel Proprietary License.

Changing the configuration...
Done.
Restart the system to apply the changes.
fmadio@fmadio100v2-228U:/opt/fmadio/drivers/4.9.247-tinycore64$

```

Then restart the system

