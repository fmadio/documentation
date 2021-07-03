# Mount Remote CIFS \(Windows\) Drive

**Requires FW:7256+**

Mounting a remote CIFS/Windows file system on the FMADIO capture device can be extremely useful in many cases. It allows end users to use familiar Windows file browser to access and interface with PCAPs and FMADIO systems.

The main usage for remote CIFS mounts is to provide a simple and robust way to push PCAP data automatically from the capture device to a Windows Share drive. Once on a Windows Share drive it is easily accessible from multiple devices.rr

### CIFS Remote Mount Config

Please edit the configuration file in

```text
/opt/fmadio/etc/disk.lua 
```

Create an entry named "CIFSDisk" as shown below, be careful all punctuation is correct. The mount point should be "remote0" or "remote1" or "remote2" which maps to /mnt/remote0 /mnt/remote1 on the capture device. 

```text
["CIFSDisk"] =
{
    ["//192.168.1.100/share"]    = "remote0:username=user,password=secret",
},
```

Mount options for username and password are specified after a double colon : 

After the configuration file has been updated, run the following command to confirm the config file has no syntax errors.

```text
fmadio@fmadio20v3-287:/mnt/store0/etc$ fmadiolua /opt/fmadio/etc/disk.lua
failed to open self? [fmadiolua]
loading filename [/opt/fmadio/etc/disk.lua]
done 0.000065Sec 0.000001Min
fmadio@fmadio20v3-287:/mnt/store0/etc$
```

Above is the correct operation. Once complete please reboot the system to confirm the new mount points.

