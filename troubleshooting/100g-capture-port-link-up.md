# 100G Capture Port Link Up

**FW: 8224+**

Getting 100G links link up either works no problem out of the box or be fickle to setup and get a link.

Usually the main source of problems is FEC vs non-FEC setting on both the switch and on the FMADIO system. FMADIO Packet Capture systems will try detect if FEC is on the link by alternating FEC enable / FEC disabled when trying to link up.

This can cause problems if the switch its connected to is also cycling thru the different combinations.

To check the current status the fmadiocli utility provides the easiest way to monitor the state. Use the command, below to get the current link state.

```
show interface status
```

![](<../.gitbook/assets/image (9) (2).png>)

In the above we see capture port 0 (cap0) is link down and capture port 1(cap1) is link up

The link may be bounced by shutting down using the command as follows

```
config interface shutdown cap0
config interface shutdown cap1
```

It may take 1min for the link to go down, use the show interface status command to wait for link down to complete.

![](<../.gitbook/assets/image (2) (2).png>)

Once the link is down, disable the shutdown using the command

```
config interface no shutdown cap0
config interface no shutdown cap1
```

#### NOTE: status currently takes 30-60sec to update. Please be patient.

### FEC Link Config

The default setting of FMADIO Packet Capture system is FEC running in "auto" mode. This means the system will try link up without FEC, if it fails FEC is enabled and link up is tried again. This process is repeated indefinitely.

For some switchs and NICs this works for others it has problems. e.g. if both end points are cycling like this its possible it will ever link up due to phase/offset of the cycles.

Its best to force the FEC setting if your expecting FEC to run on the link. This is done as follows

```
config interface fec cap0
config interface fec cap1
```

![](<../.gitbook/assets/image (7) (1) (2).png>)

After forcing FEC on the links the interface status will show a "force-" option as follows

![](<../.gitbook/assets/image (5) (2).png>)

This indicates FEC has been forced on for the specific interfaces.

NOTE: bouncing the port on the switch may be required, it depends what state the switch port is. It may have entered an error mode, or backed off on the autoneg. Bouncing the port on the switch most of the time will resolve the issue.

### Additional Link Up Debug

If the link is still failing to link up, you can trace the state machine for link up procedure using the command

```
fnic_test  --trans-trace -v
```

By default it runs on cap0  using the --port 1 can run the trace on the other port. This prints out the realtime event history of the link up process, below is an example of a link bounce.

![](<../.gitbook/assets/image (8) (2).png>)

In the above output we see the link state go from State 1 -> State 7. Where state 7 is a stable link up.

For additional assistance on link problems, please run the above and pipe to a logfile. Send the logfile to support @ fmad . io for further assistance.
