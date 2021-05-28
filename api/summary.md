---
description: A summary for developers to access a FMADIO device using the API.
---

# Summary

### FMADIO API

The FMADIO API is simple and designed for easy scripting integration.

Note: Replace the IP 1.1.1.1 with the host IP of your FMADIO device.

### Device Operation

{% api-method method="get" host=" http://1.1.1.1/sysmaster/capture\_start?StreamName=<capture name>" path="" %}
{% api-method-summary %}
Capture Start
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="StreamName" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "Status": true,
  "Str": "[Fri May 28 09:48:34 2021] successfully started capture [test1234]"
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" http://1.1.1.1/sysmaster/capture\_stop" path="" %}
{% api-method-summary %}
Capture Stop
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "Status":true,
    "Str":"[Mon Jul  2 11:26:13 2018] successfully stopped capture [TestCapture]"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="http://1.1.1.1/sysmaster/status" path="" %}
{% api-method-summary %}
Capture Status
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Downloading PCAP

{% api-method method="get" host="http://1.1.1.1/stream/list" path="" %}
{% api-method-summary %}
List All Captures
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="http://1.1.1.1/stream/ssize?StreamName=<capture sname>&StreamView=<split mode>" path="" %}
{% api-method-summary %}
Split Capture by filesize
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="StreamView" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="StreamName" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="http://1.1.1.1/stream/stime?StreamName=<capture sname>&StreamView=<split mode>" path="" %}
{% api-method-summary %}
Split Capture by time
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="TSEnd" type="integer" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="TSBegin" type="integer" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="StreamName" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="http://1.1.1.1/pcap/single?StreamName=<capture name>" path="" %}
{% api-method-summary %}
Single PCAP Download
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="FIlterBPF" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="Compression" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="StreamName" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="http://1.1.1.1/pcap/splittime?StreamName=<string>&Start=<int>&Stop=<int>&FilterBPF=<string>&FilterRE=<string>&FilterPort=<int>" path="" %}
{% api-method-summary %}
Split PCAP Time Download
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="FilterPort" type="integer" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="FilterRE" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="FilterBPF" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="StreamName" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="Stop" type="integer" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="Start" type="integer" required=true %}
Start time in nanoseconds epoch
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
PCAP Data stream. Usually used with tools like curl.
{% endapi-method-response-example-description %}

```
$ curl -u fmadio:100g "http://192.168.2.75/pcap/splittime?StreamName=TestCapture_20180702_1127&&Start=1530498788000000000&Stop=1530498789000000000&" | tcpdump  -r - -nn | head
11:33:08.000000 66:77:88:99:aa:bb > 00:44:44:44:44:44 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 153a 2d03 153a 2e03 153a 2f03  ..,..:-..:...:/.
        0x0010:  153a 3003 153a 3103 153a 3203 153a 3303  .:0..:1..:2..:3.
        0x0020:  153a 3403 153a 3503 153a 3603 153a 3703  .:4..:5..:6..:7.
        0x0030:  153a a878 4e26                           .:.xN&
11:33:08.000000 66:77:88:99:aa:bb > 00:33:33:33:33:33 Null Information, send seq 22, rcv seq 1, Flags [Poll], length 54
        0x0000:  0000 2c03 152a 2d03 152a 2e03 152a 2f03  ..,..*-..*...*/.
        0x0010:  152a 3003 152a 3103 152a 3203 152a 3303  .*0..*1..*2..*3.
        0x0020:  152a 3403 152a 3503 152a 3603 152a 3703  .*4..*5..*6..*7.
        0x0030:  152a 7b57 491d                           .*{WI.
.
.
.
.
.
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Device Management

{% api-method method="get" host="http://1.1.1.1/sysmaster/stats\_summary" path="" %}
{% api-method-summary %}
System Status
{% endapi-method-summary %}

{% api-method-description %}
Get system status information
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



### V1 API

The FMADIO V1 API uses endpoints with parameters. All V1 versions of the API endpoints shall have the format:

`/api/v1/<operation>/<task>?<params=x>`

The V1 API has advantages of the previous API. These are:

* Multiple downloads can occur at the same time on the same device \(up to 4 concurrently\).
* Improved download performance
* Ability to use compressed and bpf filter  parameters on most downloads.

Note: All original API url's shall be available as well as the new V1 endpoints.

| Description | Url |
| :--- | :--- |
| **DOWNLOADING PCAP**  |  |
| Download full capture as single PCAP | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>` |
| Download capture as single PCAP with gz compression | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>?Compression=fast` |
| Download a time range of pcaps | `http://1.1.1.1/api/v1/pcap/timerange?TSBegin=<nano second epoch start time>&TSEnd=<nano second epoch stop time>&TSMax=<nano second size>&TSMode=<nanos or msecs>` |
| Download capture within a specific time | `http://1.1.1.1/api/v1/pcap/splittime?StreamName=<capture name>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |
| Download capture with BPF Filter | `http://1.1.1.1/api/v1/pcap/single?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>` |
| Download capture with BPF Filter and time range | `http://1.1.1.1/api/v1/pcap/splittime?StreamName=<capture name>&FilterBPF=<escape encoded BPF filter>&Start=<nano second epoch start time>&Stop=<nano second epoch stop time>` |

