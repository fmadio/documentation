# Geo Map Generation

Generating a MAP requires the following. e.g. from Maxmind GeoIP Lite databas + custom hosts file

In this case we have the Maxmind City Geo database

```text
--maxmind-city /mnt/store1/cache/json/GeoLite2-City-CSV_20200324/ 
```

Then the MaxMind ASN database

```text
--maxmind-asn /mnt/store1/cache/json/GeoLite2-ASN-CSV_20200324/ 
```

Then a custom Hosts file

```text
--hosts /mnt/store0/git/json_wrangle/app/pcap2json_map/hosts.csv
```

All resulting in a final command line as follows

```text
$ ./pcap2json_map  --maxmind-city /mnt/store1/cache/json/GeoLite2-City-CSV_20200324/ --maxmind-asn /mnt/store1/cache/json/GeoLite2-ASN-CSV_20200324/ --hosts /mnt/store0/git/json_wrangle/app/pcap2json_map/hosts.csv
```

The output file is shown below

```text
fmadio@fmadio40v2-194:/opt/fmadio/bin$ ls -alh geoip_fmadio.geo
-rw-r--r--    1 fmadio   staff     421.1M May 17 22:16 geoip_fmadio.geo
fmadio@fmadio40v2-194:/opt/fmadio/bin$
```

Typically this is 400MB-500MB in size depending on the input dataset. Enabling it in pcap2json.lua configuration file requires

1\) copying geo\_fmadio.geo to /mnt/store0/etc/pcap2json\_map.geo 

2\) enabling it in pcap2json.lua configuration file as follows

