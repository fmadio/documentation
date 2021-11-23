# WWW Customization

We use nginx as our WWW front end interface, enabling all the standard features and stability this provides.

### Custom TLS Certificates

Custom TLS/SSL WWW x509 certificates are supported. Steps as follows

**STEP 1)**

Copy  \<you cert>.pem to  /opt/fmadio/etc/fmadio\_cert.pem

Copy  \<you cert>.key to  /opt/fmadio/etc/fmadio\_cert.key

**STEP 2)**

Stop the current nginx process

```
sudo killall nginx
```

**STEP 3)**

Wait for nginx to re-spawn (takes \~ 60 sec) and your certs should be used for all HTTPS/TLS communication.&#x20;

Any problems check the log files in /mnt/store0/log/nginx\_error.log&#x20;
