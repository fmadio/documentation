# External Web Application

**FW: 7738+**

FMADIO Packet capture systems have the ability to push specific PCAPs to a external 3rd Party application as follows

![FMADIO External HTTP POST Packet Capture PCAP](../.gitbook/assets/2022-03-12\_23-37.png)

This workflow enables a simple approach, using a URI to push a PCAP over HTTP using a POST request to a remote end application, then following a JSON redirect.

In this example, we are using our internally developed FMADIO Packet Scope and FMADIO Shark (FShark) as a reference example. This is for demonstration purposes only, any 3rd party web application will work.

## Request URI

The workflow process is as follows,

1. Web Application A generates a en.loader.html URI. For example Epoch Start/End and an BPF Filter
2. Web Application A directs the Client to this URI
3. FMADIO Packet Capture System  presents the Client with the FMADIO Loader web page. This shows the progress of filtering and upload to Application B and any potential errors.
4. Application B completes upload, and returns a redirect URI
5. FMADIO Loader web page follows the redirect, allowing the client to load seamlessly into Web Application B with the new PCAP data.

The example is using FMADIO developed software for this, however there is no limitation, any 3rd party application will work.

![Web application Workflow](../.gitbook/assets/2022-03-12\_22-29.png)

## Example (FMADIO Shark)

In the following example we are using FMADIO Packet Scope as as (Application A) and FMADIO Shark as (Application B).

### Web Application A

In this example, Web Application A is "FMADIO Packet Scope" with Web Application B "FMADIO Shark". PacketScope generates the loader link request as follows

![FMADIO Shark URL](../.gitbook/assets/2022-03-12\_22-40.png)

The above example has an Epoch Start time and Epoch End time as well as a BPF Filter applied, the end result is the following URI

```
http://192.168.1.100/en.loader.html?
                Path=/api/v1/pcap/splittime&
                Target=fshark&
                StreamName=coffee_20220313_1227&
                TSBegin=1647149672563087104ULL&
                TSEnd=1647150048271070976ULL&
                FilterBPF=udp%20and%20port%20%2053&&
```

This redirects to the FMADIO Loader page which processes and pushes the above Filter specification to the Target "fshark"

Expanding on the details

```
Path=/api/v1/pcap/splittime&
```

**Path** indicates how the PCAP should be generated, in this case its a specific capture name with filters.

```
Target=fshark&
```

**Target**, informs what the End Point target is. "fshark" is an internally defined EP. Due to security reasons End Point definitions can only be configured on the FMADIO System. Only the enumerated name of the End Point is used in the URI.

```
StreamName=coffee_20220313_1227&
```

**StreamName** specifies the name of the Capture to process.

```
TSBegin=1647149672563087104ULL&
```

**TSBegin** is the Epoch in Nanoseconds for the start of the Filter

```
TSEnd=1647150048271070976ULL&
```

**TSEnd** is the Epoch in Nanoseconds for the end of the Filter

```
FilterBPF=udp%20and%20port%20%2053&&
```

**FilterBPF** is the Escape Encoded BPF filter, in this case "udp and port 53" e.g. extract DNS traffic.

For full details please check the API v1 Documentation page

{% embed url="https://docs.fmad.io/fmadio-documentation/api/summary#downloading-pcap-1" %}
API Documentation
{% endembed %}

## Loader Page

Once clicked the following Loader page is visible.

![Loader Page](<../.gitbook/assets/image (119) (1) (1).png>)

In addition we added the following URI to prevent automatic reload, this can be helpful for debugging purposes.

```
NoRedirect=true&
```

Internally the FMADIO Device is issuing the following HTTP POST command thru CURL on the filtered PCAP. This URL generator is configuration on the FMADIO Device, almost anything is possible. This example the "fshark" Target is built into the system firmware.

```
cat fmadio.pcap |  /usr/local/bin/curl -s  -X POST -F  "data=@-" "http://192.168.1.101/api/v1/fshark/upload?filename=fmadio20p3_1647148506884765952.pcap&meta=eyJGaWx0ZXJ="

```

Once completed the above HTTP POST request into Web Application B is completed, it returns a redirect as follows to FMADIO Packet Capture System

```
{"redirect":"\/fshark\/en.pcapview.html?filename=fmadio20p3_1647154945535058944.pcap","status":true}

```

This redirect URL is forwarded to the Web Client is shown below

![Completed Redirect](<../.gitbook/assets/image (127).png>)

The 3rd Party Application returns the redirect as the response to the POST upload request. This is where the redirect URI is specific, or any error condition

![Application B Redirect JSON URI](<../.gitbook/assets/image (128).png>)

## Redirect Page

After the HTTP Post has been completed the 3rd Party Application B returns a redirect page. The FMADIO Loader then redirects to this location.

In this example it loads FMADIO Shark with the URI

```
/fshark/en.pcapview.html?filename=fmadio20p3_1647155157922904064.pcap
```

The following page is what the 3rd Party Application displays.&#x20;

NOTE: Due to FMADIO Shark being internally developed the Web page does look like FMADIO products. This page could show anything, there's no iframes etc.

![3rd Party Application B](<../.gitbook/assets/image (126) (1).png>)

## Recommended

The recommended URI is to use the /api/v1/pcap/timerange URI endpoint. As this does not require any stream names, just Epoch start/end times and a BPF filter.

Example as follows, please remember to Escape Encode the FilterBPF string.

```
http://192.168.1.100/en.loader.html?
                Path=/api/v1/pcap/timerange&
                Target=fshark&
                TSBegin=1647149672563087104ULL&
                TSEnd=1647150048271070976ULL&
                FilterBPF=udp%20and%20port%20%2053&&
```
