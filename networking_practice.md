# Networking Practice Questions (Newbie to Intermediate) – With Detailed Explanations

---

## 1. Cable Types

**Q1:** Which cable type is most commonly used for short-distance, high-speed LAN connections?  
a) Coaxial  
b) Twisted Pair (UTP)  
c) Single-mode Fiber  
d) Multimode Fiber  

**Answer:** b) Twisted Pair (UTP)  
**Explanation:** UTP is inexpensive and used for Ethernet cabling in most LANs up to 100 meters.

**Q2:** What cable type is best for long-distance, high-bandwidth backbone links?  
a) Twisted Pair  
b) Coaxial  
c) Single-mode Fiber  
d) Multimode Fiber  

**Answer:** c) Single-mode Fiber  
**Explanation:** Single-mode fiber supports longer distances (tens of kilometers) due to less signal loss.

---

## 2. Router vs Switch Differences

**Q3:** Which device operates at Layer 3 and can route packets between different networks?  
a) Switch  
b) Hub  
c) Router  
d) Bridge  

**Answer:** c) Router  
**Explanation:** Routers operate at the network layer and route traffic between networks.

**Q4:** Which device creates multiple collision domains but a single broadcast domain by default?  
a) Hub  
b) Switch  
c) Router  
d) Bridge  

**Answer:** b) Switch  
**Explanation:** Each port is a separate collision domain; the whole switch is a single broadcast domain unless VLANs are used.

---

## 3. Broadcast & Collision Domains

**Q5:** How many broadcast domains and collision domains in a network with 3 switches, each with 4 PCs connected (no VLANs, switches interconnected)?  
a) Broadcast=1, Collision=12  
b) Broadcast=3, Collision=12  
c) Broadcast=1, Collision=15  
d) Broadcast=3, Collision=15  

**Answer:** a) Broadcast=1, Collision=12  
**Explanation:** Without VLANs, all switches are in one broadcast domain. Each PC connection is a collision domain.

**Q6:** What separates broadcast domains?  
a) Switch  
b) Hub  
c) Router  
d) Bridge  

**Answer:** c) Router  
**Explanation:** Routers separate broadcast domains.

---

## 4. DORA Process (DHCP)

**Q7:** What is the correct order of DHCP messages in the DORA process?  
a) Discover, Offer, Request, Acknowledge  
b) Discover, Request, Offer, Acknowledge  
c) Request, Offer, Discover, Acknowledge  
d) Offer, Discover, Request, Acknowledge  

**Answer:** a) Discover, Offer, Request, Acknowledge  
**Explanation:** DORA = Discover (client), Offer (server), Request (client), Acknowledge (server).

---

## 5. TCP/IP & OSI Models

**Q8:** Which layer of the OSI model is responsible for reliable communication and flow control?  
a) Network  
b) Transport  
c) Data Link  
d) Physical  

**Answer:** b) Transport  
**Explanation:** Transport layer (Layer 4) handles reliability, flow control (TCP/UDP).

**Q9:** Which TCP/IP layer corresponds to OSI's Network layer?  
a) Application  
b) Internet  
c) Transport  
d) Link  

**Answer:** b) Internet  
**Explanation:** The Internet layer handles routing, equivalent to OSI's Network layer.

---

## 6. Port Security

**Q10:** What happens when port security violation mode is set to "restrict"?  
a) The port shuts down  
b) Violating frames are dropped and a log is generated  
c) All traffic is allowed  
d) Frames are dropped silently  

**Answer:** b) Violating frames are dropped and a log is generated  
**Explanation:** "Restrict" mode drops frames and logs violations.

---

## 7. EtherChannel & Types

**Q11:** Which protocol is open-standard for EtherChannel negotiation?  
a) PAgP  
b) LACP  
c) VTP  
d) HSRP  

**Answer:** b) LACP  
**Explanation:** LACP (IEEE 802.3ad) is open-standard; PAgP is Cisco proprietary.

**Q12:** What is the benefit of using EtherChannel?  
a) Increased bandwidth and redundancy  
b) Improved wireless range  
c) Reduced collision domains  
d) Segmented broadcast domains  

**Answer:** a) Increased bandwidth and redundancy  
**Explanation:** Multiple links act as one, increasing throughput and providing failover.

---

## 8. NAT & Types

**Q13:** What type of NAT allows many internal IPs to share a single public IP via port numbers?  
a) Static NAT  
b) Dynamic NAT  
c) PAT  
d) 1-to-1 NAT  

**Answer:** c) PAT  
**Explanation:** PAT (Port Address Translation) uses port numbers to multiplex many private IPs over one public IP.

**Q14:** What is required for static NAT?  
a) A pool of public IPs  
b) One-to-one mapping  
c) Overloading  
d) Dynamic address assignment  

**Answer:** b) One-to-one mapping  
**Explanation:** Static NAT maps each private IP to a fixed public IP.

---

## 9. ACL & Types

**Q15:** Which ACL can filter traffic based on source, destination IP, and port?  
a) Standard ACL  
b) Extended ACL  
c) Numbered ACL  
d) Dynamic ACL  

**Answer:** b) Extended ACL  
**Explanation:** Extended ACLs match source/destination IP and protocol/port.

**Q16:** Where is it best to place an extended ACL?  
a) Closest to the source  
b) Closest to the destination  
c) On the router  
d) Anywhere  

**Answer:** a) Closest to the source  
**Explanation:** This blocks unwanted traffic early.

---

## 10. ACL Configuration

**Q17:** What command applies an ACL inbound on interface FastEthernet0/1?  
a) ip access-group 101 out  
b) access-list 101 permit …  
c) ip access-group 101 in  
d) access-list 101 deny …  

**Answer:** c) ip access-group 101 in  
**Explanation:** This applies ACL 101 to inbound traffic.

**Q18:** Which command creates a standard ACL permitting traffic from 192.168.1.0/24?  
a) access-list 10 permit 192.168.1.0 0.0.0.255  
b) access-list 110 permit ip any any  
c) access-list 10 deny 192.168.1.0 0.0.0.255  
d) access-list 110 permit tcp any any eq 80  

**Answer:** a) access-list 10 permit 192.168.1.0 0.0.0.255  
**Explanation:** Standard ACLs use numbers 1-99 and match source IPs.

---

## 11. VTP & DTP

**Q19:** What is the purpose of VTP?  
a) Distributes VLAN information to switches  
b) Enables EtherChannel negotiation  
c) Assigns IP addresses  
d) Manages port security  

**Answer:** a) Distributes VLAN information to switches  
**Explanation:** VTP synchronizes VLAN configs across switches in a domain.

**Q20:** What does DTP do?  
a) Dynamically negotiates trunk links  
b) Assigns VLAN membership  
c) Prevents switching loops  
d) Translates MAC addresses  

**Answer:** a) Dynamically negotiates trunk links  
**Explanation:** DTP (Dynamic Trunking Protocol) negotiates trunking between switches.

---

## 12. VLANs & Types

**Q21:** What is the default VLAN on a Cisco switch?  
a) VLAN 1  
b) VLAN 10  
c) VLAN 100  
d) VLAN 99  

**Answer:** a) VLAN 1  
**Explanation:** All ports are in VLAN 1 by default.

**Q22:** Which VLAN type carries untagged traffic on a trunk port?  
a) Native VLAN  
b) Data VLAN  
c) Voice VLAN  
d) Management VLAN  

**Answer:** a) Native VLAN  
**Explanation:** Native VLAN is used for untagged frames on 802.1Q trunks.

---

## 13. Routing Protocols & Router ID

**Q23:** Which routing protocol uses the highest IP address on active interfaces as router ID if not manually set?  
a) OSPF  
b) EIGRP  
c) RIP  
d) BGP  

**Answer:** a) OSPF  
**Explanation:** OSPF chooses the highest loopback IP, or the highest active interface IP if none.

**Q24:** What is the function of a router ID in OSPF?  
a) Identifies the router uniquely in the OSPF network  
b) Sets the administrative distance  
c) Assigns area numbers  
d) Determines the DR/BDR election  

**Answer:** a) Identifies the router uniquely in the OSPF network  
**Explanation:** The router ID is used for OSPF process identification.

---

## 14. Spanning Tree Protocol (STP)

**Q25:** Which switch becomes the root bridge in STP?  
a) The switch with the lowest MAC address  
b) The switch with the lowest priority value  
c) The switch with the highest priority value  
d) The switch with the highest MAC address  

**Answer:** b) The switch with the lowest priority value  
**Explanation:** Priority is checked first; if equal, the lowest MAC address breaks the tie.

**Q26:** What is the role of a blocked port in STP?  
a) Forwards all traffic  
b) Prevents switching loops  
c) Segments collision domains  
d) Assigns VLANs  

**Answer:** b) Prevents switching loops  
**Explanation:** Blocked ports stop traffic to prevent loops.

---

## 15. DHCP Spoofing

**Q27:** How does DHCP snooping protect a network?  
a) Blocks DHCP responses from untrusted ports  
b) Encrypts DHCP messages  
c) Assigns static IP addresses  
d) Increases wireless security  

**Answer:** a) Blocks DHCP responses from untrusted ports  
**Explanation:** Only trusted ports can send DHCP offers, blocking rogue servers.

---

## 16. BPDU Guard

**Q28:** What does BPDU Guard do on a switch port?  
a) Disables the port if BPDU is received  
b) Increases port speed  
c) Assigns VLANs  
d) Allows trunking  

**Answer:** a) Disables the port if BPDU is received  
**Explanation:** BPDU Guard shuts down ports where BPDUs should not be seen (like edge ports).

---

## 17. Reading Routing Tables

**Q29:** In the route entry "O 10.10.20.0/24 [110/2] via 10.10.10.2", what does "O" indicate?  
a) Static route  
b) OSPF-learned route  
c) Directly connected  
d) RIP-learned route  

**Answer:** b) OSPF-learned route  
**Explanation:** "O" is for OSPF.

**Q30:** What command shows a router's routing table in Cisco IOS?  
a) show ip route  
b) show running-config  
c) show interfaces  
d) show arp  

**Answer:** a) show ip route  
**Explanation:** Displays all learned routes.

---

## 18. Default Gateway

**Q31:** Why is a default gateway necessary for hosts?  
a) To reach other networks  
b) To resolve hostnames  
c) To communicate within the same subnet  
d) To increase wireless speed  

**Answer:** a) To reach other networks  
**Explanation:** The default gateway lets hosts send traffic outside their local subnet.

---

## 19. Calculating CIDR

**Q32:** Which subnet mask supports exactly 14 usable hosts?  
a) /26  
b) /28  
c) /29  
d) /30  

**Answer:** b) /28  
**Explanation:** /28 = 16 IPs, 14 usable (subtract network/broadcast).

---

## 20. Telnet & SSH

**Q33:** Which protocol provides encrypted remote management?  
a) Telnet  
b) SSH  
c) SNMP  
d) FTP  

**Answer:** b) SSH  
**Explanation:** SSH encrypts all management traffic.

**Q34:** What is the default port for Telnet?  
a) 22  
b) 23  
c) 25  
d) 80  

**Answer:** b) 23  
**Explanation:** Telnet uses TCP port 23; SSH uses port 22.

---

# STP Calculation Practice (Broadcast & Collision Domains)

**Q35:** Given a network:  
- 2 switches, each with 5 PCs.  
- Switches connected together.  
- No VLANs.

a) How many broadcast domains?  
b) How many collision domains?  
c) If you add a router between switches, what happens?

**Answers:**  
a) 1 broadcast domain (all are in the same VLAN by default)  
b) 10 collision domains (each PC connection is a collision domain)  
c) The router separates the network into 2 broadcast domains

---

*Use these questions for solid foundational practice and to bridge to intermediate topics!*