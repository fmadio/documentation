# Authentication

In many environments different Authentication is required. By default FMADIO capture systems using built in BASIC authentication over HTTP. As this makes configuration and setup simple but is very weak security setting.

### Supported Authentication

* BAISC (insecure)
* HTTS Only + BASIC
* Single Sign On (SSO)
* RADIUS
* LDAP

## HTTPS Only

By default HTTP and HTTPS are enabled on the GUI. In any security setting HTTP needs to be disabled, as its an unsecure protocol. To disable HTTP edit the config file

```
/opt/fmadio/etc/time.lua
```

Find the "Security" section as follows

```
["Security"] =
{
    ["HTTPAccess"] = "enable",
    ["Auth"] = "BASIC",
    ["ConfigAccess"] = "full",
    ["GUIMode"] = "full",
},

```

Change the "HTTPAccess" section from "enable" to false  as follows

```
["HTTPAccess"] = false,
```

Save the file&#x20;

Then restart nginx as follows

```
sudo killall nginx
```

NGINX will restart automatically within 60 seconds with the updated configuration. Only HTTPS access is possible.

## Single Sign On

SSO configuration is more complicated, please contact support@fmad.io and we can walk you thru the setup personally

## RADIUS

**FW: 7563+**

We support RADIUS authentication using the freeradius client. Configuration is as follow

**STEP 1)**

Edit the configuration file&#x20;

```
/opt/fmadio/etc/time.lua
```

**STEP 2)**

Find the "Security" section, example shown below

```
["Security"] =
{
    ["HTTPAccess"] = false,
    ["Auth"] = "BASIC",
    ["ConfigAccess"] = "full",
    ["GUIMode"] = "full",
    ["RADIUS_Secret"] = "testing123",
    ["RADIUS_Host"] = "192.168.1.1",
    ["RADIUS_Protocol"] = "PPP",
    ["RADIUS_Timeout"] = 86400000000000,
},

```

**STEP 3)**

Change the following, this disabled the HTTP protocol&#x20;

```
["HTTPAccess"] = false,
```

Changes the following, this enables RADIUS as the authentication method

```
["Auth"] = "RADIUS",
```

Configure your RADIUS login information

```
["RADIUS_Secret"] = "testing123",
["RADIUS_Host"] = "192.168.1.1",
["RADIUS_Protocol"] = "PPP",
```

Finally the Timeout, this is how long the system waits until it will automatically logout the user and requirement them to re-authenticate. Value is in nanoseconds, scientific notation and formula is no problem. Per below, 24 hours \* 60 min \* 60 sec \* 1e9 (nanos)

```
    ["RADIUS_Timeout"] = 24*60*60*1e9,

```

**STEP 4)**

Restart nginx as follows, it will re-spawn within 60sec automatically

```
sudo killall nginx
```

**STEP 5)**

You should see a login page when accessing FMADIO as follows

![](<../.gitbook/assets/image (80).png>)

**TROUBLESHOOTING**

If there is some problems, please confirm on CLI using radclient, example as follows.

```
fmadio@fmadio100v2-228U:$ echo "User-Name = steve" | radclient <radius server ip>:1812 auth testing123
Sent Access-Request Id 95 from 0.0.0.0:56527 to 192.168.2.132:1812 length 27
Received Access-Reject Id 95 from 192.168.2.132:1812 to 192.168.2.175:56527 length 20
(0) -: Expected Access-Accept got Access-Reject
fmadio@fmadio100v2-228U:$

```

## LDAP

Please contact support@fmad.io we can walk you thru the configuration and setup personally
