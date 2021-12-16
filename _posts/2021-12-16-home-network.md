---
layout: post
title:  "Home Network"
date:   2021-12-16 20:01:02 +0900
categories: Network 
author: nathan
tag: 
  - Diagram
  - Topology
---

<script src="https://unpkg.com/vanilla-back-to-top@7.2.1/dist/vanilla-back-to-top.min.js"></script>
<script>addBackToTop({})</script>

When moving to South-Korea I only had my laptop and a monitor but my home network/homelab evolved over time.

### Table of contents
1. [Intro](#Intro)
2. [All in one server](#All in one server)

### 1. Intro <a name="Intro"></a>
I could only take my laptop and computer monitor when moving from my hometown in Belgium to South-Korea. 

At first I tried an all in one virtualized solution with 1 server but it turned into the following:
- 2 ESXI servers
- 1 Qotom pfSense router
- 1 Unifi AP
- 1 TP-Link switch
- 1 Cisco switch
- 2 Hardkernel Odroid single board computers
- 1 Raspberry Pi

### 2. All in one server <a name="All in one server"></a>
After arriving in South-Korea I was on the lookout for a server to use as Router/GNS3 server/NAS. I found a workstation quite cheap on a secondhand site, for 220.000 KRW I bought my first server in Korea. 

The use case of this servers was to run an ESXI server, virtualize pfSense and FreeNAS so I could have my own router and storage solution. 

This server did not have all the required hardware so I bought the following:
- RAM
- HBA (host bus adapter)
- 4 port intel network card
- Hard Drives and SSD

#### 3.0 Hardware <a name="Hardware"></a>
##### 3.1 RAM <a name="RAM"></a>
This server not have enough RAM and it was not ECC RAM so I bought some new ECC RAM through Ebay, but after installing this new RAM, the only thing I got were some beeps and my server did not boot anymore. After lots of searching I found out that I bought unbuffered RAM instead of the compatible buffured RAM. I did not even know that buffered or unbuffered RAM existed. So I went to the electronics market in Yongsan-gu and bought 16GB compatible RAM sticks, 2 times 8GB. There were already 2 sticks of 4GB in the servers so I had a total of 24GB RAM. 

After playing around with this server, running some containers in jails and creating a vsphere server I was running out of memory so I went back and bought another 8GB stick.

##### 3.2 HBA <a name="HBA"></a>
FreeNAS, now rebranded to TrueNAS CORE is an open source free project which enables you to create a NAS appliance. The forums are recomending to go for an HBA (host bus adapter), this is an adapter card that plugs in to an PCI slot and has multiple sata connectors. It enalbes to connect multiple hard drives to one PC. I bought the LSI-9211-8i and converted it to IT mode by flashing the firmware. This is recomended because FreeNAS is doing software RAID so we do not want the HBA itself to perform any RAID functions. After following some guides I was able to passtrough this HBA directly to FreeNAS, this enables FreeNAS to directly interact with this HBA.

##### 3.3 Intel network card <a name="Intel network card"></a>
The workstation only had 1 onboard NIC, which is technically enough when working with VLANs but as the home router did not support VLANs I purchases a new 4 port 1 Gig network card so I could terminate the WAN link in the virtualized pfSense instance and the other ports could be used for the LAN. Just like the HBA I also used passtrhough on ESXI to pfSense VM so pfSense could directly use this LAN card.

##### 3.4 Hard Drives and SSD <a name="Hard Drives and SSD"></a>
For Hard Drives I went for Western Digital 4TB which I bought locally at the electronics market in Yongsan-gu. I bought the SSD through Amazon. The HDDs were used in FreeNAS and the SSD was used to run the OS and also had the datastore for all the VMs.

#### 4.0 Software <a name="Software"></a>
##### 4.1 ESXI <a name="ESXI"></a>
As I wanted an all in one solution I went for ESXI. Within ESXI I had multiple VMs, these VMs were:
- pfSense
- FreeNAS
- GNS3
- Windows 10
- Debian



### 2. Batch scripts

#### 2.1 DHCP
The following script will change the adapter named "Ethernet" dns and IP address to DHCP. In the end it will show you the adapter new IP settings. You can press q to exit the script.
```bash
  @echo off
  netsh interface ip set address "Ethernet" dhcp
  netsh interface ip set dns name="Ethernet" dhcp
  :loop
  	netsh interface ip show config name="Ethernet" | findstr /V Metric | findstr /V suffix | findstr /V WINS
  	set /p Quit=To close this window press q:
  	if [%Quit%]==[q] exit
  goto loop
```

#### 2.2 Static IP
The following script will expect you to input an IP address, default gateway and subnet mask. It will then change the adapter named "Ethernet" with the information you provided. In the end it will show you the adapter new IP settings. You can press q to exit the script.
```bash
  @echo off
  echo "Static IP Address:"
  set /p IP_Address=

  echo "Default Gateway:"
  set /p Default_Gateway=

  echo "Subnet Mask:"
  set /p Subnet_Mask=

  netsh interface ip set address "Ethernet" static %IP_Address% %Subnet_Mask% %Default_Gateway%
  :loop
  	netsh interface ip show config name="Ethernet" | findstr /V Metric | findstr /V suffix | findstr /V WINS
  	set /p Quit=To close this window press q:
  	if [%Quit%]==[q] exit
  goto loop
```

#### 2.3 What is my IP
This script will show you the "Ethernet" and "Wi-Fi" adapter IP settings. You can press q to exit the script.
```bash
  @echo off
  :loop
  	netsh interface ip show config name="Ethernet" | findstr /V Metric | findstr /V suffix | findstr /V WINS
  	netsh interface ip show config name="Wi-Fi" | findstr /V Metric | findstr /V suffix | findstr /V WINS
  	set /p Quit=To close this window press q:
  	if [%Quit%]==[q] exit
  goto loop
```

#### 2.4 Explenation of commands used in scripts
`@echo off` this will make sure that only the required output is printed on the screen. The commands that are used inside the scripts will not be shown in the output."

`netsh interface ip set` changes the IP address and DNS settings of the Ethernet adapter to DHCP.

`netsh interface ip show` shows information about the specified interface. The `/V` will exclude the information we do not require.

`:loop` this will make a placeholder in the script where you can later on reffer to so you can jump back to this place in the script. You can give this any name you want. In this case it is named loop because we are using this as a loop.

`goto loop` this will jump back to the placeholder we made earlier which we named "loop"

`set /p Quit=` this will wait on an input of the user and place the input in the Quit variable

`if [%Quit%]==[q] exit` this will compare the user input with "q" if it is "q" then it will end the program which will also end the loop.


