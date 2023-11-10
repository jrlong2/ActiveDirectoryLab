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
Download and install Virtual Box using the link above. The first virtual machine will house the domain controller so I'll need two network adapters, one to access the internet (NAT) and one dedicated to the internal network (Internal Network) so that the Domain Controller can communicate with the internal network. Go to Settings > Network > Advanced to make these changes.  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783175/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783175_ceb4491599_c.jpg" width="800" height="490" alt="Settings - Network - Adapter 1 - NAT"/></a>
<br />

<br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783160/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783160_3329f1c7a0_c.jpg" width="800" height="492" alt="Settings - Network - Adapter 2 - Internal Network"/>
<br />
<br />
I'll give the domain controller a simple name (DC) and in the General > Advanced tab, I'll set the clipboard to "Bidrectional" which allows me to drag and drop things from my actual desktop into this virtual machine and visa versa.  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306426/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306426_5832cfc43d_c.jpg" width="800" height="491" alt="DC Settings"/></a>
<br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" data-context="true" href="https://www.flickr.com/photos/199401609@N07/53306548923/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306548923_baa5decca5_c.jpg" width="800" height="491" alt="Settings - General - Advanced - Bidirectional"/></a>
<br />
<br />
Click "Ok". Now double-click the domain controller (DC) and navigate to the directory where I downloaded Server 2019, select it and click "Mount and Retry Boot". This might take a while.  <br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53305439432/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53305439432_b8c9577613_c.jpg" width="800" height="492" alt="Server 2019 iso"/></a>
<br />
<br />
Once Server 2019 is installed, I have to configure IP addressing for the internal (private) network. The external (internet) network automatically gets IP addressing from my home router. Click "Change adapter options".  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668724/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668724_c873aa0184_c.jpg" width="799" height="633" alt="Change Adapter Options (Set IP address)"/></a>
<br />
<br />
Now I have to figure out which one of these adapters connects to the internet and name both approriately because I will be using them later when I set up routing. I'll right-click the first one and go to Status > Details. I can see that the IP address is 10.0.2.15 which looks like a proper home IP address so I'll rename it as _INTERNET_ and the other as X_Internal_X. <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306476/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306476_66f5b05075_c.jpg" width="792" height="633" alt="Configuring Network Adapters 3"/></a>
<br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783365/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783365_f9c9dd3222_c.jpg" width="799" height="636" alt="Configuring Network Adapters 4"/></a>
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
