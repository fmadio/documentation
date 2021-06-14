# Geo Map Generation

Generating a MAP requires the following. e.g. from Maxmind GeoIP Lite database + custom hosts file

In this case we have the Maxmind City Geo database

```text
--maxmind-city-block /mnt/store1/cache/json/GeoLite2-City-Blocks-IPv4.csv  --maxmind-city-location /mnt/store1/cache/json/GeoLite2-City-Locations-en.csv
```

Then the MaxMind ASN database

```text
--maxmind-asn /mnt/store1/cache/json/GeoLite2-ASN-CSV_20200324/GeoLite2-ASN-Blocks-IPv4.csv 
```

Then the MaxMind ISP database

```text
--maxmind-isp /mnt/store1/cache/json/GeoIP-124_20200714/GeoIPISP.csv
```

Then a custom Hosts file

```text
--hosts /mnt/store0/git/json_wrangle/app/pcap2json_map/hosts.csv
```

Then a scratch-file \(ensure that path to file is a [mounted scratch disk](https://docs.fmad.io/fmadio-documentation/v/fmadio20v3/configuration/scratch-disk-ext4)\)

```text
--scratch-file /mnt/store1/scratch.map
```

All resulting in a final command line as follows

```text
$ ./pcap2json_map  --scratch-file /mnt/store1/scratch.map --maxmind-city-block /mnt/store1/cache/json/GeoLite2-City-Blocks-IPv4.csv --maxmind-city-location /mnt/store1/cache/json/GeoLite2-City-Locations-en.csv --maxmind-asn /mnt/store1/cache/json/GeoLite2-ASN-CSV_20200324/GeoLite2-ASN-Blocks-IPv4.csv --maxmind-isp /mnt/store1/cache/json/GeoIP-124_20200714/GeoIPISP.csv --hosts /mnt/store0/git/json_wrangle/app/pcap2json_map/hosts.csv --generate
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

