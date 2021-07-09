# HDD Writeback

**FW: 7219+**

Configuration options are in the specified config file. Please note all options requires capture to be stopped and started before settings are applied.

```text
/opt/fmadio/etc/time.lua
```

Specifically the Writeback block, example as follows

```text
["Writeback"] =
{
        ["IOPriority"]       = 11,
        ["EnableCompress"]   = true,
        ["CheckBlockCRC"]    = true,
        ["EnableECC"]        = true,
},
```

### IOPriority

This setting changes the default writeback IO Priority, allows changing preference for faster Downloads\(default\) or faster sustained Writeback to magnetic storage.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">11</td>
      <td style="text-align:left">
        <p>Writeback to Magnetic storage is lowest priority</p>
        <p>(Default Value)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">30</td>
      <td style="text-align:left">Writeback to disk has higher priority than Download or Push speed</td>
    </tr>
  </tbody>
</table>

### EnableCompress

Setting enables or disables disk compression. For faster sustained writeback to disk speeds, disable compression. 

For 1U systems disabling compression makes little difference due to lower of HDD write bandwidth. 

For 2U systems disabling compression improves sustained writeback to HDD performance. As the system has 12 spinning disks with an aggregate 10Gbps-20Gbps \(depending on spindle position\) write throughput.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">true</td>
      <td style="text-align:left">
        <p>Enable Compression</p>
        <p>(Default setting)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">false</td>
      <td style="text-align:left">Disable disk compression. Faster sustained writeback performance on 2U
        systems</td>
    </tr>
  </tbody>
</table>

### CheckBlockCRC

This function checks the CRC when reading data from the SSDs. It calculates the CRC and checks for a match against the orignial captured CRC value, before writing the block to magnetic storage. This adds additional CPU overhead. 

Disabling this improves the sustained write performance on 2U systems. On 1U systems there is little performance advantage

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">true</td>
      <td style="text-align:left">
        <p>Enable SSD CRC Checks before writing to Magnetic Storage</p>
        <p>(Default setting)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">false</td>
      <td style="text-align:left">Do not check SSD CRC data check. This improves sustained writeback performance</td>
    </tr>
  </tbody>
</table>

### EnableECC

This is mostly a debug setting. by disabling ECC it removes the RAID5 calculation and additional IO writeback. Turning the system into a RAID0 configuration. This is mostly for debug testing and not recommend for production systems

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">true</td>
      <td style="text-align:left">
        <p>Calculate ECC RAID5 Parity</p>
        <p>(Default setting)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">false</td>
      <td style="text-align:left">No ECC calculation (RAID0 mode)</td>
    </tr>
  </tbody>
</table>

## Recommended Settings

The default settings are recommended unless there is specific use cases.

### Maximum Sustained Capture  Rate

For Maximum sustained capture rate we recommended the following settings. This disables the compression and priorities Magnetic Storage writeback over download performance.

```text
["Writeback"] =
{
        ["IOPriority"]       = 40,
        ["EnableCompress"]   = false,
        ["CheckBlockCRC"]    = false,
        ["EnableECC"]        = true,
},
```

