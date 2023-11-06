<h1>Active Directory Home Lab</h1>

 ### 

<h2>Description</h2>
This repository is a walkthrough of how I built an Active Directory Home Lab Environment. The goal here is to develop my understanding of how Active Directory and Windows networking works. To do this, I'll be using Server 2019 as the domain controller which will house Active Directory and two network adapters, and Windows 10 as the client machine on the internal network. I'm going to run a PowerShell script to automatically create 1000+ users in Active Directory, install Windows 10 and join it to the domain, then log into it with one of our domain accounts. I'll be using Oracle Virtual Box for this home lab.


<h2>Languages and Utilities Used</h2>

- <b>Active Directory</b> 
- <b>PowerShell</b>
- <b>CMD</b>

<h2>Environments Used </h2>

- <b>Oracle Virtual Box: https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html</b>
- <b>Microsoft Server 2019: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019</b>
- <b>Windows 10 (22H2): https://www.microsoft.com/en-us/software-download/windows10

<h2>Program walk-through:</h2>

<p align="center">
NETWORK OVERVIEW <br/>
<img <a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549193/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549193_cd69d8610a_c.jpg" width="800" height="479"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
