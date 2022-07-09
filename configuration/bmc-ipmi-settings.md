# BMC IPMI Settings

## Disable SSDP

In many cases disabling SSDP is required. This reduces the public visibility of the Packet Capture system.

To do this please use the Redfish API as follows

### Step 1 - Current Network Profile

Get the current BMC Network profile as follows

```
curl -u admin:xxxxxxxx -k "https://{BMC Address}/redfish/v1/Managers/Self/NetworkProtocol" |jq
```

The output will be similar to this

```
fmadio@fmadio100v2-228U:~$ curl -u admin:xxxxxx -k "https://192.168.1.100/redfish/v1/Managers/Self/NetworkProtocol" |jq
{
  "@odata.context": "/redfish/v1/$metadata#ManagerNetworkProtocol.ManagerNetworkProtocol",
  "@odata.etag": "W/\"1655790673\"",
  "@odata.id": "/redfish/v1/Managers/Self/NetworkProtocol",
  "@odata.type": "#ManagerNetworkProtocol.v1_4_1.ManagerNetworkProtocol",
  "Description": "Network Protocol Details",
  "HTTPS": {
    "Port": 443,
    "ProtocolEnabled": true
  },
  "HostName": "AMIE0D55E5D2156",
  "IPMI": {
    "Port": 623,
    "ProtocolEnabled": true
  },
  "Id": "NetworkProtocol",
  "KVMIP": {
    "Port": 443,
    "ProtocolEnabled": true
  },
  "NTP": {
    "NTPServers": [
      "time.nist.gov",
      "pool.ntp.org"
    ],
    "Port": 123,
    "ProtocolEnabled": false
  },
  "Name": "Manager Network Protocol",
  "SNMP": {
    "Port": 161,
    "ProtocolEnabled": true
  },
  "SSDP": {
    "Port": 1900,
    "ProtocolEnabled": false
  },
  "Status": {
    "Health": "OK",
    "State": "Enabled"
  },
  "VirtualMedia": {
    "Port": 443,
    "ProtocolEnabled": true
  }
}
fmadio@fmadio100v2-228U:~$

```

The import part is extracting the ekey required for the next step. In this case the value is  "1655790673"

![Redfish EKey](<../.gitbook/assets/image (124).png>)

### Step 2 : Disable SSDP

Next disable SSDP using the ekey above using the command

```
 curl -u admin:xxxxxxxx -k -X PATCH 
                           -H "Content-Type: application/json" 
                           -H "If-None-Match: {Insert ekey}" 
                            "https://{BMC Address}/redfish/v1/Managers/Self/NetworkProtocol"  
                            -d '{"SSDP":{"ProtocolEnabled": false}}' | jq

```

There is no return value from the call

### Step 3 : Verify SSDP is disabled

Use the same command in Step 1 to check the status of SSDP service. It should now be "false"

```
curl -u admin:xxxxxxxx -k "https://{BMC Address}/redfish/v1/Managers/Self/NetworkProtocol" |jq
```

![](<../.gitbook/assets/image (132).png>)
