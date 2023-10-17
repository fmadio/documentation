# Hardware

FMADIO 100G Hardware looks as follows

![FMADIO100G Gen2 1U Packet Capture System](<.gitbook/assets/image (22) (1).png>)

![FMADIO 100G Gen2 1U Packet System Side](<.gitbook/assets/image (23) (1).png>)

## Spec Sheet

{% file src=".gitbook/assets/100g-gen2-datasheet-v5.pdf" %}
FMADIO100G Gen2 Spec Sheet
{% endfile %}



![FMADIO 100G Gen2 Specsheet](<.gitbook/assets/image (12) (1).png>)



## Transceiver Support

List of known and tested transcivers that work with the system. Other transciver types should be ok, please check with support@fmad.io for clarification.

FMADIO Packet Capture systems are **Transceiver Vendor neutral,** 3rd party optics are fully supported.

| Module     | Speed   | Connector | Cable Type     | Brand                                      |
| ---------- | ------- | --------- | -------------- | ------------------------------------------ |
| 100G SR4   | 100Gbps | MPO12     | OM4 MMF        | Generic                                    |
| 100G LR4   | 100Gbps | LC        | OS2 SMF        | <p>Generic </p><p>Finisar FTLC1154RDPL</p> |
| 100G CWDM4 | 100Gbps | LC        | OS2 SMF        | <p>Generic </p><p>Finisar CTLC1157RGPL</p> |
| 100G SWDM4 | 100Gbps | LC        | OM4 MMF        | <p>Generic </p><p>Finisar FTLC9152RGPL</p> |
| 100G CR4   | 100Gbps | DAC       | Passive Twinax | Generic                                    |
| 40G SR4    | 40Gbps  | MPO12     | OM4 MMF        | <p>Generic </p><p>Finisar FTL410QE4C</p>   |
| 40G LR4    | 40Gbps  | LC        | OS2 SMF        | Generic                                    |
| 40G CR4    | 40Gbps  | DAC       | Passive Twinax | Generic                                    |

## Rails installation

Installation of the rails is tool less please see the following instructions

![](<.gitbook/assets/image (41).png>)

## Power Supply

System is installed with dual 1200W power supply. By default the system is configured as follows

| Total Power Consumption | Behavior                                                                           | LED                                                       |
| ----------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------- |
| < 400W                  | <p>HOT-STANDBY</p><p>Only Primary PSU is active, Secondary PSU in standby mode</p> | <p>Primary, Solid Green </p><p>Secondary, Green Blink</p> |
|   > 400W                | <p>HOT-HOT</p><p>Both Primary and Secondary PSU are in active mode</p>             | Both Solid Green                                          |

![FMADIO 100G PSU Configuration](<.gitbook/assets/image (57).png>)

The above behavior can be overridden to always be in HOT-HOT load balancing mode, with a configuration setting. Please contact support@fmad.io for more information

## Power Consumption

### FMADIO 100G 1U Analytics

![](<.gitbook/assets/image (81).png>)

![](<.gitbook/assets/image (76).png>)

## Specification

### Physical Dimensions:

* 438 x 43.5 x 730mm
* 17.2" x 1.7" x 28.7"
* (Designed for EIA 19” rack)

### Weight:

* 26KG / 57 lbs

### Color:

* Silver metallic

### Power Supply:

* 2 x Redundant Hot swappable 1200W&#x20;
* 110-240AC, 12-7A, 50-60Hz

### Fan:

* 8 x 40x40x56mm (23,000rpm) Chassis
* 2 x 1 x 40x40x56mm (Power Supply)

### Airflow:

* Front to back

### Operating Conditions:

* Operating temperature: 10°C to 25°C
* Operating humidity: 8-80% (non-condensing)

### Storage:

* 10 x U.2 NVMe Front panel drives

### Network:

* 1 x IPMI BMC out of band management (RJ45)
* 2 x RJ45 Management ports
* 2 x SFP+(or QSFP+) High speed management ports
* 2 x QSFP28 Capture ports
* 1 x Serial port (RJ45 connector)

### Platform:

* Dual socket Intel Xeon Platform
* DDR4 RDIMM RAM
