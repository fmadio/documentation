# Password Change

## WEBGUI

FMADIO Packet Capture Systems use the default login and password when the system is shipped. Additional WebGUI users can be added manually using the htpasswd utility. To set a new password "password" for the fmadio account use the following command line:

```text
fmadio@fmadio20v2-149:$ sudo htpasswd /opt/fmadio/etc/htpasswd fmadio password

```

By default this utility overwrites the existing user account, so only 1 user account is possible. However additional users are added by appending to the /opt/fmadio/etc/htpasswd file. The following shows creating a user account "test" with the password "newpassword".

```text
fmadio@fmadio20v2-149:$ sudo htpasswd /tmp/ptmp test newpassword 
fmadio@fmadio20v2-149:$ cat /tmp/ptmp >> /opt/fmadio/etc/htpasswd
```

Please be careful duplicate usernames are not in the /opt/fmadio/etc/htpasswd file. Use a text editor to adjust the file if needed.

The new users and passwords can now access the GUI. In addition for logging the nginx access logs will show the username for all URL requests.

## SSH

Unfortunately adding additional SSH usernames is not possible, as the permissions may be incorrectly set causing undefined system behavior. However multiple people can login to the system using different SSH keys via the .authorized\_keys config file.

The authorized ssh keys file is located in

```text
/opt/fmadio/etc/fmadio_authorized_keys

```

Please note, the authorized\_keys file in the users .ssh account directory does not persist across reboots. Keys must be added to the above location.

### Change default SSH password

Changing the default SSH password uses the  standard linux utilit "passwd". 

```text
sudo passwd fmadio
```

Example below

```text
fmadio@fmadio20v3-287:/etc$ sudo passwd fmadio
Changing password for fmadio
Enter the new password (minimum of 5, maximum of 8 characters)
Please use a combination of upper and lower case letters and numbers.
New password:
Re-enter new password:
passwd: password changed.
fmadio@fmadio20v3-287:/etc$

```



