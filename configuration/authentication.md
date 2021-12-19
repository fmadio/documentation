# Authentication

In many environments different Authentication is required. By default FMADIO capture systems using built in BASIC authentication over HTTP. As this makes configuration and setup simple but is very weak security setting.

### Supported Authentication

* BAISC (insecure)
* HTTS Only + BASIC
* RADIUS
* Active Directory (SSO via OAUTH 2.0)
* Google Cloud (SSO via OAUTH 2.0)

## HTTPS Only

By default HTTP and HTTPS are enabled on the GUI. In any security setting HTTP needs to be disabled, as its an unsecure protocol. To disable HTTP edit the config file

### General Config

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

### Restart Nginx

Then restart nginx as follows

```
sudo killall nginx
```

NGINX will restart automatically within 60 seconds with the updated configuration. Only HTTPS access is possible.

SSO configuration is more complicated, please contact support@fmad.io and we can walk you thru the setup personally

## RADIUS

**FW: 7563+**

We support RADIUS authentication using the freeradius client. Configuration is as follow

### **General Config**

Edit the configuration file&#x20;

```
/opt/fmadio/etc/time.lua
```

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

### **Disable HTTP Access**

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

### **Restart Nginx**

Restart nginx as follows, it will re-spawn within 60sec automatically

```
sudo killall nginx
```

### **Login**

You should see a login page when accessing FMADIO as follows

![](<../.gitbook/assets/image (80) (1).png>)

### **TROUBLESHOOTING**

If there is some problems, please confirm on CLI using radclient, example as follows.

```
fmadio@fmadio100v2-228U:$ echo "User-Name = steve" | radclient <radius server ip>:1812 auth testing123
Sent Access-Request Id 95 from 0.0.0.0:56527 to 192.168.2.132:1812 length 27
Received Access-Reject Id 95 from 192.168.2.132:1812 to 192.168.2.175:56527 length 20
(0) -: Expected Access-Accept got Access-Reject
fmadio@fmadio100v2-228U:$

```

## Active Directory (SSO via OAUTH 2.0)

**FW:7608+**

FMADIO Capture devices can authenticate the users using Active Directory via the OAUTH 2.0 protocol. This enable Single Sign On with ADFS.

In the follow example we have used a reverse SSH tunnel to temporarily put FMADIO system on a public IP, as Azure Active Directory services require internet accessible devices for the redirect\_uri. For an On Premise Active Directory server this is not required.

### General Config

Start by editing the general FMADIO configuration file

```
/opt/fmadio/etc/time.lua
```

Then setting HTTP (un-encrypted) access to "disable", and Auth method to "OAUTH", example shown below. The other security fields can be left as is.

```
["Security"] =
{
    ["HTTPAccess"]      = "disable",
    ["Auth"]            = "OAUTH",
.
.
```

Save the file and ensure there are no parse errors by running fmadiolua /opt/fmadio/etc/time.lua

### OAUTH Config

Next create a file name

```
/opt/fmadio/etc/oauth_opts.lua
```

This file contains the ADFS OAUTH End points as follows

```
local config =
{
    redirect_uri                = "https://fmadio100v2-ip-address:8888/secure/",
    discovery                   = "https://login.microsoftonline.com/571b0fe2-75cb-48de-9144-0cb928e90751/v2.0/.well-known/openid-configuration",
    client_id                   = "d41c59e7-6906-4569-9cc0-c6762541d2cd",
    client_secret               = "fSY7Q~dkbG~mHlJYipKiC0XCMhnXQbOkOP5iE",
    ssl_verify                  = "no",
    scope                       = "openid email profile",
    redirect_uri_scheme         = "https",
}

return config
```

These fields are from the ADFS Endpoint URI information, for example as follows. We created a fmadio sign in entry, this has the following client\_id entered above.

The "discovery" config in the above needs to be the OpenID Connect Metadata document, as seen below.

![](<../.gitbook/assets/image (118) (1).png>)

the "client\_id" is the shown below

![](<../.gitbook/assets/image (124) (1).png>)

The "client\__secret" in the above config needs to be the Value shown below, not the secretID_

![](<../.gitbook/assets/image (126).png>)

Finally the "redirect\_uri" needs to be registered as follows.

![](<../.gitbook/assets/image (119) (1).png>)

Once config is complete, please confirm no syntax errors by running&#x20;

```
fmadiolua /opt/fmadio/etc/oauth_opts.lua
```

Correct output is as follows, if there are any syntax errors please correct.

![](<../.gitbook/assets/image (115) (1).png>)

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically

```
sudo killall nginx
```

### Logging in

Next point a browser to the FMADIO device, it should redirect you to the Active Directory login page as follows.

![](<../.gitbook/assets/image (121).png>)

Login to the system using your Azure / Microsoft credentials. Then the FMADIO device dashboard will be shown as below

![](<../.gitbook/assets/image (127).png>)

### Logout

Logout is the same, using the logout button shown below

![](<../.gitbook/assets/image (90) (1).png>)

Then choose an account to sign out of

![](<../.gitbook/assets/image (120) (1).png>)

## Google Cloud (SSO via OAUTH 2.0)

**FW:7608+**

While less practical as its typically for publicly accessible sites, it can be used with a Google Cloud VPC to tunnel authentication requests from a private network to Google Cloud infrastructure.

In this example we just reverse ssh tunnel an FMADIO system onto the public internet (strongly discouraged) for demonstration purposes only.&#x20;

### General Config

Start by editing the general FMADIO configuration file

```
/opt/fmadio/etc/time.lua
```

Then setting HTTP (un-encrypted) access to "disable", and Auth method to "OAUTH", example shown below. The other security fields can be left as is.

```
["Security"] =
{
    ["HTTPAccess"]      = "disable",
    ["Auth"]            = "OAUTH",
.
.
```

Save the file and ensure there are no parse errors by running fmadiolua /opt/fmadio/etc/time.lua

#### OAUTH Config

Next create a file name

```
/opt/fmadio/etc/oauth_opts.lua
```

This file contains the Google Cloud OAUTH End points as follows.&#x20;

```
local config =
{
    redirect_uri                = "https://fmadio100v2-ip-address.com:8888/secure",
    discovery                   = "https://accounts.google.com/.well-known/openid-configuration",
    client_id                   = "431009152914-0jpm7fa0crh619gr48kv4c06h5ibl9ou.apps.googleusercontent.com",
    client_secret               = "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",
    ssl_verify                  = "no",
    scope                       = "openid email profile",
    redirect_uri_scheme         = "https",
}

return config
```

The "clientid" and "client\_secret" need to be replaced with the generated authentication information from google per below. The above is a throw away example only

### Google Credentials

Next generate Google OAUTH credentials as follows.

![](<../.gitbook/assets/image (90).png>)

Then fill in the information, as follows. Google is a bit more strict and requires TLD endpoints not raw IPs

![](<../.gitbook/assets/image (80).png>)

Which results in the following secret information

![](<../.gitbook/assets/image (115).png>)

Update the oauth\_opts.lua file above with the information

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically

```
sudo killall nginx
```

### Logging In

Next point the browser to the FMADIO device and it will redirect to Google Sign in account&#x20;

![](<../.gitbook/assets/image (118).png>)

Login using your Google account information, and it will re-direct you to the FMADIO dashboard.

![](<../.gitbook/assets/image (124).png>)

Any further questions please contact support@fmad.io for assistance.

## Ping Identity (SSO via OUAUTH 2.0)

**FW:7608+**

Ping Identity is a popular onprem authentication system, typically used in large organizations. We support Single Sign On with their product suite, below is an example configuration example setup using the Cloud Services. This example uses a reverse SSH tunnel to put the FMADIO device on a publicly accessible IP (we strongly discourage) for demonstration purposes only, to replicate setting up an On Premise install.

### General Config

Start by editing the general FMADIO configuration file

```
/opt/fmadio/etc/time.lua
```

Then setting HTTP (un-encrypted) access to "disable", and Auth method to "OAUTH", example shown below. The other security fields can be left as is.

```
["Security"] =
{
    ["HTTPAccess"]      = "disable",
    ["Auth"]            = "OAUTH",
.
.
```

Save the file and ensure there are no parse errors by running fmadiolua /opt/fmadio/etc/time.lua

#### OAUTH Config

Next create a file name

```
/opt/fmadio/etc/oauth_opts.lua
```

This file contains the Ping Identity OAUTH End points as follows.&#x20;

```
local config =
{
    redirect_uri                = "https://fmadio100v2-ip-address.com:8888/secure/",
    discovery                   = "https://auth.pingone.asia/9ad4c7dc-4e8c-43e4-9064-3c15b7e34fa5/as/.well-known/openid-configuration",
    client_id                   = "1feb3806-3351-45d2-b385-ea012a4ad1af",
    client_secret               = "3.j0O-pU6TZ.g.MAReW53A2Dv11n~cdTbFINIiyMdkv_dUxH",
    ssl_verify                  = "no",
    scope                       = "openid email profile",
    redirect_uri_scheme         = "https",
}

return config
```

The "clientid" and "client\_secret" need to be replaced with the generated authentication information from Ping Identity interface per below. The above is a throw away example only

### Ping Identity Credentials

We setup a web application using Ping Identity interface as follows. The key fields are shown in red.

![](<../.gitbook/assets/image (117).png>)

These fields are mapped directly into the oauth\_opts.lua configuration file above.

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically.

```
sudo killall nginx
```

### Logging In

Next point the browser to the FMADIO device and it will redirect to Ping Idneitty SSO account as follows

![](<../.gitbook/assets/image (119).png>)

After a successful authentication the FMADIO dashboard is seen

![](<../.gitbook/assets/image (129).png>)

&#x20;Any further questions or problems, please contact us support@fmad.io
