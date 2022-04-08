# Network Traffic Profile

Generating the network traffic profile can help debug many issues

FMADIO Capture System has built in utility "capinfos2" which can provide fast and compact summary information about a PCAP.,

For network profile information please run as follows

```
// Some code
sudo stream_cat <insert capture name> | capinfos2 -v --size-histo
```

This will generate a packet size histogram such as the following

![](<../.gitbook/assets/image (123).png>)

With a more compact histogram version shown below

![](<../.gitbook/assets/image (119).png>)

Finally at the end there is a number list, which can be sent to FMADIO Support so we can replicate your network traffic profile locally with our synthetic packet generator.

![](<../.gitbook/assets/image (122).png>)
