# Networking MCQ Practice (1–60) – Newbie to Intermediate, With Answers & Explanations

---

## 1. Which cable type is most commonly used for short-distance, high-speed LAN connections?
a) Coaxial  
b) Twisted Pair (UTP)  
c) Single-mode Fiber  
d) Multimode Fiber  
**Answer:** b) Twisted Pair (UTP)  
**Explanation:** Used for Ethernet LANs up to 100 meters due to cost and flexibility.

---

## 2. What cable type is best for long-distance, high-bandwidth backbone links?
a) Twisted Pair  
b) Coaxial  
c) Single-mode Fiber  
d) Multimode Fiber  
**Answer:** c) Single-mode Fiber  
**Explanation:** Supports tens of kilometers due to low signal loss.

---

## 3. Which device operates at Layer 3 of OSI and can route packets between different networks?
a) Switch  
b) Hub  
c) Router  
d) Bridge  
**Answer:** c) Router  
**Explanation:** Routers operate at the Network Layer, enabling inter-network communication.

---

## 4. Which device creates multiple collision domains but one broadcast domain by default?
a) Hub  
b) Switch  
c) Router  
d) Bridge  
**Answer:** b) Switch  
**Explanation:** Switches separate collision domains per port, but all ports share a broadcast domain unless VLANs are used.

---

## 5. How many broadcast domains and collision domains in a network with 3 switches, each with 4 PCs connected (no VLANs)?
a) Broadcast=1, Collision=12  
b) Broadcast=3, Collision=12  
c) Broadcast=1, Collision=15  
d) Broadcast=3, Collision=15  
**Answer:** a) Broadcast=1, Collision=12  
**Explanation:** No VLANs means one broadcast domain; each PC connection is a collision domain.

---

## 6. What separates broadcast domains?
a) Switch  
b) Hub  
c) Router  
d) Bridge  
**Answer:** c) Router  
**Explanation:** Routers separate broadcast domains; switches separate collision domains.

---

## 7. What is the order of DHCP messages in the DORA process?
a) Discover, Offer, Request, Acknowledge  
b) Discover, Request, Offer, Acknowledge  
c) Request, Offer, Discover, Acknowledge  
d) Offer, Discover, Request, Acknowledge  
**Answer:** a) Discover, Offer, Request, Acknowledge  
**Explanation:** DORA stands for Discover, Offer, Request, and Acknowledge.

---

## 8. Which OSI layer is responsible for reliable communication and flow control?
a) Network  
b) Transport  
c) Data Link  
d) Physical  
**Answer:** b) Transport  
**Explanation:** Layer 4 (Transport) handles reliability (TCP) and flow control.

---

## 9. Which TCP/IP layer corresponds to OSI's Network layer?
a) Application  
b) Internet  
c) Transport  
d) Link  
**Answer:** b) Internet  
**Explanation:** The Internet layer handles routing, similar to OSI's Network layer.

---

## 10. What happens when port security violation mode is set to "restrict"?
a) The port shuts down  
b) Violating frames are dropped and a log is generated  
c) All traffic is allowed  
d) Frames are dropped silently  
**Answer:** b) Violating frames are dropped and a log is generated  
**Explanation:** Restrict mode drops frames and logs the violation.

---

## 11. Which protocol is open-standard for EtherChannel negotiation?
a) PAgP  
b) LACP  
c) VTP  
d) HSRP  
**Answer:** b) LACP  
**Explanation:** LACP (IEEE 802.3ad) is open-standard; PAgP is Cisco proprietary.

---

## 12. What is the benefit of using EtherChannel?
a) Increased bandwidth and redundancy  
b) Improved wireless range  
c) Reduced collision domains  
d) Segmented broadcast domains  
**Answer:** a) Increased bandwidth and redundancy  
**Explanation:** Multiple links act as one, increasing bandwidth and providing failover.

---

## 13. What type of NAT allows many internal IPs to share a single public IP via port numbers?
a) Static NAT  
b) Dynamic NAT  
c) PAT  
d) 1-to-1 NAT  
**Answer:** c) PAT  
**Explanation:** PAT (Port Address Translation) multiplexes many private IPs over one public IP using port numbers.

---

## 14. What is required for static NAT?
a) A pool of public IPs  
b) One-to-one mapping  
c) Overloading  
d) Dynamic address assignment  
**Answer:** b) One-to-one mapping  
**Explanation:** Each private IP is mapped to a fixed public IP.

---

## 15. Which ACL can filter traffic based on source, destination IP, and port?
a) Standard ACL  
b) Extended ACL  
c) Numbered ACL  
d) Dynamic ACL  
**Answer:** b) Extended ACL  
**Explanation:** Extended ACLs match source/destination IP and protocol/port.

---

## 16. Where should an extended ACL be placed for best efficiency?
a) Closest to the source  
b) Closest to the destination  
c) On the router  
d) Anywhere  
**Answer:** a) Closest to the source  
**Explanation:** This blocks unwanted traffic early.

---

## 17. What command applies an ACL inbound on interface FastEthernet0/1?
a) ip access-group 101 out  
b) access-list 101 permit …  
c) ip access-group 101 in  
d) access-list 101 deny …  
**Answer:** c) ip access-group 101 in  
**Explanation:** This applies ACL 101 to inbound traffic.

---

## 18. Which command creates a standard ACL permitting traffic from 192.168.1.0/24?
a) access-list 10 permit 192.168.1.0 0.0.0.255  
b) access-list 110 permit ip any any  
c) access-list 10 deny 192.168.1.0 0.0.0.255  
d) access-list 110 permit tcp any any eq 80  
**Answer:** a) access-list 10 permit 192.168.1.0 0.0.0.255  
**Explanation:** Standard ACLs use numbers 1-99 and match source IPs.

---

## 19. What is the purpose of VTP?
a) Distributes VLAN information to switches  
b) Enables EtherChannel negotiation  
c) Assigns IP addresses  
d) Manages port security  
**Answer:** a) Distributes VLAN information to switches  
**Explanation:** VTP synchronizes VLAN configs across switches.

---

## 20. What does DTP do?
a) Dynamically negotiates trunk links  
b) Assigns VLAN membership  
c) Prevents switching loops  
d) Translates MAC addresses  
**Answer:** a) Dynamically negotiates trunk links  
**Explanation:** DTP negotiates trunking between switches.

---

## 21. What is the default VLAN on a Cisco switch?
a) VLAN 1  
b) VLAN 10  
c) VLAN 100  
d) VLAN 99  
**Answer:** a) VLAN 1  
**Explanation:** All ports are in VLAN 1 by default.

---

## 22. Which VLAN type carries untagged traffic on a trunk port?
a) Native VLAN  
b) Data VLAN  
c) Voice VLAN  
d) Management VLAN  
**Answer:** a) Native VLAN  
**Explanation:** Native VLAN is used for untagged frames on 802.1Q trunks.

---

## 23. Which routing protocol uses the highest IP address on active interfaces as router ID if not manually set?
a) OSPF  
b) EIGRP  
c) RIP  
d) BGP  
**Answer:** a) OSPF  
**Explanation:** OSPF chooses the highest loopback IP, or the highest active interface IP if none.

---

## 24. What is the function of a router ID in OSPF?
a) Identifies the router uniquely in the OSPF network  
b) Sets the administrative distance  
c) Assigns area numbers  
d) Determines the DR/BDR election  
**Answer:** a) Identifies the router uniquely in the OSPF network  
**Explanation:** The router ID is used for OSPF process identification.

---

## 25. Which switch becomes the root bridge in STP?
a) The switch with the lowest MAC address  
b) The switch with the lowest priority value  
c) The switch with the highest priority value  
d) The switch with the highest MAC address  
**Answer:** b) The switch with the lowest priority value  
**Explanation:** Priority is checked first; if equal, the lowest MAC address breaks the tie.

---

## 26. What is the role of a blocked port in STP?
a) Forwards all traffic  
b) Prevents switching loops  
c) Segments collision domains  
d) Assigns VLANs  
**Answer:** b) Prevents switching loops  
**Explanation:** Blocked ports stop traffic to prevent loops.

---

## 27. How does DHCP snooping protect a network?
a) Blocks DHCP responses from untrusted ports  
b) Encrypts DHCP messages  
c) Assigns static IP addresses  
d) Increases wireless security  
**Answer:** a) Blocks DHCP responses from untrusted ports  
**Explanation:** Only trusted ports can send DHCP offers, blocking rogue servers.

---

## 28. What does BPDU Guard do on a switch port?
a) Disables the port if BPDU is received  
b) Increases port speed  
c) Assigns VLANs  
d) Allows trunking  
**Answer:** a) Disables the port if BPDU is received  
**Explanation:** BPDU Guard shuts down ports where BPDUs should not be seen (like edge ports).

---

## 29. In the route entry "O 10.10.20.0/24 [110/2] via 10.10.10.2", what does "O" indicate?
a) Static route  
b) OSPF-learned route  
c) Directly connected  
d) RIP-learned route  
**Answer:** b) OSPF-learned route  
**Explanation:** "O" is for OSPF.

---

## 30. What command shows a router's routing table in Cisco IOS?
a) show ip route  
b) show running-config  
c) show interfaces  
d) show arp  
**Answer:** a) show ip route  
**Explanation:** Displays all learned routes.

---

## 31. Why is a default gateway necessary for hosts?
a) To reach other networks  
b) To resolve hostnames  
c) To communicate within the same subnet  
d) To increase wireless speed  
**Answer:** a) To reach other networks  
**Explanation:** The default gateway lets hosts send traffic outside their local subnet.

---

## 32. Which subnet mask supports exactly 14 usable hosts?
a) /26  
b) /28  
c) /29  
d) /30  
**Answer:** b) /28  
**Explanation:** /28 = 16 IPs, 14 usable (subtract network/broadcast).

---

## 33. Which protocol provides encrypted remote management?
a) Telnet  
b) SSH  
c) SNMP  
d) FTP  
**Answer:** b) SSH  
**Explanation:** SSH encrypts all management traffic.

---

## 34. What is the default port for Telnet?
a) 22  
b) 23  
c) 25  
d) 80  
**Answer:** b) 23  
**Explanation:** Telnet uses TCP port 23; SSH uses port 22.

---

## 35. Which protocol resolves IP addresses to MAC addresses?
a) ARP  
b) DNS  
c) DHCP  
d) ICMP  
**Answer:** a) ARP  
**Explanation:** ARP (Address Resolution Protocol) maps IP addresses to MAC addresses.

---

## 36. What is the default administrative distance of a static route in Cisco IOS?
a) 90  
b) 110  
c) 1  
d) 120  
**Answer:** c) 1  
**Explanation:** Static routes have an administrative distance of 1.

---

## 37. Which protocol uses port 53?
a) HTTP  
b) DNS  
c) SMTP  
d) SNMP  
**Answer:** b) DNS  
**Explanation:** DNS uses port 53 for queries.

---

## 38. Which IPv6 address type is link-local?
a) fe80::/10  
b) 2001::/16  
c) fc00::/7  
d) ff00::/8  
**Answer:** a) fe80::/10  
**Explanation:** fe80::/10 is reserved for link-local addresses.

---

## 39. Which command shows all VLANs on a Cisco switch?
a) show vlan  
b) show running-config  
c) show interfaces  
d) show mac-address-table  
**Answer:** a) show vlan  
**Explanation:** This displays all configured VLANs and their status.

---

## 40. Which device breaks up broadcast domains?
a) Switch  
b) Hub  
c) Router  
d) Bridge  
**Answer:** c) Router  
**Explanation:** Routers separate broadcast domains.

---

## 41. What is the function of STP (Spanning Tree Protocol)?
a) Prevents routing loops  
b) Prevents switching loops  
c) Segments broadcast domains  
d) Encrypts VLAN traffic  
**Answer:** b) Prevents switching loops  
**Explanation:** STP blocks redundant links to prevent loops.

---

## 42. How many usable hosts are available in a /29 subnet?
a) 8  
b) 6  
c) 4  
d) 2  
**Answer:** b) 6  
**Explanation:** /29 = 8 addresses, 6 usable (exclude network/broadcast).

---

## 43. Which protocol is used for secure remote device management?
a) Telnet  
b) SSH  
c) SNMP  
d) FTP  
**Answer:** b) SSH  
**Explanation:** SSH encrypts remote management sessions.

---

## 44. What is the main function of DHCP?
a) Assigns IP addresses dynamically  
b) Resolves host names  
c) Encrypts network traffic  
d) Prevents network loops  
**Answer:** a) Assigns IP addresses dynamically  
**Explanation:** DHCP automatically assigns IP addresses and configures host network settings.

---

## 45. What is the maximum number of VLANs supported on a Cisco switch (normal range)?
a) 64  
b) 256  
c) 1005  
d) 4096  
**Answer:** c) 1005  
**Explanation:** VLAN IDs 1–1005 are in the normal range; extended range is up to 4096.

---

## 46. Which protocol is used to synchronize time in a network?
a) NTP  
b) FTP  
c) HTTP  
d) SNMP  
**Answer:** a) NTP  
**Explanation:** NTP (Network Time Protocol) synchronizes clocks over the network.

---

## 47. What does “show mac-address-table” command display?
a) IP addresses  
b) ARP table  
c) MAC addresses learned on switch ports  
d) Routing table  
**Answer:** c) MAC addresses learned on switch ports  
**Explanation:** Shows which MACs are on which ports.

---

## 48. Which protocol is used for network troubleshooting by sending echo requests?
a) SNMP  
b) ICMP  
c) DHCP  
d) ARP  
**Answer:** b) ICMP  
**Explanation:** ICMP (ping) sends echo requests/replies.

---

## 49. Which protocol is used to transfer files securely over the network?
a) FTP  
b) SFTP  
c) TFTP  
d) SMTP  
**Answer:** b) SFTP  
**Explanation:** SFTP (SSH File Transfer Protocol) encrypts file transfers.

---

## 50. What is the purpose of the “enable secret” command in Cisco IOS?
a) Configures the console password  
b) Encrypts the privileged exec password  
c) Enables SNMP  
d) Sets the SSH password  
**Answer:** b) Encrypts the privileged exec password  
**Explanation:** Stores the enable password in encrypted form.

---

## 51. What does “show running-config” display?
a) Saved configuration  
b) Current active configuration  
c) VLAN database  
d) Routing table  
**Answer:** b) Current active configuration  
**Explanation:** Displays current settings in RAM.

---

## 52. Which protocol helps prevent IP address conflicts in a network?
a) DHCP  
b) ARP  
c) ICMP  
d) RIP  
**Answer:** a) DHCP  
**Explanation:** DHCP manages IP assignment to avoid conflicts.

---

## 53. Which command displays interface status on a Cisco device?
a) show version  
b) show interfaces  
c) show vlan  
d) show arp  
**Answer:** b) show interfaces  
**Explanation:** Shows physical/logical status and statistics.

---

## 54. What is the function of the “ip helper-address” command?
a) Assigns a static IP address  
b) Forwards DHCP requests to a server  
c) Enables OSPF  
d) Sets the default gateway  
**Answer:** b) Forwards DHCP requests to a server  
**Explanation:** Used for DHCP relay.

---

## 55. Which protocol is used to advertise VLAN information between switches?
a) DTP  
b) VTP  
c) STP  
d) CDP  
**Answer:** b) VTP  
**Explanation:** VTP advertises VLAN info.

---

## 56. What does the “show cdp neighbor” command provide?
a) Routing table  
b) Neighboring Cisco device info  
c) VLAN membership  
d) Interface statistics  
**Answer:** b) Neighboring Cisco device info  
**Explanation:** CDP (Cisco Discovery Protocol) lists neighbors.

---

## 57. How does STP select the root port on a switch?
a) Port with lowest MAC address  
b) Port with lowest path cost to root bridge  
c) Port with highest priority  
d) Port with lowest port number  
**Answer:** b) Port with lowest path cost to root bridge  
**Explanation:** The root port is closest to the root bridge.

---

## 58. Which command shows the ARP table on a Cisco device?
a) show arp  
b) show mac-address-table  
c) show ip route  
d) show interfaces  
**Answer:** a) show arp  
**Explanation:** Lists IP-to-MAC mappings.

---

## 59. Which protocol is used for automatic neighbor discovery in IPv6?
a) DHCPv6  
b) NDP  
c) ARP  
d) OSPFv3  
**Answer:** b) NDP  
**Explanation:** NDP (Neighbor Discovery Protocol) replaces ARP in IPv6.

---

## 60. What is a collision domain?
a) A group of hosts that share the same broadcast  
b) All devices on the same VLAN  
c) A segment where packet collisions could occur  
d) Network area separated by a router  
**Answer:** c) A segment where packet collisions could occur  
**Explanation:** Each switch port is a separate collision domain.

---
