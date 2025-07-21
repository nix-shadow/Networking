# Networking MCQ 1-25 with Detailed Explanations and Correct Answers

---

**1. Which of the following cables provides longest distance transmission?**  
a) Twisted Pair Cable  
b) Multimode Fiber Optic Cable  
c) Coaxial Cable  
d) Single mode Fiber Optic Cable  

**Correct Answer:** d) Single mode Fiber Optic Cable  
**Explanation:**  
- Twisted pair and coaxial cables are limited to short distances (few hundred meters).
- Multimode fiber can go further (up to 2 km), but single-mode fiber supports transmission up to 100 km or more due to less signal attenuation and dispersion.  
- Thus, single mode fiber is preferred for long-haul, high-speed links.

---

**2. Which device can reduce both collision and broadcast domains when configured correctly? (select all that apply)**  
a) Switch with vlans  
b) Router  
c) Hub  

**Correct Answer:** a) Switch with VLANs, b) Router  
**Explanation:**  
- Switches with VLANs segment the network into multiple broadcast domains (one per VLAN).
- Routers separate both collision domains (each interface) and broadcast domains (each routed subnet).
- Hubs do not segment either domain—they extend a single collision domain.

---

**3. Which TCP/IP layer is responsible for hop-to-hop communication and error checking?**  
a) Network Layer  
b) Transport Layer  
c) Application Layer  
d) Data Link Layer  

**Correct Answer:** d) Data Link Layer  
**Explanation:**  
- Data Link Layer is responsible for framing, addressing, and error checking on a single link between two devices (hop-to-hop).
- Network Layer handles end-to-end routing.
- Transport Layer manages end-to-end sessions and reliability.

---

**4. Which EtherChannel protocol is open standard?**  
a) LACP  
b) PAgP  
c) HSRP  
d) VTP  

**Correct Answer:** a) LACP  
**Explanation:**  
- LACP (Link Aggregation Control Protocol) is IEEE 802.3ad standard.
- PAgP is Cisco proprietary.
- HSRP and VTP are unrelated to EtherChannel.

---

**5. What action does Port Security take when the maximum number of MAC addresses is exceeded, and the violation mode is set to protect?**  
a) Shuts down the port permanently  
b) Drops violating frames and sends a log message  
c) Allows all traffic  
d) Drops violating frames silently  

**Correct Answer:** d) Drops violating frames silently  
**Explanation:**  
- "Protect" mode drops frames from unknown MAC addresses without notification or logging.
- "Restrict" logs and drops, "Shutdown" disables the port.

---

**6. Which type of NAT uses a pool of public IP addresses and assigns them to internal private IP addresses dynamically, as sessions are initiated?**  
a) Static NAT  
b) Dynamic NAT  
c) PAT (Port Address Translation)  
d) 1-to-1 NAT  

**Correct Answer:** b) Dynamic NAT  
**Explanation:**  
- Dynamic NAT uses a pool of public addresses and assigns them as needed.
- Static NAT and 1-to-1 NAT use fixed mappings.
- PAT uses a single public IP with many ports.

---

**7. Which VPN protocol is commonly used to provide site-to-site connectivity?**  
a) PPTP  
b) L2TP  
c) IPsec  
d) SSL VPN  

**Correct Answer:** c) IPsec  
**Explanation:**  
- IPsec is the industry standard for site-to-site VPNs, providing encryption and authentication.

---

**8. You need to create subnets for department with exactly 24 hosts. What CIDR prefix should you use?**  
a) /26  
b) /25  
c) /24  
d) /27  

**Correct Answer:** d) /27  
**Explanation:**  
- /27 subnet mask provides 32 IP addresses, 30 usable hosts (2 reserved for network/broadcast).
- /26 provides 62 hosts, which is more than needed.

---

**9. Why is configuring a default gateway important for a host device?**  
a) It enables the device to communicate with devices on other networks  
b) It allows the device to bypass the DNS server  
c) It improves the device’s wireless signal strength  
d) It restricts communication to only IPv6 addresses  

**Correct Answer:** a) It enables the device to communicate with devices on other networks  
**Explanation:**  
- The default gateway forwards traffic to other networks/subnets.

---

**10. Which DHCP message is broadcast by the client to locate available DHCP servers?**  
a) DHCP Discover  
b) DHCP Offer  
c) DHCP Request  
d) DHCP Acknowledgment  

**Correct Answer:** a) DHCP Discover  
**Explanation:**  
- The client sends a DHCP Discover to find servers.
- Servers reply with DHCP Offer.

---

**11. Which gateway will be the active gateway in HSRP?**  
a) router with priority: 100  
b) router with priority: 150  
c) router with priority: 200  
d) router with priority: 300  

**Correct Answer:** c) router with priority: 200  
**Explanation:**  
- HSRP priority ranges from 0-255. Highest valid priority wins. 300 is not valid.

---

**12. Which ACL type can filter traffic based on port numbers?**  
a) Standard ACL  
b) Extended ACL  
c) Numbered ACL  
d) Port-based ACL  

**Correct Answer:** b) Extended ACL  
**Explanation:**  
- Extended ACLs filter based on protocol, IP, and port numbers.
- Standard ACLs only filter source IP.

---

**13. How does DHCP Snooping protect a network?**  
a) It prevents unauthorized DHCP servers from assigning IP addresses  
b) It blocks all DHCP traffic from passing through switches  
c) It encrypts all DHCP communications  
d) It assigns IP addresses dynamically to all connected devices  

**Correct Answer:** a) It prevents unauthorized DHCP servers from assigning IP addresses  
**Explanation:**  
- DHCP Snooping allows only trusted ports to send DHCP offers, blocking rogue servers.

---

**14. Where should an extended ACL be placed for optimal performance and accuracy?**  
a) Closest to the destination  
b) Closest to the source  
c) Anywhere in the network  
d) On the default gateway  

**Correct Answer:** b) Closest to the source  
**Explanation:**  
- Placing ACLs near the source blocks unwanted traffic before it traverses the network.

---

**15. Which OSPF router type is responsible for redistributing routes from non-OSPF routing protocols into OSPF?**  
a) ABR  
b) DR  
c) ASBR  
d) Internal Router  

**Correct Answer:** c) ASBR  
**Explanation:**  
- Autonomous System Boundary Router redistributes external routes into OSPF.

---

**16. In which scenario would an OSPF router be both an ABR and an ASBR?**  
a) If it connects two stub areas  
b) If it connects multiple OSPF areas and redistributes external routes  
c) If it is elected as a DR  
d) If it only has one interface in Area 0  

**Correct Answer:** b) If it connects multiple OSPF areas and redistributes external routes  
**Explanation:**  
- If the router connects areas and also redistributes external routes, it functions as both ABR and ASBR.

---

**17. In VLAN configuration, what is required on a switch port to allow traffic from multiple VLANs?**  
a) Access port  
b) Trunk port  
c) Native VLAN port  
d) Loopback interface  

**Correct Answer:** b) Trunk port  
**Explanation:**  
- Trunk ports carry traffic for multiple VLANs using tagging protocols (802.1Q).

---

**18. How many broadcast domains and collision domains are there in the following diagram?**  
a) Collision domain=10, broadcast=5  
b) Collision domain=7, broadcast=3  
c) Collision domain=8, broadcast=3  
d) Collision domain=9, broadcast=3  

**Correct Answer:** c) Collision domain=8, broadcast=3  
**Explanation:**  
- Each switch port is a separate collision domain. Number of VLANs = broadcast domains.

---

**19. Refer the diagram and answer the questions: (subjective)**  
(Note: Fa0/3 belongs to the ports connected to the cross links.)
- a) Which switch will be the Root Bridge after the topology change?  
  **Answer:** The switch with the lowest Bridge ID (usually SW1).
- b) What is the status of port Fa0/3 on Switch 4 in the new topology?  
  **Answer:** Blocking state (STP blocks redundant links).
- c) Which switch has the lowest Bridge ID, and why is it significant in STP?  
  **Answer:** SW1; the lowest Bridge ID is elected as root bridge, centralizing STP calculations.
- d) What happens if a new switch with lower priority than the Root Bridge is added?  
  **Answer:** It becomes the new root bridge and triggers a recalculation of the topology.

---

**20. Refer to the exhibit. Which statement explains the configuration error message that is received?**  
a) It belongs to a private IP address range.   
b) The router does not support /28 mask.   
c) It is a network address.   
d) It is a broadcast address.  

**Correct Answer:** d) It is a broadcast address.  
**Explanation:**  
- Routers cannot assign broadcast addresses to interfaces.

---

**21. Refer to the exhibit. Which command provides this output?**  
a) show ip route   
b) show cdp neighbor   
c) show ip interface   
d) show interface  

**Correct Answer:** b) show cdp neighbor  
**Explanation:**  
- This command displays information about directly connected Cisco devices.

---

**22. Refer the diagram and answer the questions: (subjective)**  
- a) PC2 is unable to access google.com (1.1.1.1). What could be the reason?  
  **Answer:** PC2 is not permitted by the NAT access-list, so traffic is not translated and dropped.
- b) How does Access-List 100 control traffic, and what is its role in NAT?  
  **Answer:** It defines which internal IPs are eligible for NAT translation.
- c) What would happen if Access-List 101 were removed from the configuration?  
  **Answer:** Devices matching that access-list lose NAT and Internet access.

---

**23. Refer to the exhibit. What does the router use as its OSPF router-ID?**  
a) 10.10.1.10   
b) 10.10.10.20   
c) 172.16.15.10   
d) 192.168.0.1  

**Correct Answer:** c) 172.16.15.10  
**Explanation:**  
- OSPF chooses the highest loopback IP or highest active interface IP.

---

**24. Refer to the exhibit. How does SW2 interact with other switches in this VTP domain?**  
a) It transmits and processes VTP updates from any VTP clients on the network on its trunk ports.   
b) It processes VTP updates from any VTP clients on the network on its access ports.   
c) It receives updates from all VTP servers and forwards all locally configured VLANs out all trunk ports.  
d) It forwards only the VTP advertisements that it receives on its trunk ports.  

**Correct Answer:** d) It forwards only the VTP advertisements that it receives on its trunk ports.  
**Explanation:**  
- VTP transparent mode forwards VTP advertisements but does not process them.

---

**25. Refer to the exhibit. If OSPF is running on this network, how does Router2 handle traffic from Site B to 10.10.13.128/25 at Site A?**  
a) It sends packets out of interface Fa0/1 only.   
b) It sends packets out of interface Fa0/2 only.   
c) It load-balances traffic out of Fa0/1 and Fa0/2.   
d) It cannot send packets to 10.10.13.128/25.  

**Correct Answer:** d) It cannot send packets to 10.10.13.128/25.  
**Explanation:**  
- There is no matching route for this subnet in the routing table.

---

# Additional Objective Practice Questions

---

**26. Which protocol resolves IP addresses to MAC addresses?**  
a) ARP  
b) DNS  
c) DHCP  
d) ICMP  
**Answer:** a) ARP  
**Explanation:** ARP (Address Resolution Protocol) maps IP addresses to MAC addresses.

---

**27. What is the default administrative distance of a static route in Cisco IOS?**  
a) 90  
b) 110  
c) 1  
d) 120  
**Answer:** c) 1  
**Explanation:** Static routes have an administrative distance of 1.

---

**28. Which protocol uses port 53?**  
a) HTTP  
b) DNS  
c) SMTP  
d) SNMP  
**Answer:** b) DNS  
**Explanation:** DNS uses port 53 for queries.

---

**29. Which IPv6 address type is link-local?**  
a) fe80::/10  
b) 2001::/16  
c) fc00::/7  
d) ff00::/8  
**Answer:** a) fe80::/10  
**Explanation:** fe80::/10 is reserved for link-local addresses.

---

**30. Which command shows all VLANs on a Cisco switch?**  
a) show vlan  
b) show running-config  
c) show interfaces  
d) show mac-address-table  
**Answer:** a) show vlan  
**Explanation:** This displays all configured VLANs and their status.

---

**31. Which device breaks up broadcast domains?**  
a) Switch  
b) Hub  
c) Router  
d) Bridge  
**Answer:** c) Router  
**Explanation:** Routers separate broadcast domains.

---

**32. What is the function of STP (Spanning Tree Protocol)?**  
a) Prevents routing loops  
b) Prevents switching loops  
c) Segments broadcast domains  
d) Encrypts VLAN traffic  
**Answer:** b) Prevents switching loops  
**Explanation:** STP blocks redundant links to prevent loops.

---

**33. How many usable hosts are available in a /29 subnet?**  
a) 8  
b) 6  
c) 4  
d) 2  
**Answer:** b) 6  
**Explanation:** /29 = 8 addresses, 6 usable (exclude network/broadcast).

---

**34. Which protocol is used for secure remote device management?**  
a) Telnet  
b) SSH  
c) SNMP  
d) FTP  
**Answer:** b) SSH  
**Explanation:** SSH encrypts remote management sessions.

---

**35. What is the main function of DHCP?**  
a) Assigns IP addresses dynamically  
b) Resolves host names  
c) Encrypts network traffic  
d) Prevents network loops  
**Answer:** a) Assigns IP addresses dynamically  
**Explanation:** DHCP automatically assigns IP addresses and configures host network settings.

---

*Use these detailed explanations to understand not just the correct answer, but why it's correct—and why the other options are incorrect!*