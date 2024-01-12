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

<p align="center">
<img src="https://i.ibb.co/RgqJ33W/3.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/18ncT6h/4.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/FgctHsp/5.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/tLtD1bx/6.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/JyWxWyD/7.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/8Ph9Yzw/8.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/GJdCSDZ/9.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/5kPJfJD/10.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/prNp5sh/11.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/vDn0Pm4/12.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />

<p align="center">
<img src="https://i.ibb.co/hXXncgW/13.png" height="80%" width="80%" alt="Detection and Prevention of ARP Poisoning Attacks in Cisco Devices"/>
<br />
<br />
