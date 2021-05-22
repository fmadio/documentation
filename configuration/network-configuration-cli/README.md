# Network Configuration \(CLI\)

Modifying the network configuration setting in a restricted Colocation environment can be far easier to achieve via the command line. The first step is SSH into the system, change to the specified directory and view the current network settings, as shown below



```text
aaron@display0:/tmp$ ssh fmadio@192.168.11.75
fmadio@192.168.11.75's password:
  _____                    .___.__      10G
_/ ____\_____  _____     __| _/|__|  ____
\   __\/     \ \__  \   / __ | |  | /  _ \
 |  | |  Y Y  \ / __ \_/ /_/ | |  |(  <_> )
 |__| |__|_|  /(____  /\____ | |__| \____/
            \/      \/      \/
============================================
  -+ no user serviceable parts inside +-
fmadio@fmadio10-049:~$ cd /mnt/store0/etc
fmadio@fmadio10-049:/mnt/store0/etc$ cat network.lua
-- auto generated on Tue Apr 14 10:38:13 2015
local Config =
{
["sf0"] =
{
    ["Mode"]    = "disabled",
    ["Address"] = "192.168.1.2",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.1.1",
    ["DNS"]     = "192.168.1.1",
},
["sf1"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.12.10",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.12.1",
    ["DNS"]     = "192.168.12.1",
},
["eth0"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.11.75",
    ["Netmask"] = "255.255.255.0",
    ["Gateway"] = "192.168.11.1",
    ["DNS"]     = "192.168.11.1",
},
["bmc"] =
{
    ["Mode"]    = "static",
    ["Address"] = "192.168.11.73",
    ["Netmask"] = "255.255.255.255",
    ["Gateway"] = "192.168.11.1",
    ["DNS"]     = "192.168.11.1",
},
}
return Config
```



In the example configuration file above, the network ports are mapped as follows

