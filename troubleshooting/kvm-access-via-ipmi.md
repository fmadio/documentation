# KVM Access via IPMI

FMADIO systems include a full full Keyboard Video Mouse (KVM) remote access using the onboard AST2500/AST2600 BMC chipset. This enables remote power cycles, reboots and others all using a simple HTTPS HTML5 web browser (No requirement for Java)

To log into the system via the KVM please run the following steps

**STEP 1**

Log into the IPMI/BMC system, please contact support@fmad.io for default IP and credentials.

It uses a self signed certificate unless a certified certificate has been uploaded. Please click thru to the login page

![Self Signed Certificate](<../.gitbook/assets/image (118) (1).png>)

Login page looks like the following. Please contact support@fmad.io for default credentials

![IPMI BMC Login page](<../.gitbook/assets/image (123) (1) (1).png>)

**STEP 2**

Select "Remote Control" on the left menu

![FMADIO IPMI Remote Control](<../.gitbook/assets/image (130).png>)

**STEP 3**

Select "H5Viewer" to launch the web HTML KVM page.&#x20;

NOTE: this uses normal HTTPS connection no additional firewall rules or ports need to be opened

![FMADIO HTML5 KVM](<../.gitbook/assets/image (125) (1).png>)

**STEP 4**

HTML5 KVM viewer is launched. Looks like the following below. From here please login to the FMADIO device to setup and configuration.

Due to screen saver, it may appear blank. Please press any key to disable the screen saver.

NOTE: the currently configured IPs are shown at boot.

![](<../.gitbook/assets/image (121) (1).png>)

