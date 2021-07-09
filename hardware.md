# Hardware

FMADIO 100G Hardware looks as follows

![FMADIO100G Gen2 1U Packet Capture System](.gitbook/assets/image%20%2822%29.png)

![FMADIO 100G Gen2 1U Packet System Side](.gitbook/assets/image%20%2823%29.png)

## Spec Sheet

{% file src=".gitbook/assets/100g-gen2-datasheet-v5.pdf" caption="FMADIO100G Gen2 Spec Sheet" %}



![FMADIO 100G Gen2 Specsheet](.gitbook/assets/image%20%2812%29.png)



## Transceiver Support

List of known and tested transcivers that work with the system. Other transciver types should be ok, please check with support@fmad.io for clarification.

FMADIO Packet Capture systems are **Transceiver Vendor neutral,** 3rd party optics are fully supported.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Module</th>
      <th style="text-align:left">Speed</th>
      <th style="text-align:left">Connector</th>
      <th style="text-align:left">Cable Type</th>
      <th style="text-align:left">Brand</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">100G SR4</td>
      <td style="text-align:left">100Gbps</td>
      <td style="text-align:left">MPO12</td>
      <td style="text-align:left">OM4 MMF</td>
      <td style="text-align:left">Generic</td>
    </tr>
    <tr>
      <td style="text-align:left">100G LR4</td>
      <td style="text-align:left">100Gbps</td>
      <td style="text-align:left">LC</td>
      <td style="text-align:left">OS2 SMF</td>
      <td style="text-align:left">
        <p>Generic</p>
        <p>Finisar FTLC1154RDPL</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">100G CWDM4</td>
      <td style="text-align:left">100Gbps</td>
      <td style="text-align:left">LC</td>
      <td style="text-align:left">OS2 SMF</td>
      <td style="text-align:left">
        <p>Generic</p>
        <p>Finisar CTLC1157RGPL</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">100G SWDM4</td>
      <td style="text-align:left">100Gbps</td>
      <td style="text-align:left">LC</td>
      <td style="text-align:left">OM4 MMF</td>
      <td style="text-align:left">
        <p>Generic</p>
        <p>Finisar FTLC9152RGPL</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">100G CR4</td>
      <td style="text-align:left">100Gbps</td>
      <td style="text-align:left">DAC</td>
      <td style="text-align:left">Passive Twinax</td>
      <td style="text-align:left">Generic</td>
    </tr>
    <tr>
      <td style="text-align:left">40G SR4</td>
      <td style="text-align:left">40Gbps</td>
      <td style="text-align:left">MPO12</td>
      <td style="text-align:left">OM4 MMF</td>
      <td style="text-align:left">
        <p>Generic</p>
        <p>Finisar FTL410QE4C</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">40G LR4</td>
      <td style="text-align:left">40Gbps</td>
      <td style="text-align:left">LC</td>
      <td style="text-align:left">OS2 SMF</td>
      <td style="text-align:left">Generic</td>
    </tr>
    <tr>
      <td style="text-align:left">40G CR4</td>
      <td style="text-align:left">40Gbps</td>
      <td style="text-align:left">DAC</td>
      <td style="text-align:left">Passive Twinax</td>
      <td style="text-align:left">Generic</td>
    </tr>
  </tbody>
</table>

## Rails installation

Installation of the rails is tool less please see the following instructions

![](.gitbook/assets/image%20%2841%29.png)

## Power Supply

System is installed with dual 1200W power supply. By default the system is configured as follows

<table>
  <thead>
    <tr>
      <th style="text-align:left">Total Power Consumption</th>
      <th style="text-align:left">Behavior</th>
      <th style="text-align:left">LED</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&lt; 400W</td>
      <td style="text-align:left">
        <p>HOT-STANDBY</p>
        <p>Only Primary PSU is active, Secondary PSU in standby mode</p>
      </td>
      <td style="text-align:left">
        <p>Primary, Solid Green</p>
        <p>Secondary, Green Blink</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&gt; 400W</td>
      <td style="text-align:left">
        <p>HOT-HOT</p>
        <p>Both Primary and Secondary PSU are in active mode</p>
      </td>
      <td style="text-align:left">Both Solid Green</td>
    </tr>
  </tbody>
</table>

![FMADIO 100G PSU Configuration](.gitbook/assets/image%20%2857%29.png)

The above behavior can be overridden to always be in HOT-HOT load balancing mode, with a configuration setting. Please contact support@fmad.io for more information

## Power Consumption

### FMADIO 100G 1U Analytics

![](.gitbook/assets/image%20%2877%29.png)

![](.gitbook/assets/image%20%2874%29.png)

