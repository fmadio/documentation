---
description: Errata related to FMADIO100v2 Capture System
---

# Errata

## Capture Port 0&#x20;

Due to design decision of the FPGA clocking QSFP0 Capture port must be connected first or put in shutdown mode using the fmadiocli.

Recommended configuration is always connect QSFP0 port first

### Root cause

The reason is if the frontend of the capture FPGA uses the clock 322mhz clock off that runs off the QSFP0 port. If the port is left un-connected and not shutdown the system will try automatically link up in different modes. Transitioning between modes requires resetting of the clock resulting in unstable behavior on Capture Port 1
