# Serial Port Access over LAN

FMADIO packet capture systems all have serial port either virtual or physical connectors. Typically serial port access over LAN is the preferred method as it provides an out of bands interface to the system that only requires a SSH terminal \(No KVM or HTM or Java\).

Serial port access is archived using the IPMITOOL located at the following link:

[https://github.com/ipmitool/ipmitool](https://github.com/ipmitool/ipmitool)

We are using IPMITOOL 1.8.16 in this example

```text
aaron@ingress:~$ ipmitool -V
ipmitool version 1.8.16
```

Start by checking the system status via IPMITOOL with the sdr argument. Contact support for the default password and replace the IP address \(192.168.1.123\) with the IP address of the BMC device

```text
aaron@ingress:~$ ipmitool -U admin -P XXXXXXX -H 192.168.1.123 sdr
Watchdog         | 0x00              | ok
SEL              | 0x00              | ok
BPB_FAN_1A       | 18750 RPM         | ok
BPB_FAN_1B       | 15000 RPM         | ok
BPB_FAN_2A       | 18600 RPM         | ok
BPB_FAN_2B       | 15000 RPM         | ok
BPB_FAN_3A       | 18300 RPM         | ok
BPB_FAN_3B       | 15000 RPM         | ok
BPB_FAN_4A       | 18600 RPM         | ok
BPB_FAN_4B       | 15000 RPM         | ok
BPB_FAN_5A       | 18600 RPM         | ok
BPB_FAN_5B       | 15000 RPM         | ok
BPB_FAN_6A       | 18450 RPM         | ok
BPB_FAN_6B       | 14850 RPM         | ok
BPB_FAN_7A       | 18600 RPM         | ok
BPB_FAN_7B       | 15000 RPM         | ok
BPB_FAN_8A       | 18600 RPM         | ok
BPB_FAN_8B       | 14850 RPM         | ok
CPU0_Status      | 0x00              | ok
CPU1_Status      | 0x00              | ok
CPU0_TEMP        | 34 degrees C      | ok
CPU1_TEMP        | 33 degrees C      | ok
DIMMG0_TEMP      | 28 degrees C      | ok
DIMMG1_TEMP      | 29 degrees C      | ok
DIMMG2_TEMP      | 29 degrees C      | ok
DIMMG3_TEMP      | 29 degrees C      | ok
CPU0_DTS         | 61 degrees C      | ok
CPU1_DTS         | 62 degrees C      | ok
HDD_TEMP_0       | 22 degrees C      | ok
HDD_TEMP_1       | 22 degrees C      | ok
INLET_AIR_TEMP   | 24 degrees C      | ok
MB_TEMP1         | 29 degrees C      | ok
MB_TEMP2         | 31 degrees C      | ok
NVMeG0_TEMP      | no reading        | ns
NVMeG1_TEMP      | no reading        | ns
NVMeG2_TEMP      | 29 degrees C      | ok
NVMeG3_TEMP      | no reading        | ns
NVMeG4_TEMP      | no reading        | ns
NVMeG5_TEMP      | no reading        | ns
PCH_TEMP         | 34 degrees C      | ok
PS1_Status       | 0x00              | ok
PS2_Status       | 0x00              | ok
PSU1_HOTSPOT     | no reading        | ns
PSU2_HOTSPOT     | 32 degrees C      | ok
P_12V            | 12.03 Volts       | ok
P_1V0_AUX_LAN    | 0.99 Volts        | ok
P_1V05_PCH       | 1.02 Volts        | ok
P_1V8_AUX_PCH    | 1.78 Volts        | ok
P_3V3            | 3.24 Volts        | ok
P_5V             | 4.93 Volts        | ok
P_5V_STBY        | 4.91 Volts        | ok
P_VBAT           | 2.93 Volts        | ok
P_VCCIN_CPU0     | 1.75 Volts        | ok
P_VCCIN_CPU1     | 1.75 Volts        | ok
P_VDDQ_CPU0_ABC  | 1.20 Volts        | ok
P_VDDQ_CPU0_DEF  | 1.20 Volts        | ok
P_VDDQ_CPU1_ABC  | 1.20 Volts        | ok
P_VDDQ_CPU1_DEF  | 1.20 Volts        | ok
P_VNN_PCH_AUX    | 0.97 Volts        | ok
SYS_POWER        | 350 Watts         | ok
aaron@ingress:~$

```

In the above we see the sensor status of the FMADIO device, thus the tool is connecting to the device and BMC interface is working correctly.

To connect to the serial port use the following command

ipmitool -U admin -P &lt;password&gt; -H &lt;host IP address&gt; -I lanplus sol activate

Example as below

```text
aaron@ingress:~$ ipmitool -U admin -P XXXX -H 192.168.1.123 -I lanplus sol activate
[SOL Session operational.  Use ~? for help]

fmadio100v2
fmadio100v2-228U login:
fmadio100v2
fmadio100v2-228U login:
```

And you now have full serial port access to the system.

### Troubleshooting

The serial port is mutually exclusive access, e.g.. only one user at a time can access it. If a previous session is still connected the following will be shown

```text
aaron@ingress:~$ ipmitool -U admin -P XXXXX -H 192.168.1.123 -I lanplus sol activate
Info: SOL payload already active on another session
aaron@ingress:~$
```

In such cases, please disconnect the previous session as follows

```text
aaron@ingress:~$ ipmitool -U admin -P XXXXXX -H 192.168.1.123 -I lanplus sol deactivate
aaron@ingress:~$
```

Then reconnect 

```text
aaron@ingress:~$ ipmitool -U admin -P XXXXXX  -H 192.168.1.123 -I lanplus sol activate
[SOL Session operational.  Use ~? for help]

fmadio100v2
fmadio100v2-228U login:
fmadio100v2
fmadio100v2-228U login:
```

