---
layout: post
title:  "Homelab Evolution"
date:   2022-01-23 21:01:02 +0900
categories: Network 
author: nathan
tag: 
  - Diagram
  - Topology
---

When moving to South Korea I only had my laptop and a monitor, my home network/homelab evolved over time according to my needs.

# In this blog post
1\. [Intro](#Intro)  
2\. [March 2018 - All in one server](#March 2018)  
3\. [July 2018 - WAP and Switch](#July 2018)  
4\. [November 2018 - Odroid HC2 and Qotom pfSense](#November 2018)  
5\. [October 2019 - Synology NAS](#October 2019)  
6\. [February 2020 - New Laptop, Repurpose Old Laptop](#February 2020)  
7\. [January 2022 - New Home, New Needs](#January 2022)  
8\. [Future](#Future)  


# 1. Intro <a name="Intro"></a>
I could only take my laptop and computer monitor when moving from my hometown in Belgium to South Korea. I like to research things that can make my life easier and I also like technology so I built this homelab which evolved as my needs changed. This blog post is all about sharing my road to the homelab I have now.

![homelab](/images/homelab_evolution/homelab.gif)


# 2. March 2018 - All-in-one server<a name="March 2018"></a>
After arriving in South Korea I was on the lookout for a server to use as a Router/GNS3 server/NAS all-in-one solution. I found a workstation quite cheap on a secondhand site, for 220.000 KRW (around 180 USD) I bought my first server in Korea. 

The use case of this server was to run an ESXI server, virtualize pfSense and FreeNAS so I could have my own router and storage solution. 

This server did not have all the required hardware so I bought the following extra hardware:
- RAM
- HBA (host bus adapter)
- 4 port intel network card
- Hard drives

## 2.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2018-03.png)


# 3. July 2018 - WAP and Switch<a name="July 2018"></a>
Having my own wireless solution is something I was looking out for so at July 2018 I bought a Unifi WAP AC LR. I installed the WLC on my PC which makes it easy to manage the WAP. I wanted to have 2 wireless networks, one for all IOT devices or other devices which I do not trust and one for the devices I trust. This requires the use of VLANs to separate both networks. To use VLANs, a supported managed switch is required so I bought a TP-Link TL-SG108E which gives me the ability to work with VLANs.

## 3.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2018-07.png)

# 4. November 2018 - Odroid HC2 and Qotom pfSense<a name="November 2018"></a>
The all-in-one server was too power-hungry and it was uncomfortable to run my router on the same device as the other VMs so I decided to separate the functionality in their own hardware. I bought a Qotom PC where I am running pfSense for my router and an Odroid HC2 where I am using OMV (open media vault) as a NAS solution. I decided to configure OpenVPN which gives me access to my internal network or to just connect with in case I am connected to an unsafe wireless network. Under OMV I am running docker with PiHole as my DNS server. Duck DNS is a dynamic DNS solution which I am using to connect the VPN so there are no connection issues even if the WAN IP changes.

## 4.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2019-10.png)

# 5. October 2019 - Synology NAS<a name="October 2019"></a>
The Odroid HC2 is a fun device but it was having stability issues and I wanted to have 2 mirrored hard drives just for peace of mind if one hard drive fails. I was also getting interested in building a media center and these containers require some more RAM so I bought a Synology NAS DS218+. The built-in 2GB was not enough so I bought 8GB extra RAM which was sufficient to run all the containers I want.

## 5.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2019-10.png)

# 6. February 2020 - New Laptop, Repurpose Old Laptop<a name="February 2020"></a>
The screen hinges of my laptop Dell Vostro V131 failed and while installing new ones I damaged my screen. I did not want to invest in this 6-year-old laptop so I decided to but a new laptop, the Lenovo T490. I repurposed the old laptop, disconnected the screen, and installed Proxmox as I wanted to try out something else as the familiar VMware ESXI. Proxmox was a good experience and not so different from ESXI. I also experimented with PRTG network monitoring that I set up to send me mails in case the servers are down.

## 6.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2020-02.png)

# 7. January 2022 - New Home, New Needs<a name="January 2022"></a>
Moving to a new home changed the place where the NAS is located. Due to COVID-19 and all the working from home, I need to work next to where the NAS is located. I tried to silence the NAS with velcro strips and rubber strips at the HDD bays and also bought shock-absorbing pads to put underneath the NAS but it was still too loud. We are mainly watching Netflix so I decided to stop most of the containers which were running on the NAS because they kept the hard drives busy the whole time. With the hard drives silent the NAS almost makes no noise anymore. I bought my own domain, nabomang.com with comes from my initials and the Korean word for network. Some family is still using the external VPN so I kept the Duck DNS service but I am also trying out the Namecheap DDNS service which is stable until now. I also migrated from OpenVPN to WireGuard as VPN solution as there is now a package available for pfSense. I changed from GNS3 to EVE-NG as a network virtualization solution and I feel that the interface fits better with me.

## 7.1 Topology <a name="Topology"></a>

![homelab](/images/homelab_evolution/HomeNetwork_2022-01.png)



# 8. Future<a name="Future"></a>
When I find time I am planning to start using the Odroid HC2 again and run some Docker containers on this device. My homelab will keep changing as my needs keep evolving.
