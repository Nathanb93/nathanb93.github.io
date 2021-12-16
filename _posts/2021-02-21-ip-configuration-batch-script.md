---
layout: post
title:  "IP Config Batch Script"
date:   2021-02-21 15:01:02 +0900
categories: Windows 
author: nathan
tag: 
  - Automation
  - Batch
---

<script src="https://unpkg.com/vanilla-back-to-top@7.2.1/dist/vanilla-back-to-top.min.js"></script>
<script>addBackToTop({})</script>

Changing an IP address on a Windows PC multiple times a day can be time consuming. Learn here how to make a batch script that helps in automating this process.

### 1. Explanation
If you have to change your IP address from static to DHCP and back to static you will know that it takes some time to change this. You have to go to the adapter menu and change the adapter settings.
In this tutorial I will give some example batch scripts to change the adapter IP address settings without going to the menu.
The adapter name can change from computer to computer so best to check your adapter name by typing "ipconfig" in the cmd window.

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
