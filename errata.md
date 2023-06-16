---
description: Errata related to FMADIO100v2 Capture System
---

# Errata

## Capture Port 0&#x20;

Due to design decision of the FPGA clocking QSFP0 Capture port must be connected first or put in shutdown mode using the fmadiocli.

Recommended configuration is always connect QSFP0 port first

### Root cause

The reason is if the frontend of the capture FPGA uses the clock 322mhz clock that runs off the QSFP0 port FPGA GTY transceiver. If the port is left un-connected and not shutdown the system will try automatically link up in different modes. The constant attempted link up, transitioning between modes (eg FEC and non-FEC) requires resetting of the clock resulting in unstable behavior on Capture Port 1
