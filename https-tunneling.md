# HTTPS Tunneling

**FW: 7297+**

**PCAP2JSON v2: Ver:606+**

By default pcap2json does not support HTTPS / TLS connections as it severely reduces the ES push performance. As a work around you can use NGINX as a proxy to receive HTTP and send HTTPS to the destination end point, As shown below

![PCAP2JSON HTTPS/TLS Tunneling](.gitbook/assets/image%20%2894%29.png)

In this example we are configuring FMADIO100G system running pcap2json v2 pushing over TLS to a cloud ElasticStack instance hosted by elastic.io

## 1\) Configure TLS Proxy

Configuring pcap2json for this is fairly simple start by configuring the nginx proxy as follows in the file

```text
/opt/fmadio/etc/time.lua
```

If the WWW section has not been created, please add as shown below. Replace the Endpoint + API Key with your own

```text
["WWW"] =
{
        ["ES_EndPoint"]     = "fmadio-colo.cloud.es.io",
        ["ES_APIKey"]       = "*****secrect API key****",
}

```

Please refer to [https://www.elastic.co/guide/en/kibana/master/api-keys.html](https://www.elastic.co/guide/en/kibana/master/api-keys.html) for documentation on how to generate Elastic Search API Keys

## 2\) Restart Nginx

For the above configuration to take effect, need to reload nginx by doing the following

```text
sudo kilalll nginx

```

After killing the process it may take up to 1 minute for nginx to respawn and be up and running

## 3\) Check the TLS Proxy 

Next check the TLS proxy is working correctly by querying the ES Node for a valid response. 

```text
curl http://localhost/elasticsearch/tls/
```

Note we are using http to get the cluster status, the URI prefix uses the nginx running on the FMADIO system to proxy and output HTTPS connection to our ES EndPoint usually on a WAN or internet facing IP.

A correct example output looks like the following

```javascript
fmadio@fmadio100v2-228U:~$ curl http://localhost/elasticsearch/tls/
{
  "name" : "instance-0000000000",
  "cluster_name" : "b8d4757cbebd4562a334c85d0ebce474",
  "cluster_uuid" : "fH2qoLTfSViX0RWlbsKdiQ",
  "version" : {
    "number" : "7.13.4",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "c5f60e894ca0c61cdbae4f5a686d9f08bcefc942",
    "build_date" : "2021-07-14T18:33:36.673943207Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.2",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
fmadio@fmadio100v2-228U:~$

```

If this is not the correct result, check the nginx logfile per below for trouble shooting 

```javascript
/mnt/store0/log/nginx_error.log
```

## 4\) Configure pcap2json

Configuring pcap2json is quite simple, because HTTPS/TLS push is less performant, we need to restrict the total number of con-current ES push operations, as follows

```javascript
    "--output-buffercnt 4",
```

Next change the --es-host target as follows

```javascript
    "--es-host 127.0.0.1:80:1e6",
```

In addition we need to prefix all URI requests so FMADIO nginx will route it correctly

```javascript
    "--es-uriprefix /elasticsearch/tls/",
```

Save it and pcap2json will run as normal but push to your remote ES cluster using HTTPS/TLS. 

Any issues check the logfile below for problems + the same nginx error logfile as above

```javascript
/mnt/store0/log/pcap2json_backend.cur
```

Following is a full backend example configuration file for reference.

```javascript
,
["backend"] =
{
    "--index-name pcap2json_test7",
    "--output-buffercnt 4",
    "--output-espush",

    "--es-bulk-action index",
    "--es-host 127.0.0.1:80:1e6",
    "--es-uriprefix /elasticsearch/tls/",
    "--geoip /mnt/store0/etc/pcap2json_map.geo",
},

```



