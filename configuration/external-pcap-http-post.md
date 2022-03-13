# External PCAP HTTP POST

**FW: 7738+**

FMADIO Packet capture systems have the ability to push specific PCAPs to a external 3rd Party application as follows

![FMADIO 3rd Part HTTP POST Integration](../.gitbook/assets/2022-03-12\_23-23.png)

This workflow enables a simple way using a URI to push a PCAP over HTTP using a POST request to a remote end application.

In this example, we are using our internally developed FMADIO Shark (FShark) as a reference example. This 3rd party application runs NGINX to receive and process the HTTP POST PCAP data.

## Request URI

The workflow process is as follows, we are using FMADIO Developed software for this, however there is no limitation, any 3rd party application will work. For example using Grafana Pushing (Application A) and FMADIO Shark (Application B).&#x20;

![Web application Workflow](../.gitbook/assets/2022-03-12\_22-29.png)

## Example&#x20;

In the following example we are using FMADIO Packet Scope as as (Application A) and FMADIO Shark as (Application B).

### Application A

Application A in this case is FMADIO Packet Scope with an FMADIO Shark web link request as follows

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

Path indicates how the PCAP should be generated, in this case its a specific capture name with filters.

```
Target=fshark&
```

Target, informs what the End Point target is. "fshark" is an internally defined EP. Due to security reasons End Point definitions can only be configured on the FMADIO System. Only the enumerated name of the End Point is used in the URI.

```
StreamName=coffee_20220313_1227&
```

StreamName specifies the name of the Capture to process.

```
TSBegin=1647149672563087104ULL&
```

TSBegin is the Epoch in Nanoseconds for the start of the Filter

```
TSEnd=1647150048271070976ULL&
```

TSEnd is the Epoch in Nanoseconds for the end of the Filter

```
FilterBPF=udp%20and%20port%20%2053&&
```

FilterBPF is the Escape Encoded BPF filter, in this case "udp and port 53" e.g. extract DNS traffic.

For full details please check the API v1 Documentation page

{% embed url="https://docs.fmad.io/fmadio-documentation/api/summary#downloading-pcap-1" %}
API Documentation
{% endembed %}

## Loader Page

Once clicked the following Loader page is visible.

![Loader Page](<../.gitbook/assets/image (119).png>)

In addition we added the following URI to prevent automatic reload, this can be helpful for debugging purposes.

```
NoRedirect=true&
```

Once completed the redirect URL is shown as follows

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

NOTE: Due to FMADIO Shark being internally developed the Web page does look like FMADIO products. This page could show anything, theres no iframes etc.

![3rd Party Application B](<../.gitbook/assets/image (126).png>)

## Example

The recomended URI is to use the /api/v1/pcap/timerange URI endpoint. As this does not require any stream names, just Epoch start/end times and a BPF filter.

Example as follows, please remember to Escape Encode the FilterBPF string.

```
http://192.168.1.100/en.loader.html?
                Path=/api/v1/pcap/timerange&
                Target=fshark&
                TSBegin=1647149672563087104ULL&
                TSEnd=1647150048271070976ULL&
                FilterBPF=udp%20and%20port%20%2053&&
```
