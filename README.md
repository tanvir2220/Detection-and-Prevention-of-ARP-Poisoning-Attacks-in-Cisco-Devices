<h1>Detection and Prevention of ARP Poisoning Attacks on Cisco Devices</h1>


<h2>Overview</h2>
This project involves detection of  ARP Poisoning and ARP Spoofing attack on Cisco switches in a simulated environment. Later, I discussed the feature and configuration of DAI (Dynamic ARP Inspection) in Cisco switch to prevent ARP Poisoning attack.
<br />

<h2>Environments Used </h2>

- <b>Kali Linux</b>
- <b>Ettercap</b>
- <b>Cisco Packet Tracer</b>


<h2>Program Walk-Through</h2>

ARP Poisoning

•	The ARP (Address Resolution Protocol) is used to find the MAC address of any IP address that you are trying to reach on your local network

•	It’s a simple protocol and vulnerable to an attack called ARP Poisoning or ARP Spoofing

•	ARP poisoning is an attack where we send fake ARP reply packets on the network

•	There are two possible attacks

•	MITM (Man In The Middle): The attacker will send an ARP reply with its own MAC address and the IP address of a legitimate host, server or router

o	When the victim receives the ARP reply it will update its ARP table

o	When it tries to reach the legitimate device, the IP packets will end up at the attacker

•	DOS (Denial of Service): The attacker will send many ARP replies with the MAC address of a legitimate server

o	All devices in the network will update their ARP tables and all IP packets in the network will be sent to the server, overloading it with traffic

<p align="center">
<img src="https://i.ibb.co/6FjJHDK/1.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above we have a switch that connects two computers and a router, which is used for Internet access

•	The computer on the left side is a Windows computer with a user browsing the Internet, the computer on the top is our attacker

Traffic Pattern without ARP Poisoning

•	Let’s look at the MAC addresses and ARP tables of the host on the left side (192.168.1.1) and the router

C:\Users\host1>ipconfig /all

Physical Address. . . . . . . . . : 00-50-56-8E-5E-33

IPv4 Address. . . . . . . . . . . : 192.168.1.1(Preferred)

Subnet Mask . . . . . . . . . . . : 255.255.255.0

Default Gateway . . . . . . . . . : 192.168.1.254

•	Above you can see the MAC address and IP address of the host on the left side

•	This is a Windows 8 computer

•	The default gateway is 192.168.1.254 (R1)

•	Here’s the ARP table

C:\Users\host1>arp -a

Interface: 192.168.1.1 --- 0xc

Internet Address      Physical Address      Type

192.168.1.254         00-22-90-35-64-8a     dynamic

•	The output above is the IP address and MAC address of the router

•	We can verify the MAC address of the router like this

R1#show interfaces FastEthernet 0/0 | include bia

Hardware is Gt96k FE, address is 0022.9035.648a (bia 0022.9035.648a)

•	And here’s the ARP table of the router with an entry for the host on the left side

R1#show ip arp | include 192.168.1.1

Internet  192.168.1.1             8   0050.568e.5e33  ARPA   FastEthernet0/0

•	This is how it should be, our traffic pattern looks like this

<p align="center">
<img src="https://i.ibb.co/swHjTmh/2.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Now let’s see what happens when we perform an ARP poisoning attack

Traffic Pattern with ARP Poisoning

•	There are several tools you can use for ARP poisoning

•	I decided to use Kali which is a great Linux distribution with plenty of security tools

•	Kali comes with an application called Ettercap which offers a couple of MITM (Man in the Middle) attacks

•	Do yourself a favour and never try ARP poisoning on any production network, you should always use a lab environment to test any security tools

•	There are methods to detect ARP poisoning which we will cover in the DAI (Dynamic ARP Inspection) lesson

•	I will launch Ettercap Graphical interface on the host with IP address 192.168.1.2

•	You will be greeted with the following screen

<p align="center">
<img src="https://i.ibb.co/RgqJ33W/3.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Open the Sniff menu and select Unified sniffing

<p align="center">
<img src="https://i.ibb.co/18ncT6h/4.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Select your interface and click OK

<p align="center">
<img src="https://i.ibb.co/8Ph9Yzw/8.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Now open the Hosts menu and select Hosts list

<p align="center">
<img src="https://i.ibb.co/FgctHsp/5.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	It should show you the following screen

<p align="center">
<img src="https://i.ibb.co/tLtD1bx/6.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above you can see our host (192.168.1.1) and our router (192.168.1.254)

•	We can ignore the IPv6 address

•	Now open the Mitm menu and select ARP poisoning

<p align="center">
<img src="https://i.ibb.co/JyWxWyD/7.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Select the Sniff remote connections option

•	This means that Ettercap will send ARP replies to the host and the router

<p align="center">
<img src="https://i.ibb.co/8Ph9Yzw/8.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Once you have clicked OK, Ettercap will start sending ARP replies

•	This is what it looks like in Wireshark

<p align="center">
<img src="https://i.ibb.co/GJdCSDZ/9.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above you can see the ARP reply that it sent towards the host

•	The sender MAC address is of the ATTACKER and the sender IP address is from the ROUTER

•	Here’s the ARP reply that it sends to the router

<p align="center">
<img src="https://i.ibb.co/5kPJfJD/10.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above we see the sender MAC address is the ATTACKER and the sender IP address is the HOST

•	Once our host and router receives these ARP replies, they will update their ARP tables:

C:\Users\host1>arp -a

Interface: 192.168.1.1 --- 0xc

Internet Address      Physical Address      Type

192.168.1.254         00-50-56-8e-ee-89     dynamic

R1#show ip arp | include 192.168.1.1

Internet  192.168.1.1             0   0050.568e.ee89  ARPA   FastEthernet0/0

•	Above you see the MAC address of the attacker in both entries in the ARP tables

•	We can verify the MAC address of the attacker with the ifconfig command

• ifconfig eth0

eth0      Link encap:Ethernet  HWaddr 00:50:56:8e:ee:89

•	Our traffic pattern now looks like this

<p align="center">
<img src="https://i.ibb.co/prNp5sh/11.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Our MITM attack is successful

•	All traffic now flows through the attacker’s computer

•	It’s up to your imagination what kind of bad stuff you want to do now

•	Let me give you an example

•	Kali has an application called Driftnet that shows you all images that are requested

•	Let’s start it on the attacker’s computer

• driftnet -i eth0

•	On the victim’s computer I visited Cisco.com and opened a few pages

•	Here’s what driftnet shows us

<p align="center">
<img src="https://i.ibb.co/vDn0Pm4/12.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above you see a nice overview with some of the images on the Cisco website

•	This is a fun example but there are also applications that let you capture authentication information for protocols like FTP, POP3, etc.

# DAI (Dynamic ARP Inspection)

•	Dynamic ARP Inspection (DAI) is a security feature that protects ARP (Address Resolution Protocol) which is vulnerable to an attack like ARP poisoning

•	DAI checks all ARP packets on untrusted interfaces

•	It will compare the information in the ARP packet with the DHCP snooping database and/or an ARP access-list

•	If the information in the ARP packet doesn’t matter, it will be dropped

<p align="center">
<img src="https://i.ibb.co/hXXncgW/13.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

•	Above we have four devices

•	The router on the left side called host will be a DHCP client, the router on the right side is our DHCP server and on top we have a router that will be used as an attacker

•	The switch in the middle will be configured for Dynamic ARP Inspection

Configuration

•	We’ll start with the switch

•	First, we need to make sure that all interfaces are in the same VLAN

SW1(config)#interface range fa0/1 - 3

SW1(config-if-range)#switchport mode access

SW1(config-if-range)#switchport access vlan 123

SW1(config-if-range)#spanning-tree portfast

•	Now we can configure DHCP snooping

SW1(config)#ip dhcp snooping 

SW1(config)#ip dhcp snooping vlan 123

SW1(config)#no ip dhcp snooping information option 

•	The commands above will enable DHCP snooping globally for VLAN 123 and disables the insertion of option 82 in DHCP packets

•	Don’t forget to make the interface that connects to the DHCP server trusted

SW1(config)#interface FastEthernet 0/3

SW1(config-if)#ip dhcp snooping trust 

•	The switch will now keep track of DHCP messages

•	Let’s configure a DHCP server on the router on the right side

DHCP(config)#ip dhcp pool MY_POOL

DHCP(dhcp-config)#network 192.168.1.0 255.255.255.0

•	That’s all we need

•	Let’s see if the host can get an IP address

HOST(config)#interface FastEthernet 0/0

HOST(config-if)#ip address dhcp 

•	A few seconds later we see this message:

%DHCP-6-ADDRESS_ASSIGN: Interface FastEthernet0/0 assigned DHCP address 192.168.1.1, mask 255.255.255.0, hostname HOST

•	Let’s check if our switch has stored something in the DHCP snooping database

SW1#show ip dhcp snooping binding 

MacAddress          	IpAddress        	Lease(sec)  	Type           	VLAN  	Interface

------------------  		---------------  	----------  	-------------  	----  	--------------------

00:1D:A1:8B:36:D0   	192.168.1.1      	86330       	dhcp-snooping   123   	FastEthernet0/1

Total number of bindings: 1

•	There it is, an entry with the MAC address and IP address of our host

•	Now we can continue with the configuration of DAI

•	There’s only one command required to activate it

SW1(config)#ip arp inspection vlan 123

•	The switch will now check all ARP packets on untrusted interfaces

•	All interfaces are untrusted by default

•	Let’s see if this will work or not

•	I’ll configure the IP address of our host on our attacker

ATTACK(config)#interface FastEthernet 0/0

ATTACK(config-if)#ip address 192.168.1.1 255.255.255.0

•	Now let’s see what happens when we try to send a ping from the attacker to our DHCP router

ATTACK#ping 192.168.1.254

•	The ping is failing

•	What does our switch think of this?

SW1#
%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Req) on Fa0/2, vlan 123.([0017.5aed.7af0/192.168.1.1/0000.0000.0000/192.168.1.254/01:20:08 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Req) on Fa0/2, vlan 123.([0017.5aed.7af0/192.168.1.1/0000.0000.0000/192.168.1.254/01:20:10 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Req) on Fa0/2, vlan 123.([0017.5aed.7af0/192.168.1.1/0000.0000.0000/192.168.1.254/01:20:10 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Req) on Fa0/2, vlan 123.([0017.5aed.7af0/192.168.1.1/0000.0000.0000/192.168.1.254/01:20:10 UTC Tue Mar 2 1993])

•	Above you can see that all ARP requests from our attacker are dropped

•	The switch checks the information found in the ARP request and compares it with the information in the DHCP snooping database

•	Since it doesn’t match, these packets are discarded

•	You can find the number of dropped ARP packets with the following command

SW1#show ip arp inspection

•	Above you see the number of drops increase

•	So far so good, our attacker has been stopped

•	We still have one problem though

•	Let me first shut the interface on our attacker before we continue:

ATTACK(config)#interface FastEthernet 0/0

ATTACK(config-if)#shutdown

•	Let me show you what happens when we try to send a ping from the host to our DHCP router

HOST#ping 192.168.1.254

•	This ping is failing but why?

•	We are not spoofing anything

•	Here’s what the switch tells us

SW1#

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Res) on Fa0/3, vlan 123.([0016.c7be.0ec8/192.168.1.254/001d.a18b.36d0/192.168.1.1/01:24:48 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Res) on Fa0/3, vlan 123.([0016.c7be.0ec8/192.168.1.254/001d.a18b.36d0/192.168.1.1/01:24:50 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Res) on Fa0/3, vlan 123.([0016.c7be.0ec8/192.168.1.254/001d.a18b.36d0/192.168.1.1/01:24:52 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Res) on Fa0/3, vlan 123.([0016.c7be.0ec8/192.168.1.254/001d.a18b.36d0/192.168.1.1/01:24:54 UTC Tue Mar 2 1993])

%SW_DAI-4-DHCP_SNOOPING_DENY: 1 Invalid ARPs (Res) on Fa0/3, vlan 123.([0016.c7be.0ec8/192.168.1.254/001d.a18b.36d0/192.168.1.1/01:24:56 UTC Tue Mar 2 1993])

•	Our switch is dropping ARP replies from the DHCP router to our host

•	Since the DHCP router has no idea how to reach the host, the ping is failing

HOST#show ip arp

Protocol  Address          		Age (min)  	Hardware Addr   Type   		Interface

Internet  192.168.1.1             	-   		001d.a18b.36d0  ARPA   	FastEthernet0/0

Internet  192.168.1.254           	0   		Incomplete      	  ARPA

DHCP#show ip arp

Protocol  Address          		Age (min)  	Hardware Addr   Type   		Interface

Internet  192.168.1.1             	0   		001d.a18b.36d0  ARPA   	FastEthernet0/0

Internet  192.168.1.254           	-   		0016.c7be.0ec8   ARPA   	FastEthernet0/0

•	Why is the switch dropping the ARP reply?

•	The problem is that the DHCP router is using a static IP address

•	DAI checks the DHCP snooping database for all packets that arrive on untrusted interfaces,

•	When it doesn’t find a match the ARP packet is dropped

•	To fix this, we need to create a static entry for our DHCP router

SW1(config)#arp access-list DHCP_ROUTER

SW1(config-arp-nacl)#permit ip host 192.168.1.254 mac host 0016.c7be.0ec8

•	First, we create an ARP access-list with a permit statement for the IP address and MAC address of the DHCP router

•	Now we need to apply this to DAI

SW1(config)#ip arp inspection filter DHCP_ROUTER vlan 123 ?   

static  Apply the ACL statically

•	We use the ip arp inspection filter command for this

•	But you have to be careful if you use the static parameter

•	We tell the switch not to check the DHCP snooping database

•	It will only check our ARP access-list and when it doesn’t find an entry, the ARP packet will be dropped

•	Make sure you add the filter without the static parameter

SW1(config)#ip arp inspection filter DHCP_ROUTER vlan 123

•	There we go

•	The switch will now check the ARP access-list first and when it doesn’t find a match, it will check the DHCP snooping database

•	Let’s try that ping again

HOST#ping 192.168.1.254

•	Excellent our ping is now working because of the static entry for the DHCP router

•	Another way to deal with this issue is to configure the interface as trusted

•	DAI will allow all ARP packets on trusted interfaces

SW1(config)#interface FastEthernet 0/3

SW1(config-if)#ip arp inspection trust

•	Anything else we can do with DAI?

•	There are some additional security checks you can enable if you want

SW1(config)#ip arp inspection validate ?

dst-mac  	Validate destination MAC address

ip       		Validate IP addresses

src-mac  	Validate source MAC address

•	Here’s what these options mean

•	dst-mac: Checks the destination MAC address in the Ethernet header against the target MAC address in the ARP packet

o	This check is performed for ARP replies

o	ARP replies with different MAC addresses will be dropped

•	ip: Checks for invalid and unexpected IP addresses

o	For example, 0.0.0.0, 255.255.255.255 and multicast addresses

•	src-mac: Checks the source MAC address in the Ethernet header against the sender’s MAC address in the ARP packet

o	This check is performed for both ARP requests and replies

o	ARP packets with different MAC addresses will be dropped

o	You can only enable one of these options at the same time

o	Here’s an example how to enable the dst-mac check

SW1(config)#ip arp inspection validate dst-mac

•	Finally, we can also configure ARP rate-limiting

•	By default, there is a limit of 15 pps for ARP traffic on untrusted interfaces

•	Here’s how you can change it

SW1(config)#interface FastEthernet 0/1

SW1(config-if)#ip arp inspection limit rate 10

•	This interface now only allows 10 ARP packets per second

Conclusion

•	That’s all we have for DAI (Dynamic ARP Inspection)

•	It’s a nice security feature but make sure that you have ARP access-lists in place for all devices with static IP addresses before you enable this

•	You don’t want to block most of your traffic after enabling this

