# Authentication

In many environments different Authentication is required. By default FMADIO capture systems using built in BASIC authentication over HTTP. As this makes configuration and setup simple but is very weak security setting.

## Supported Authentication

* BAISC (insecure)
* HTTPS Only + BASIC&#x20;
* RADIUS
* Active Directory (SSO via OAUTH 2.0)
* Google Cloud (SSO via OAUTH 2.0)
* Ping Identity Cloud (SSO via OAUTH 2.0)

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

### Public IP Testing

In the follow example we have used a reverse SSH tunnel to temporarily put FMADIO system on a public IP, as Azure Active Directory services require internet accessible devices for the redirect\_uri.&#x20;

For an On Premise Active Directory server this is not required.&#x20;

Example Reverse SSH Tunnel

```
 ssh -R 8888:192.168.1.100:443 ec2-user@aws-instance.compute.amazonaws.com
```

NOTE: SSH tunnel should not use localhost, as all localhost sourced requests bypass authentication. Instead use the IP address of the management interface

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

![](<../.gitbook/assets/image (118) (1) (1) (1).png>)

the "client\_id" is the shown below

![](<../.gitbook/assets/image (124) (1) (1) (1) (1) (1).png>)

The "client\__secret" in the above config needs to be the Value shown below, not the secretID_

![](<../.gitbook/assets/image (126) (1) (1).png>)

Finally the "redirect\_uri" needs to be registered as follows.

![](<../.gitbook/assets/image (119) (1) (1) (1) (1).png>)

Once config is complete, please confirm no syntax errors by running&#x20;

```
fmadiolua /opt/fmadio/etc/oauth_opts.lua
```

Correct output is as follows, if there are any syntax errors please correct.

![](<../.gitbook/assets/image (115) (1) (1).png>)

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically

```
sudo killall nginx
```

### Logging in

Next point a browser to the FMADIO device, it should redirect you to the Active Directory login page as follows.

![](<../.gitbook/assets/image (121) (1) (1).png>)

Login to the system using your Azure / Microsoft credentials. Then the FMADIO device dashboard will be shown as below

![](<../.gitbook/assets/image (127) (1) (1) (1).png>)

### Logout

Logout is the same, using the logout button shown below

![](<../.gitbook/assets/image (90) (1) (1) (1).png>)

Then choose an account to sign out of

![](<../.gitbook/assets/image (120) (1) (1) (1) (1).png>)

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

![](<../.gitbook/assets/image (90) (1) (1).png>)

Then fill in the information, as follows. Google is a bit more strict and requires TLD endpoints not raw IPs

![](<../.gitbook/assets/image (80).png>)

Which results in the following secret information

![](<../.gitbook/assets/image (115) (1).png>)

Update the oauth\_opts.lua file above with the information

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically

```
sudo killall nginx
```

### Logging In

Next point the browser to the FMADIO device and it will redirect to Google Sign in account&#x20;

![](<../.gitbook/assets/image (118) (1) (1).png>)

Login using your Google account information, and it will re-direct you to the FMADIO dashboard.

![](<../.gitbook/assets/image (124) (1) (1) (1) (1).png>)

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

![](<../.gitbook/assets/image (117) (1).png>)

These fields are mapped directly into the oauth\_opts.lua configuration file above.

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process as below. It will reswpan on a 1min cron job automatically.

```
sudo killall nginx
```

### Logging In

Next point the browser to the FMADIO device and it will redirect to Ping Idneitty SSO account as follows

![](<../.gitbook/assets/image (119) (1) (1) (1).png>)

After a successful authentication the FMADIO dashboard is seen

![](<../.gitbook/assets/image (129) (1) (1).png>)

&#x20;Any further questions or problems, please contact us support@fmad.io

## PAM LDAP

**FW: 8529+**

FMADIO systems support Linux PAM ( [https://github.com/linux-pam/linux-pam](https://github.com/linux-pam/linux-pam) ) as an authetication method. One option for centralized authentication is to use LDAP via PAM.

1\) First run fmadiocli settings to set the authentication method

[https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-auth](https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-auth)

```
config security auth pam-ldap
```

2\) We also strongly recommend to disable HTTP access as all username / passwords are sent over un-encrypted HTTP

[https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-http](https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-http)

```
config security http false
```

3\) Configure LDAP client nslcd. Copy the default config file as follows

```
cp /opt/fmadio/etc_ro/nslcd.conf /opt/fmadio/etc/nslcd.conf
```

The default config looks like the following

```
fmadio@fmadio100v2-228U:~$ cat /opt/fmadio/etc_ro/nslcd.conf
# /etc/nslcd.conf
# nslcd configuration file. See nslcd.conf(5)
# for details.
#log none debug

# The user and group nslcd should run as.
uid root
gid root

# The location at which the LDAP server(s) should be reachable.
uri ldap://192.168.1.100

# The search base that will be used for all queries.
base dc=fmad,dc=io

# The LDAP protocol version to use.
#ldap_version 3

# The DN to bind with for normal lookups.
#binddn cn=annonymous,dc=example,dc=net
#bindpw secret

# The DN used for password modifications by root.
#rootpwmoddn cn=admin,dc=example,dc=com

# SSL options
#ssl off
#tls_reqcert never
#tls_cacertfile /etc/ssl/certs/ca-certificates.crt

# The search scope.
#scope sub

#ssl start_tls
#tls_reqcert allow

fmadio@fmadio100v2-228U:~$

```

Modify the uri, base and any other LDAP specific configs to the enviroment and save it

4\) reboot system

5\) check LDAP connectivity

Changing the username/domain/ip address etc to match your environment

```
ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
```

Successful authentication looks like the following

```
fmadio@fmadio100v2-228U:~$ ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
dn:cn=fmadio-user,dc=fmad,dc=io
```

Once this is working, both SSH, WWW-Admin and WWW-User LDAP posix group members can login to the system.

The LDAP posixGroups are

fmadio-ssh-admin  - for SSH access

fmadio-www-admin - for WWW admin access (can change anything)

fmadio-www-user - for WWW user access (monitoring and pcap downloading)

6\) Both SSH and WWW now fully configured using LDAP as centralized authentication

### LDAP Optional

Some environments require a notice when logging in, such as the following

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

This can be customized as follows

1\) copy the default template&#x20;

```
cp /opt/fmadio/www/login/authorized_access.html.default /opt/fmadio/etc/authorized_access.html
```

2\) Edit the content of

```
/opt/fmadio/etc/authorized_access.html
```

3\) restart nginx

kill nginx and wait 60sec for it to restart

```
sudo killall nginx
```

### Troubleshooting

Configuration usually does not go as planned, as such heres some tips to try

1\) run nslcd in the foreground

```
sudo killall nslcd
sudo /usr/local/sbin/nslcd -f
```

This will check the /etc/nslcd.conf configuration file is working correctly, either config typeo or LDAP server problems.

Once its running ensure local lookups work correctly as follows

```
ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
```

2\) check nginx config files

The nginx logfiles are located in

```
tail -F -n 100 /mnt/store0/log/nginx_error.log
```

Any errors there might help understand the issues

3\) check syslog file for PAM logs

```
tail -F -n 100 /mnt/store0/log/messages |grep -i pam
```

This will print out logs of all PAM messages and may help debugging
