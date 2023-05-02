##  [Authentication ](#authentication)
    
[Supported Authentication](#supported-authentication)
    
[HTTPS only](#https-only)
    
[RADIUS](#radius)

[Active Directory](#active-directory)

[Google Cloud](#google-cloud)
    
[Ping Identity](#ping-identity)
    
[LDAP Optional](#ldap-optional)  
</br>


# Authentication  
[Supported]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Authentication"

In many environments, different Authentication is required. By default, FMADIO captures systems  </br> 
using built-in BASIC authentication over HTTP as this makes configuration and setup simple.  </br> However, it is a very weak security setting. The following is a list of [Supported Authentication](#supported-authentication) methods  
</br>

## Supported Authentication
[Supported]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Supported Authentication"

* BASIC (insecure)
* HTTPS Only + BASIC&#x20;
* RADIUS
* Active Directory (SSO via OAUTH 2.0)
* Google Cloud (SSO via OAUTH 2.0)
* Ping Identity Cloud (SSO via OAUTH 2.0)  
</br>

## HTTPS Only
[HTTPS only]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md "HTTPS only"

By default, HTTP and HTTPS are enabled on the GUI. In any security setting, HTTP must be disabled,  </br> as it's an unsecured protocol. To disable HTTP, edit the config file:

### General Config

```c
/opt/fmadio/etc/time.lua
```

Find the "Security" section as follows

```c
["Security"] =
{
    ["HTTPAccess"] = "enable",
    ["Auth"] = "BASIC",
    ["ConfigAccess"] = "full",
    ["GUIMode"] = "full",
},

```

Edit the "HTTPAccess" section from "enable" to "false" as follows:

```c
["HTTPAccess"] = false,
```

Save the file&#x20;

### Restart Nginx

Restart nginx by running the following command:

```c
sudo killall nginx
```

NGINX will restart automatically within 60 seconds with the updated configuration. Only HTTPS  </br> access is now possible.
SSO configuration is more complicated, please contact support@fmad.io  </br> and we can walk you thru the setup personally  
</br>

## RADIUS
[RADIUS]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Radius"

**FW: 7563+**

We support RADIUS authentication using the freeradius client. Configuration is as follow

### **General Config**

Edit the configuration file using the command&#x20;

```c
/opt/fmadio/etc/time.lua
```

Find the "Security" section, as shown below:

```c
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

Edit the following configuration. This disables the HTTP protocol:&#x20;

```c
["HTTPAccess"] = false,
```

Edit the following, it sets RADIUS as the authentication method

```c
["Auth"] = "RADIUS",
```

Configure your RADIUS login information

```c
["RADIUS_Secret"] = "testing123",
["RADIUS_Host"] = "192.168.1.1",
["RADIUS_Protocol"] = "PPP",
```

Finally, there's a Timeout, the user is logged out and will be required to re-authenticate. The value is  </br> in nanoseconds, an example of converting 1 day to nanoseconds and setting it as the timeout value  </br>is shown below:

```c
    ["RADIUS_Timeout"] = 24*60*60*1e9,

```

### **Restart Nginx**

Restart nginx using the command below and it will re-spawn within 60sec automatically

```c
sudo killall nginx
```

### **Login**

You should see a login page to access FMADIO as shown in the screenshot below:  

</br>
  
<img src="../.gitbook/assets/image (80) (1).png" width="800px" height="400px">
</br>
### **TROUBLESHOOTING**

If there are some problems, please confirm on CLI using radclient, see example below.

```c
fmadio@fmadio100v2-228U:$ echo "User-Name = steve" | radclient <radius server ip>:1812 auth testing123
Sent Access-Request Id 95 from 0.0.0.0:56527 to 192.168.2.132:1812 length 27
Received Access-Reject Id 95 from 192.168.2.132:1812 to 192.168.2.175:56527 length 20
(0) -: Expected Access-Accept got Access-Reject
fmadio@fmadio100v2-228U:$

```

### Active Directory
[Active Directory]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Active Directory"

**FW:7608+**

FMADIO Capture devices can authenticate the users using Active Directory via the OAUTH 2.0  </br> protocol. This enables Single Sign On with ADFS.

### Public IP Testing

In the following example, we have used a reverse SSH tunnel to temporarily put the FMADIO system  </br> on a public IP, as Azure Active Directory services require internet-accessible devices for the redirect\_uri.&#x20;

> :memo: **Note:**  For an On-Premise Active Directory Server, this is not required.&#x20;

Example of Reverse SSH Tunnel:

```c
 ssh -R 8888:192.168.1.100:443 ec2-user@aws-instance.compute.amazonaws.com
```

NOTE: SSH tunnel should not use localhost, as all localhost-sourced requests bypass authentication.  </br>Instead, use the IP address of the management interface

### General Config

Start by editing the general FMADIO configuration file

```c
/opt/fmadio/etc/time.lua
```

Then setting HTTP (un-encrypted) access to "disable", and Auth method to "OAUTH", as shown in the  </br> example below. The other security fields can be left as they are.

```c
["Security"] =
{
    ["HTTPAccess"]      = "disable",
    ["Auth"]            = "OAUTH",
.
.
```

Save the file and ensure there are no parse errors by running fmadiolua /opt/fmadio/etc/time.lua

### OAUTH Config

Next, create a file name

```c
/opt/fmadio/etc/oauth_opts.lua
```

This file contains the ADFS OAUTH End points as follows

```c
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

These fields are from the ADFS Endpoint URI information. We created a fmadio sign-in entry, which  </br> has the client\_id entered above.
The "discovery" config in the above needs to be the OpenID   </br> Connect Metadata document, as seen in the screenshot below.  
</br>

<img src="../.gitbook/assets/image (118) (1) (1) (1).png" width="800px" height="400px">

the "client\_id" is shown below  
</br>

<img src="../.gitbook/assets/image (124) (1) (1) (1) (1) (1).png" width="800px" height="400px">

> :memo: **Note:** The "client\__secret" in the above config needs to be the Value shown below, not the secretID_  

</br>

<img src="../.gitbook/assets/image (126) (1) (1).png" width="800px" height="400px" alt="Alt text" style="box-shadow: 3px 3px 3px gray;">  

</br>

Finally the "redirect\_uri" needs to be registered as follows:  

</br>
<img src="../.gitbook/assets/image (119) (1) (1) (1).png" width="800px" height="400px">

Once config is complete, please confirm there are no syntax errors by running&#x20;

```c
fmadiolua /opt/fmadio/etc/oauth_opts.lua
```

The correct output is as shown in the screenshot below. If there are any syntax errors, please correct  </br> them.  

</br>

<img src="../.gitbook/assets/image (115) (1) (1).png" width="800px">

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process using the command below.  </br> It will re-spawn on a 1min cron job automatically

```c
sudo killall nginx
```

### Logging in

Next, point a browser to the FMADIO device. It should redirect you to the Active Directory login  </br> page as shown in the screenshot below.   

</br>

<img src="../.gitbook/assets/image (121) (1) (1).png" width="800px" height="400px">  
</br>

Login to the system using your Azure / Microsoft credentials. Then the FMADIO device dashboard  </br> will be as shown below   
</br>

<img src="../.gitbook/assets/image (127) (1) (1) (1).png" width="800px" height="400px">  
</br>

### Logout

Logout is the same, using the logout button shown below:  
</br>
 
<img src="../.gitbook/assets/image (90) (1) (1) (1).png" width="800px" height="400px">  
</br>

Then choose an account to sign out of 

</br>

<img src="../.gitbook/assets/image (120) (1) (1) (1).png" width="800px">

## Google Cloud
[Google]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Google Cloud"

**FW:7608+**

While less practical as its typically for publicly accessible sites, it can be used with a Google   </br> Cloud VPC to tunnel authentication requests from a private network to Google Cloud infrastructure.

In this example, we just reverse ssh tunnel an FMADIO system onto the public internet   </br> (strongly discouraged) for demonstration purposes only.&#x20;

### General Config

Start by editing the general FMADIO configuration file

```c
/opt/fmadio/etc/time.lua
```

Then set HTTP (un-encrypted) access to "disable", and Auth method to "OAUTH", as shown in the   </br> example below. The other security fields can be left as they are.

```c
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

```c
/opt/fmadio/etc/oauth_opts.lua
```

> :memo: **Note:**  This file contains the Google Cloud OAUTH End points as follows.&#x20;

```c
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

> :memo: **Note:**  The "clientid" and "client\_secret" need to be replaced with the generated   </br> authentication information from google as shown below. The above is a throw away example only

### Google Credentials

Next, generate Google OAUTH credentials as follows:  
</br>

<img src="../.gitbook/assets/image (90) (1) (1).png" width="800px">  
</br>
Then fill in the information, as follows. Google is a bit more strict and requires TLD endpoints  </br> not raw IPs  
</br>

<img src="../.gitbook/assets/image (80).png" width="800px">

Which results in the following secret information  

</br>
<img src="../.gitbook/assets/image (115) (1).png" width="800px">  
</br>

Update the oauth\_opts.lua file above with the information

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process with the command   </br> below. It will re-spawn on a 1min cron job automatically

```c
sudo killall nginx
```

### Logging In

Next, point the browser to the FMADIO device and it will redirect to Google Sign in account&#x20;  
</br>

<img src="../.gitbook/assets/image (118) (1) (1) (1).png" width="800px" height="400px">

Login using your Google account information, and it will re-direct you to the FMADIO dashboard.  

</br>

<img src="../.gitbook/assets/image (119) (1) (1) (1).png" width="800px" height="400px">  

</br>

> :memo: **Note:**  Any further questions please contact support@fmad.io for assistance.  

</br>

## Ping Identity
[Ping]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "Ping Identity"

**FW:7608+**

Ping Identity is a popular On-Premise authentication system typically used in large organizations.   </br> We support Single Sign On with their product suite. Below is an example of a configuration setup  </br> using the Cloud Services. This example uses a reverse SSH tunnel to put  </br> the FMADIO device on a publicly accessible IP (we strongly discourage this approach)  </br> for demonstration purposes only, to replicate setting up an On-Premise install.

### General Config

Start by editing the general FMADIO configuration file

```
/opt/fmadio/etc/time.lua
```

Then set HTTP (un-encrypted) access to "disable" and Auth method to "OAUTH" as shown below. The other  </br> security fields can be left as they are.

```c
["Security"] =
{
    ["HTTPAccess"]      = "disable",
    ["Auth"]            = "OAUTH",
.
.
```

Save the file and ensure there are no parse errors by running fmadiolua /opt/fmadio/etc/time.lua

### OAUTH Config

Next, create a file name

```c
/opt/fmadio/etc/oauth_opts.lua
```

This file contains the Ping Identity OAUTH End points as follows.&#x20;

```c
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

The "clientid" and "client\_secret" need to be replaced with the generated authentication information </br> from Ping Identity interface as shown below. The above is a throw away example only

### Ping Identity Credentials

We setup a web application using Ping Identity interface as follows. The key fields are shown in red.  

</br>
 
<img src="../.gitbook/assets/image (117) (1).png" width="800px" height="400px"> 

</br> 
<img src="../.gitbook/assets/image (118) (1) (1).png" width="800px" height="400px"> 

</br>

These fields are mapped directly into the oauth\_opts.lua configuration file above.

### Restart nginx

Restart nginx to load in the new configuration file, by killing the process using the command below.  </br> It will re-spawn on a 1min cron job automatically.

```c
sudo killall nginx
```

### Logging In

Next, point the browser to the FMADIO device and it will redirect to Ping Identity SSO account  </br> as follows  
</br>
  
<img src="../.gitbook/assets/image (119) (1) (1) (1).png" width="800px" height="400px">


After a successful authentication, the FMADIO dashboard is as shown below:  
</br>

<img src="../.gitbook/assets/image (129) (1) (1).png" width="800px"> 

&#x20;Any further questions or problems, please contact us support@fmad.io

## PAM LDAP

**FW: 8529+**

FMADIO systems support Linux PAM ( [https://github.com/linux-pam/linux-pam](https://github.com/linux-pam/linux-pam) ) as an authetication   </br> method. One option for centralized authentication is to use LDAP via PAM.

1\) First run fmadiocli settings to set the authentication method

[https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-auth](https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-auth)

```c
config security auth pam-ldap
```

2\) We also strongly recommend to disable HTTP access as all username / passwords are sent over  </br> un-encrypted HTTP

[https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-http](https://docs.fmad.io/fmadio-documentation/cli-reference/fmadiocli#config-security-http)

```c
config security http false
```

3\) Configure LDAP client nslcd. Copy the default config file as follows:

```c
cp /opt/fmadio/etc_ro/nslcd.conf /opt/fmadio/etc/nslcd.conf
```

The default config looks like the following

```c
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

```c
ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
```

Successful authentication looks like the following

```c
fmadio@fmadio100v2-228U:~$ ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
dn:cn=fmadio-user,dc=fmad,dc=io
```

Once this is working, both SSH, WWW-Admin and WWW-User LDAP posix group members can  </br> login to the system.

The LDAP posixGroups are:  </br>

```c
fmadio-ssh-admin  - for SSH access

fmadio-www-admin - for WWW admin access (can change anything)

fmadio-www-user - for WWW user access (monitoring and pcap downloading)
```

6\) Both SSH and WWW now fully configured using LDAP as centralized authentication

## LDAP Optional
[LDAP]: https://github.com/fmadio/documentation/edit/fmadio100v2/configuration/authentication.md  "LDAP Optional"

Some environments require a notice when logging in, such as the following  
</br>
<img src="../.gitbook/assets/image (1) (2).png" width="800px">  
</br>

This can be customized as follows:

1\) copy the default template&#x20;

```c
cp /opt/fmadio/www/login/authorized_access.html.default /opt/fmadio/etc/authorized_access.html
```

2\) Edit the content of

```c
/opt/fmadio/etc/authorized_access.html
```

3\) restart nginx

kill nginx and wait 60sec for it to restart

```c
sudo killall nginx
```

### Troubleshooting

Configuration usually does not go as planned, as such here are some > :memo: **Note:**tips to try

1\) run nslcd in the foreground

```c
sudo killall nslcd
sudo /usr/local/sbin/nslcd -f
```

This will check the /etc/nslcd.conf configuration file is working correctly, either config type  </br> or LDAP server problems.

Once its running ensure local lookups work correctly as follows

```c
ldapwhoami -x  -D cn=fmadio-user,dc=fmad,dc=io  -H ldap://192.168.1.100/ -w "password"
```

2\) check nginx config files

The nginx logfiles are located in

```c
tail -F -n 100 /mnt/store0/log/nginx_error.log
```

Any errors there might help understand the issues

3\) check syslog file for PAM logs

```c
tail -F -n 100 /mnt/store0/log/messages |grep -i pam
```

This will print out logs of all PAM messages and may help debugging
