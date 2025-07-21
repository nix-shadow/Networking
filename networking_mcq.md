# Networking MCQ Practice (61–80) – Newbie Friendly, With Answers & Explanations

---

## 61. What is the main difference between a router and a switch?

a) A router connects devices in the same network; a switch connects different networks  
b) A switch forwards packets based on MAC address; a router forwards packets based on IP address  
c) A router only works with wireless devices; a switch only works with wired devices  
d) Both router and switch use the same method for packet forwarding  

**Answer:** b) A switch forwards packets based on MAC address; a router forwards packets based on IP address  
**Explanation:**  
- Switches use MAC addresses to forward frames within the same network (Layer 2).
- Routers use IP addresses to route packets between different networks (Layer 3).

---

## 62. What is a broadcast domain?

a) Area where packets can collide  
b) All devices that receive broadcast frames sent by any device within the domain  
c) Only devices that use static IP addresses  
d) Section of network separated by a switch  

**Answer:** b) All devices that receive broadcast frames sent by any device within the domain  
**Explanation:**  
- Devices in the same broadcast domain receive broadcast traffic.
- Routers separate broadcast domains; switches do not unless VLANs are used.

---

## 63. Which device reduces collision domains in a network?

a) Hub  
b) Switch  
c) Router  
d) Modem  

**Answer:** b) Switch  
**Explanation:**  
- Switches create a separate collision domain for each port, reducing collisions.

---

## 64. What does the ARP protocol do in networking?

a) Assigns IP addresses to devices  
b) Encrypts network traffic  
c) Resolves IP addresses to MAC addresses  
d) Routes packets between networks  

**Answer:** c) Resolves IP addresses to MAC addresses  
**Explanation:**  
- ARP (Address Resolution Protocol) helps devices find the MAC address for a known IP address on the local subnet.

---

## 65. What is the function of a default gateway?

a) Assigns IP addresses  
b) Connects a device to other networks  
c) Blocks Internet traffic  
d) Encrypts data  

**Answer:** b) Connects a device to other networks  
**Explanation:**  
- The default gateway is the next-hop router that forwards packets destined for other networks.

---

## 66. In the DORA process, what is the purpose of the DHCP Offer?

a) The client requests an IP address  
b) The server offers an available IP address to the client  
c) The client acknowledges the IP address  
d) The server requests the client's MAC address  

**Answer:** b) The server offers an available IP address to the client  
**Explanation:**  
- DHCP Offer is the server's response to the client's Discover message, offering an IP and configuration details.

---

## 67. What is the main advantage of using VLANs in a network?

a) Faster wireless speed  
b) Increased collision domains  
c) Segmentation of broadcast domains for better security and management  
d) Allows only one device to connect at a time  

**Answer:** c) Segmentation of broadcast domains for better security and management  
**Explanation:**  
- VLANs allow you to partition a network into logical segments, reducing broadcast traffic and improving security.

---

## 68. Which protocol is used by switches to prevent loops in the network?

a) RIP  
b) OSPF  
c) STP  
d) DNS  

**Answer:** c) STP  
**Explanation:**  
- Spanning Tree Protocol blocks redundant paths, preventing switching loops.

---

## 69. What is a collision domain?

a) Area where broadcast traffic occurs  
b) Section of network where packet collisions can happen  
c) Network separated by a router  
d) VLAN segment  

**Answer:** b) Section of network where packet collisions can happen  
**Explanation:**  
- In Ethernet, a collision domain is a network segment where data packets can collide with one another.

---

## 70. How does a switch learn MAC addresses of devices?

a) Manually entered by an admin  
b) By monitoring the source MAC address of incoming frames  
c) By using DNS  
d) Through routing tables  

**Answer:** b) By monitoring the source MAC address of incoming frames  
**Explanation:**  
- Switches build a MAC address table by learning the source MAC addresses of frames they receive.

---

## 71. Which layer of the OSI model does a switch operate?

a) Layer 1  
b) Layer 2  
c) Layer 3  
d) Layer 4  

**Answer:** b) Layer 2  
**Explanation:**  
- Switches operate at the Data Link layer, forwarding frames based on MAC addresses.

---

## 72. What is the main role of the Transport layer in the OSI model?

a) Routing packets  
b) Reliable data transfer and error correction  
c) Assigning IP addresses  
d) Encrypting data  

**Answer:** b) Reliable data transfer and error correction  
**Explanation:**  
- The Transport layer (Layer 4) manages end-to-end connections and reliability (TCP/UDP).

---

## 73. What is a MAC address?

a) Unique identifier for a network interface card  
b) IP address for a device  
c) VLAN number  
d) Port number  

**Answer:** a) Unique identifier for a network interface card  
**Explanation:**  
- MAC addresses are hardware identifiers used at Layer 2 for local network communication.

---

## 74. Which protocol helps prevent DHCP spoofing attacks?

a) DHCP Snooping  
b) ARP  
c) ICMP  
d) NTP  

**Answer:** a) DHCP Snooping  
**Explanation:**  
- DHCP Snooping allows only trusted ports to send DHCP responses, blocking rogue servers.

---

## 75. What is the function of NAT?

a) Encrypts network traffic  
b) Translates private IP addresses to public IP addresses for Internet access  
c) Assigns MAC addresses  
d) Prevents broadcast storms  

**Answer:** b) Translates private IP addresses to public IP addresses for Internet access  
**Explanation:**  
- NAT allows multiple devices in a private network to share one public IP for Internet access.

---

## 76. What does "show interfaces" command display on a Cisco device?

a) Routing table  
b) VLAN configuration  
c) Status and statistics of all interfaces  
d) MAC address table  

**Answer:** c) Status and statistics of all interfaces  
**Explanation:**  
- It shows operational status, errors, speed, and more for each interface.

---

## 77. Which protocol is used for network device management and monitoring?

a) SNMP  
b) SMTP  
c) FTP  
d) HTTP  

**Answer:** a) SNMP  
**Explanation:**  
- SNMP (Simple Network Management Protocol) is used to monitor and manage network devices.

---

## 78. What is the main security risk of using Telnet instead of SSH?

a) Telnet uses too much bandwidth  
b) Telnet is slow  
c) Telnet sends data in plain text, making it easy to intercept  
d) Telnet only works with wireless devices  

**Answer:** c) Telnet sends data in plain text, making it easy to intercept  
**Explanation:**  
- SSH encrypts all traffic, making it much more secure than Telnet.

---

## 79. What is the purpose of a "trunk port" on a switch?

a) Connect a switch to a router  
b) Carry traffic for multiple VLANs between switches  
c) Assign IP addresses  
d) Increase port speed  

**Answer:** b) Carry traffic for multiple VLANs between switches  
**Explanation:**  
- Trunk ports use tagging (like 802.1Q) to carry traffic for multiple VLANs.

---

## 80. What command would you use to see a list of all directly connected Cisco devices?

a) show vlan  
b) show cdp neighbor  
c) show ip route  
d) show running-config  

**Answer:** b) show cdp neighbor  
**Explanation:**  
- The "show cdp neighbor" command uses Cisco Discovery Protocol to list directly connected Cisco devices.

---
