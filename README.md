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
Now I'll rename this PC. Right-click the Start Menu > System > Rename this PC. I'll name it "DC' meaning Domain Controller. Click Next > Restart now > Continue.  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783215/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783215_6901a6b2bb_c.jpg" width="793" height="629" alt="Rename your PC"/></a>
<br />
<br />
I have to assign an IP address to the internal adapter. The diagram above shows the "NIC (Internal)" IP address 172.16.0.1 which is what I'll assign. I'm not going to use a default gateway because the DC itself is going to serve as the default gateway. When I install Active Directory, it automatically installs DNS and this server is going to use itself as the DNS server so I'll give it a loopback address 127.0.0.1 which allows it to ping itself. This forces a restart. <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668554/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668554_50c54ccc10_c.jpg" width="800" height="635" alt="Set IP &amp; DNS server address"/></a>
<br />
<br />
After the reboot, I'll install Active Directory and create a domain. On the Server Manager Dashboard click Add roles and features > Next > Next > Next and then I'm going to select Active Directory Domain Services > Add Features. This might take a while.  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668719/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668719_342e251554_c.jpg" width="782" height="553" alt="Choose Active Directory Domain Services"/></a> <br />

<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549198/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549198_7d2da2d5e0_c.jpg" width="800" height="390" alt="AD DS Installed"/></a>
<br />
<br />
At this point I've installed the software for Active Directory Domain Services but I haven't created the domain yet. So for the post deployment configuration I'll click the flag in the upper-right corner and select "Promote this server to a domain controller".   <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53305439497/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53305439497_598f63c70f_c.jpg" width="800" height="414" alt="Promote server to the DC"/>
<br />
<br />
Add new forest and give it a simple name like mydomain.com, this forces a restart.  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549188/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549188_66f58d90b8_c.jpg" width="753" height="561" alt="Add a new forest"/></a>
<br />
<br />
Now I can see MYDOMAIN\Administrator indicating that the domain was created successfully. I'll log in with my simple password "Password1" for the purpose of this lab ONLY!  <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306396/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306396_31386bc6f5_c.jpg" width="800" height="450" alt="MYDOMAIN-Administrator"/>
<br />
<br />
Now I'll create my own domain admin account instead of using the one built in. Go to Start > Windows Administrative Tools > Active Directory Users and Computers. Then right-click on mydomain.com, select "New" in the drop down menu and select "Organizational Unit". This will house my admin account. <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306461/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306461_77cc23b48f_c.jpg" width="800" height="415" alt="Creating admin account 2"/></a> <br />
<br />
<br /> I'll name my admin account "_ADMINS". Then I'll right-click "_ADMINS" and go to New > User. In the window that opens, I'll fill in my name. NOTE: In the space "User logon name", I want to use a- to signify that this is an admin account, my first name initial and my last name (a-jlong) because this is the common naming convention in a lot of organizations.
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668674/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668674_a050ca9639_c.jpg" width="800" height="415" alt="Creating admin account 3"/></a>
<br />
<br />
I have an account now but it's still not an admin, so to make it a domain admin I need to right-click my name and in the drop down go to Properties > Member Of > Add. In the window that opens, type "domain admins" in the lower box, then click "Check Names". <br/>
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549078/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549078_af68d23b10_c.jpg" width="752" height="525" alt="Make new user an admin 1"/></a> <br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783290/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783290_f8ed1d6eb8_z.jpg" width="578" height="534" alt="Make new user an admin 2"/>
<br />
<br />
Click Ok. Now my account is a domain admin account. To use this, I'm going to sign out and sign back in with my domain admin account (a-jlong). <br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668654/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668654_0efa39b1a0_c.jpg" width="800" height="450" alt="Log in to Other user (a-jrlong2)"/></a>
<br />
<br />
Now I'm going to install the remote access server and network address translation. The purpose of RAS/NAT is to allow the client (when I install it) to be on the internal network but still have access to the internet through the domain controller. To do this, go to Add roles and features > Next > Next > select Remote Access.<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668689/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668689_438a38297b_c.jpg" width="780" height="558" alt="Configure RAS-NAT"/></a>
<br />
<br />
Next > select Routing. Then keep clicking "Next" then click "Install". This will take some time.<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306501/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306501_042d128a8c_c.jpg" width="783" height="553" alt="Configure RAS-NAT (Install Routing)"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549148/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549148_c31d556bdc_c.jpg" width="784" height="558" alt="Configure RAS-NAT (Install)"/></a>
<br />
<br />
Now in the upper-right corner I'll select "Tools" and go down to "Routing and Remote Access". Right-click on "DC (local)" and select "Configure and Enable Routing and Remote Access".
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306301/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306301_caa22a9642_c.jpg" width="800" height="450" alt="Setting up Remote Access 1"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306548963/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306548963_72e92e7842_c.jpg" width="800" height="450" alt="Setting up Remote Access 2"/></a>
<br />
<br />
Then I need to install NAT to allow my internal client to access the internet using one IP address.
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306668534/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306668534_74df8a3bc5_c.jpg" width="800" height="450" alt="Setting up Remote Access 3"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53332107947/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53332107947_c52e290f05_c.jpg" width="800" height="426" alt="Routing and Remote Access"/></a><br />
Now that RAS/NAT is configured, I'm going to set up DHCP on the domain controller with scope information so that my Windows 10 client can get an IP address that will let it get on the internet. Click on Add roles and features > Next > Next then select "DHCP Server". Keep clicking "Next", then Install.<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306783315/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306783315_1db87540c6_c.jpg" width="779" height="560" alt="Installing DHCP Server"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306306391/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306306391_441d2e4661_c.jpg" width="780" height="559" alt="Installing DHCP Server 2"/>
<br />
<br />
Looking at the diagram, I defined a DHCP scope that will give IP addresses in this range (172.16.0.100-200). Right-click "IPv4" and select "New Scope".<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53333385999/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53333385999_55a0e63b1f_c.jpg" width="800" height="450" alt="New Scope"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53333386004/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53333386004_756a57401c_c.jpg" width="800" height="466" alt="New Scope 2"/></a><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53333044651/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53333044651_f82e333b67_c.jpg" width="800" height="467" alt="New Scope 3"/></a><br />
<br />
Looking at the "Lease duration for DHCP clients" section in the image below, this defines how long a computer can have an IP address before it needs to be refreshed. For example, if you're in a cafe and you have a lease duration of 8 days, nobody will be able to use that IP address until the lease expires. But for the purpose of a home lab, this is fine.
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53305439647/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53305439647_bb1ca883a6_c.jpg" width="778" height="548" alt="Configure DHCP Scope 2"/><br />
<br />
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53306549158/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53306549158_7039737778_c.jpg" width="786" height="550" alt="Configure DHCP Scope"/></a><br />
<br />
Now I can see my IPv4 is green and my scope has been created. There are no leases in it because I haven't created the client computer yet.
<img src="<a data-flickr-embed="true" data-header="true" href="https://www.flickr.com/photos/199401609@N07/53333044666/in/album-72177720312399520/" title=><img src="https://live.staticflickr.com/65535/53333044666_0dec6a07e1_c.jpg" width="796" height="482" alt="New Scope 4"/></a><br />
<br />
Before I create my client computer, I'm going to use a PowerShell script to create over 1000 user accounts in Active Directory so I don't have to manually create them.
<br />
<br />
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
