# Capture Port Link Speed

FMADIO Capture systems capture at multiple different link speeds based on the Device Model number selected, we offer all port speeds at no additional charge. The following port configurations are supported\
\
**FMADIO100v2**:\
\- 2x100G\
\- 2x40G\
\- 4x25G  (in progress)\
\- 8x10G\


### CONFIG

Configuring the different port speeds requires updating the FPGA NIC, which requires setting the Capture Port mode and then re-updating the devices firmware. The steps are shown below:\
\
**Step 1)**\
Select the port configuration "Config Page - > Port Config" as shown below. In this example 2x10G mode is selected.

![](<../.gitbook/assets/image (117) (1).png>)

**Step 2)**\
After the port configuration has been chosen, click the "CHANGE" button to change the port speed. This will reboot the system twice as its reconfiguring the FPGA device. It will take 3-5 minutes to complete the operation

![](<../.gitbook/assets/image (115) (1) (1).png>)

**Step 3)**\
Once the update has completed, please verify the capture port configuration on the GUI dashboard, as shown below in blue.

![](<../.gitbook/assets/image (80) (1).png>)
