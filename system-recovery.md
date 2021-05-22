# System Recovery

In the unlikely event of a complete boot failure, system can be recovered by booting via the Virtual CDROM interface over a HTML BMC connection

Start by going to the BMC interface \(default IP is 192.168.0.93\) contact us for default login/password

![](.gitbook/assets/image.png)

Start the Remote HTML KVM

![](.gitbook/assets/image%20%288%29.png)

Will look like this. Select Brose Files, selecting an ISO image + Start the Media

![](.gitbook/assets/image%20%285%29.png)



System will boot Ubuntu \(for example\), we are using \( systemrescue 8.01 amd64\)

[https://sourceforge.net/projects/systemrescuecd/files/sysresccd-x86/8.01/systemrescue-8.01-amd64.iso/download](https://sourceforge.net/projects/systemrescuecd/files/sysresccd-x86/8.01/systemrescue-8.01-amd64.iso/download)

![](.gitbook/assets/image%20%286%29.png)

System will boot as follows, it may take several minutes depending on the speed of the HTML &lt;-&gt; FMADIO System connection. Recommend the closer the HTML instance is to the FMAD device the better.



















