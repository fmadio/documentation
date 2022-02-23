# PCAP Upload

All FMADIO Packet capture systems can also uploaded raw 3rd party PCAP files into the system. This allows Packetscope, Tcpscope, Analysis and Replay plugins to work on external and archived historical data. The upload functionality is heavily used internally for our own testing and regression frameworks.

Please note:

**Capturing must be stopped. Running Capture and Upload simultaneously results in undefined behavior**

## Upload Local PCAP CLI

If the PCAP your uploading is small, you can

**Step 1)**

scp the PCAP onto the OS disk. e.g. /mnt/store0/tmp2/

```
scp upload.pcap fmadio@192.168.1.1:/mnt/store0/tmp2/
```

\
**Step 2)**

Upload using the utility **stream\_upload**. The upload fetchs data via stdin allowing a wide range of options from a local PCAP file, to remote PCAP, to a curl URL or PCAP generation utility running on the system. The following example is a simple upload a PCAP thats on the local filesystem.

```
fmadio@fmadio20-049:/mnt/store0/tmp2$ cat hitcon_small.pcap | sudo stream_upload --name test_upload 
FSPrefetch
FSPrefetch Chunk: 5000.184ms 5000ms
FSPrefetch Chunk timeoued: 5000.000ms max
create stream [test_upload_20170725_1625]
     0.178GB Uploaded
     0.610GB Uploaded
     0.864GB Uploaded
     1.116GB Uploaded
     1.335GB Uploaded
     1.472GB Uploaded
     1.667GB Uploaded
     1.793GB Uploaded
     1.947GB Uploaded
     2.225GB Uploaded
fmadio@fmadio20-049:/mnt/store0/tmp2$
```

Note: the timestamp resolution of the uploaded PCAP is automatically detected and converted to FMADIO native nanosecond format.

**Step 3)**

Confirm upload.

```
fmadio@fmadio20-049:/mnt/store0/tmp2$ sudo stream_dump
Streams:
 [0000]    [this should be empty]      0GB Chunk(Cnt:       0 Start:       1 End:       0) Inv:-nan Cap:-nan CacheI:-nan Cache:-nan Disk:-nan Drop:-nan Pkt:0
 [0001] test_upload_20170725_1625      2GB Chunk(Cnt:    9341 Start:       8 End:    9348) Inv:0.000 Cap:0.000 CacheI:0.000 Cache:0.000 Disk:1.000 Drop:0.000 Pkt:10851045

```

## Upload Remote PCAP CLI

Sometimes you need to upload very large multi TB PCAP to the FMADIO Packet Capture System. In such cases there isn't enough local storage on the OS disk for the scp method to work. To upload a large PCAP use the streaming/pipe functionality of the stream\_upload utility.&#x20;

In this example we are uploading a raw PCAP over SSH into the FMADIO Packet Capture system. From an SSH shell on the capture system the command SSH\`s into the remote system where the PCAP is stored and issues a "cat" command on the PCAP to be uploaded.&#x20;

Effectively piping the remote PCAP down the ssh connection. This is then read by the stream\_upload command in --stdin mode, instead of reading from the local file system. For maximum performance its best to use the 10G management port for the connection.

```
$ ssh user@10.10.10.10 cat path_to_pcap.pcap | sudo /opt/fmadio/bin/stream_upload --name remote_upload  --stdin
FSPrefetch
FSPrefetch Chunk: 5000.184ms 5000ms
FSPrefetch Chunk timeoued: 5000.000ms max
create stream [remote_upload_20170725_1625]
     0.178GB Uploaded
     0.610GB Uploaded
     0.864GB Uploaded
     1.116GB Uploaded
     1.335GB Uploaded
     1.472GB Uploaded
     1.667GB Uploaded
     1.793GB Uploaded
     1.947GB Uploaded
     2.225GB Uploaded
        .      
        .      
        .      
        .      
fmadio@fmadio20-049:~$ 
```

Using this approach the PCAP is streamed onto the system via SSH, with no temporarily files created. The maximum PCAP that can be uploaded is limited by the capture systems total storage capacity.
