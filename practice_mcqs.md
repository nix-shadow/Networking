# Newbie to Intermediate Networking MCQs - Complete Study Guide
## 20 Essential Topics for Network Fundamentals

---

## 1. CABLE TYPES

**1. Which cable type is best for connecting devices that are 500 meters apart?**  
a) Cat5e UTP  
b) Cat6 UTP  
c) Single-mode Fiber  
d) Coaxial Cable  

**Answer:** c) Single-mode Fiber  
**Explanation:**  
- **UTP cables** limited to ~100 meters
- **Coaxial** good for ~500m but limited bandwidth
- **Single-mode fiber** can go 10km+ with high bandwidth
- **Multimode fiber** limited to ~2km

---

**2. What type of cable do you use to connect two switches directly?**  
a) Straight-through cable  
b) Crossover cable  
c) Rollover cable  
d) Fiber optic cable  

**Answer:** b) Crossover cable (Traditional) or a) Straight-through (Modern)  
**Explanation:**  
- **Traditional:** Like devices need crossover (switch-to-switch, PC-to-PC)
- **Modern switches** have Auto-MDIX feature - auto-detect and adjust
- **Straight-through:** Different devices (PC to switch)
- **Rollover:** Console connections only

---

**3. Which pins are swapped in a crossover cable?**  
a) 1&2 with 3&6  
b) 1&3 with 2&6  
c) 1&8 with 2&7  
d) 4&5 with 7&8  

**Answer:** a) 1&2 with 3&6  
**Explanation:**  
- Pin 1 (TX+) ↔ Pin 3 (RX+)
- Pin 2 (TX-) ↔ Pin 6 (RX-)
- This allows transmit from one device to reach receive on the other

---

## 2. ROUTER VS SWITCH DIFFERENCES

**4. What is the main function of a router?**  
a) Forward frames within a network  
b) Route packets between different networks  
c) Amplify electrical signals  
d) Convert protocols  

**Answer:** b) Route packets between different networks  
**Explanation:**  
- **Router:** Works at Layer 3, connects different networks/subnets
- **Switch:** Works at Layer 2, forwards frames within same network
- Routers use IP addresses, switches use MAC addresses

---

**5. Which device makes decisions based on MAC addresses?**  
a) Router  
b) Switch  
c) Hub  
d) Firewall  

**Answer:** b) Switch  
**Explanation:**  
- **Switch:** Maintains MAC address table, forwards based on destination MAC
- **Router:** Uses IP addresses and routing table
- **Hub:** No decision making - broadcasts everything

---

**6. A switch receives a frame with an unknown destination MAC address. What does it do?**  
a) Drops the frame  
b) Sends error message  
c) Floods the frame out all ports (except source)  
d) Forwards to default gateway  

**Answer:** c) Floods the frame out all ports (except source)  
**Explanation:**  
- Unknown unicast = **flood**
- Switch learns MAC addresses from source addresses of incoming frames
- Once learned, switch forwards directly to correct port

---

## 3. BROADCAST & COLLISION DOMAINS

**7. Count the broadcast domains in this network:**
```
[PC1]--[Switch1]--[Router]--[Switch2]--[PC2]
         |                     |
       [PC3]                 [PC4]
```
a) 1 broadcast domain  
b) 2 broadcast domains  
c) 3 broadcast domains  
d) 4 broadcast domains  

**Answer:** b) 2 broadcast domains  
**Explanation:**  
- **Router separates broadcast domains**
- Left side: PC1, PC3, Switch1 = Domain 1
- Right side: PC2, PC4, Switch2 = Domain 2

---

**8. Count collision domains in the same network above:**  
a) 4 collision domains  
b) 5 collision domains  
c) 6 collision domains  
d) 2 collision domains  

**Answer:** b) 5 collision domains  
**Explanation:**  
- **Each switch port = separate collision domain**
- SW1-PC1=1, SW1-PC3=1, SW1-Router=1, Router-SW2=1, SW2-PC2=1, SW2-PC4=1
- Wait: SW1-Router + Router-SW2 = 2 separate domains
- Total: **5 collision domains**

---

**9. What happens when you replace a hub with a switch?**  
a) Collision domains decrease, broadcast domains stay same  
b) Collision domains increase, broadcast domains stay same  
c) Both increase  
d) Both decrease  

**Answer:** b) Collision domains increase, broadcast domains stay same  
**Explanation:**  
- **Hub:** 1 large collision domain for all connected devices
- **Switch:** Each port = separate collision domain
- **Broadcast domains:** No change (both forward broadcasts)

---

## 4. DORA PROCESS (DHCP)

**10. What does DORA stand for in DHCP?**  
a) Discover, Offer, Request, Acknowledge  
b) Distribute, Obtain, Receive, Accept  
c) Detect, Open, Route, Allow  
d) Dynamic, Obtain, Resolve, Assign  

**Answer:** a) Discover, Offer, Request, Acknowledge  
**Explanation:**  
1. **Discover:** Client broadcasts to find DHCP servers
2. **Offer:** Server offers IP configuration
3. **Request:** Client requests specific offer
4. **Acknowledge:** Server confirms assignment

---

**11. Which DHCP message is sent as a broadcast by the client?**  
a) DHCP Offer  
b) DHCP Request  
c) DHCP Discover  
d) Both b and c  

**Answer:** d) Both b and c  
**Explanation:**  
- **DHCP Discover:** Always broadcast (client has no IP)
- **DHCP Request:** Broadcast (so other servers know client's choice)
- **DHCP Offer:** Unicast (server knows client MAC)
- **DHCP ACK:** Usually unicast

---

**12. A client receives multiple DHCP offers. What happens next?**  
a) Accepts the first offer received  
b) Accepts all offers  
c) Sends DHCP Request for chosen offer  
d) Ignores all offers  

**Answer:** c) Sends DHCP Request for chosen offer  
**Explanation:**  
- Client typically chooses **first offer received**
- Sends **broadcast DHCP Request** naming the chosen server
- Other servers see the request and withdraw their offers

---

## 5. TCP/IP & OSI MODELS

**13. Which layer of the OSI model does a router operate at?**  
a) Layer 1 (Physical)  
b) Layer 2 (Data Link)  
c) Layer 3 (Network)  
d) Layer 4 (Transport)  

**Answer:** c) Layer 3 (Network)  
**Explanation:**  
- **Layer 1:** Physical signals (cables, electrical)
- **Layer 2:** Frames, MAC addresses (switches)
- **Layer 3:** Packets, IP addresses (routers)
- **Layer 4:** Segments, port numbers (TCP/UDP)

---

**14. Which protocol operates at the Transport layer?**  
a) IP  
b) ARP  
c) TCP  
d) HTTP  

**Answer:** c) TCP  
**Explanation:**  
- **Transport Layer (4):** TCP, UDP
- **Network Layer (3):** IP, ICMP, ARP
- **Application Layer (7):** HTTP, FTP, SMTP

---

**15. What is the correct order of data encapsulation?**  
a) Data → Segment → Packet → Frame → Bits  
b) Data → Packet → Segment → Frame → Bits  
c) Data → Frame → Packet → Segment → Bits  
d) Bits → Frame → Packet → Segment → Data  

**Answer:** a) Data → Segment → Packet → Frame → Bits  
**Explanation:**  
- **Application:** Data
- **Transport:** Segment (add TCP/UDP header)
- **Network:** Packet (add IP header)
- **Data Link:** Frame (add Ethernet header/trailer)
- **Physical:** Bits (electrical signals)

---

## 6. PORT SECURITY

**16. What happens when port security violation mode is set to "shutdown"?**  
a) Port continues working but logs violation  
b) Port drops violating frames silently  
c) Port goes to err-disabled state  
d) Port resets automatically  

**Answer:** c) Port goes to err-disabled state  
**Explanation:**  
- **Shutdown:** Port disabled, requires manual re-enablement
- **Restrict:** Drops frames + logs + SNMP traps
- **Protect:** Drops frames silently

---

**17. Which command shows the MAC addresses learned on a port?**  
a) show port-security  
b) show mac-address-table  
c) show interfaces  
d) show running-config  

**Answer:** b) show mac-address-table  
**Explanation:**  
- **show mac-address-table:** All learned MACs
- **show port-security:** Port security status
- **show port-security address:** Secure MAC addresses

---

**18. What is the default maximum MAC addresses for port security?**  
a) 1  
b) 2  
c) 8  
d) 132  

**Answer:** a) 1  
**Explanation:**  
- Default maximum is **1 MAC address** per port
- Can be configured up to 132 (or higher on some switches)
- Typically used on access ports connected to single PC

---

## 7. ETHERCHANNEL & TYPES

**19. Which EtherChannel protocol is Cisco proprietary?**  
a) LACP  
b) PAgP  
c) UDLD  
d) DTP  

**Answer:** b) PAgP  
**Explanation:**  
- **PAgP:** Port Aggregation Protocol (Cisco proprietary)
- **LACP:** Link Aggregation Control Protocol (IEEE 802.3ad standard)
- Both create logical bundles of physical links

---

**20. What are the LACP modes? (Choose all correct)**  
a) Active  
b) Passive  
c) Auto  
d) Desirable  

**Answer:** a) Active, b) Passive  
**Explanation:**  
- **LACP modes:** Active (initiates), Passive (waits)
- **PAgP modes:** Auto (waits), Desirable (initiates)
- **At least one side must be "initiating" for EtherChannel to form**

---

**21. How many physical links can be bundled in an EtherChannel?**  
a) Up to 4  
b) Up to 8  
c) Up to 16  
d) No limit  

**Answer:** b) Up to 8  
**Explanation:**  
- **Maximum 8 active links** in EtherChannel
- Additional links can be standby
- All links must have same speed, duplex, VLAN config

---

## 8. NAT & TYPES

**22. What does PAT stand for?**  
a) Port Address Translation  
b) Packet Address Translation  
c) Private Address Translation  
d) Protocol Address Translation  

**Answer:** a) Port Address Translation  
**Explanation:**  
- **PAT:** Many private IPs share one public IP using different ports
- Also called NAT Overload
- Most common NAT type for home/small business

---

**23. Which NAT type uses a one-to-one mapping?**  
a) Static NAT  
b) Dynamic NAT  
c) PAT  
d) NAT Overload  

**Answer:** a) Static NAT  
**Explanation:**  
- **Static NAT:** Fixed mapping (192.168.1.10 always becomes 203.0.113.10)
- **Dynamic NAT:** Pool of public IPs, assigned as needed
- **PAT:** Many-to-one with port numbers

---

**24. A company has 100 internal hosts but only 5 public IP addresses. Which NAT type should they use?**  
a) Static NAT  
b) Dynamic NAT  
c) PAT  
d) Cannot be done  

**Answer:** c) PAT  
**Explanation:**  
- **PAT allows many internal hosts to share few public IPs**
- Uses port numbers to distinguish connections
- 5 public IPs could support hundreds of internal hosts

---

## 9. ACL & TYPES

**25. What can Standard ACLs filter on?**  
a) Source IP only  
b) Source and destination IP  
c) Source IP and port numbers  
d) All of the above  

**Answer:** a) Source IP only  
**Explanation:**  
- **Standard ACLs (1-99, 1300-1999):** Source IP only
- **Extended ACLs (100-199, 2000-2699):** Source/dest IP, protocol, ports
- Standard ACLs are simpler but less flexible

---

**26. Where should you place a Standard ACL?**  
a) Close to the source  
b) Close to the destination  
c) Anywhere in the network  
d) Only on routers  

**Answer:** b) Close to the destination  
**Explanation:**  
- **Standard ACLs:** Near destination (limited filtering capability)
- **Extended ACLs:** Near source (can make specific decisions early)
- Prevents blocking legitimate traffic unnecessarily

---

**27. Which ACL number range is for Extended ACLs?**  
a) 1-99  
b) 100-199  
c) 800-899  
d) 1000-1099  

**Answer:** b) 100-199  
**Explanation:**  
- **Standard:** 1-99, 1300-1999
- **Extended:** 100-199, 2000-2699
- **Named ACLs** can be either standard or extended

---

## 10. ACL CONFIGURATION

**28. What does this ACL line do: `access-list 10 deny 192.168.1.0 0.0.0.255`**  
a) Denies host 192.168.1.0 only  
b) Denies entire 192.168.1.0/24 network  
c) Denies all traffic  
d) Syntax error  

**Answer:** b) Denies entire 192.168.1.0/24 network  
**Explanation:**  
- **Wildcard mask 0.0.0.255** = match first 3 octets, any host in last octet
- Blocks all hosts from 192.168.1.1 to 192.168.1.254
- Remember: 0 = must match, 255 = don't care

---

**29. Every ACL has an implicit rule at the end. What is it?**  
a) Permit any  
b) Deny any  
c) Log any  
d) No implicit rule  

**Answer:** b) Deny any  
**Explanation:**  
- **Implicit "deny any"** at end of every ACL
- If traffic doesn't match any explicit rule, it's denied
- Must add "permit any" if you want to allow other traffic

---

**30. How do you apply an ACL to an interface for incoming traffic?**  
a) `ip access-group 10 in`  
b) `access-list 10 in`  
c) `ip access-class 10 in`  
d) `access-group 10 inbound`  

**Answer:** a) `ip access-group 10 in`  
**Explanation:**  
- **`ip access-group <ACL> in`** for incoming traffic
- **`ip access-group <ACL> out`** for outgoing traffic
- Applied at interface configuration mode

---

## 11. VTP & DTP

**31. What does VTP stand for?**  
a) VLAN Trunking Protocol  
b) Virtual Terminal Protocol  
c) VLAN Transfer Protocol  
d) Virtual Trunk Protocol  

**Answer:** a) VLAN Trunking Protocol  
**Explanation:**  
- **VTP:** Synchronizes VLAN database across switches
- Modes: Server, Client, Transparent
- Reduces manual VLAN configuration

---

**32. Which VTP mode can create, modify, and delete VLANs?**  
a) Client  
b) Server  
c) Transparent  
d) All of the above  

**Answer:** b) Server  
**Explanation:**  
- **Server:** Can create/modify/delete VLANs, sends updates
- **Client:** Receives updates, cannot modify VLANs
- **Transparent:** Forwards updates but maintains own VLAN database

---

**33. What does DTP negotiate?**  
a) VLAN assignments  
b) Trunk/Access mode  
c) IP addresses  
d) Spanning tree  

**Answer:** b) Trunk/Access mode  
**Explanation:**  
- **DTP (Dynamic Trunking Protocol):** Auto-negotiates trunk formation
- Modes: Dynamic Auto, Dynamic Desirable, Trunk, Access
- Security best practice: Disable DTP on access ports

---

## 12. VLANS & TYPES

**34. What is the default VLAN on Cisco switches?**  
a) VLAN 0  
b) VLAN 1  
c) VLAN 1002  
d) VLAN 4094  

**Answer:** b) VLAN 1  
**Explanation:**  
- **VLAN 1:** Default VLAN, all ports start here
- **VLAN 1002-1005:** Default Token Ring/FDDI VLANs
- **VLAN 4094:** Maximum VLAN ID

---

**35. Which VLAN carries untagged traffic on a trunk?**  
a) Management VLAN  
b) Native VLAN  
c) Default VLAN  
d) Access VLAN  

**Answer:** b) Native VLAN  
**Explanation:**  
- **Native VLAN:** Traffic sent untagged on 802.1Q trunks
- Default native VLAN is VLAN 1
- Security practice: Change native VLAN to unused ID

---

**36. How many VLANs can you have on a single switch?**  
a) 64  
b) 256  
c) 1024  
d) 4094  

**Answer:** d) 4094  
**Explanation:**  
- **VLAN IDs:** 1-4094 (12-bit field)
- **Reserved:** 0, 4095
- **Ethernet VLANs:** 1-1005 (normal range), 1006-4094 (extended range)

---

## 13. ROUTING PROTOCOLS & ROUTER ID

**37. Which has the lowest administrative distance?**  
a) Static route  
b) EIGRP  
c) OSPF  
d) RIP  

**Answer:** a) Static route  
**Explanation:**  
- **Static route:** 1 (most trusted)
- **EIGRP:** 90
- **OSPF:** 110  
- **RIP:** 120 (least trusted)
- Lower = more trusted

---

**38. How is OSPF Router ID determined? (Choose the first preference)**  
a) Highest IP on any interface  
b) Manually configured router-id  
c) Lowest IP on any interface  
d) Highest loopback IP  

**Answer:** b) Manually configured router-id  
**Explanation:**  
**OSPF Router ID selection order:**
1. **Manually configured** `router-id` command
2. **Highest loopback IP** address
3. **Highest physical interface IP** address

---

**39. What metric does RIP use?**  
a) Bandwidth  
b) Delay  
c) Hop count  
d) Cost  

**Answer:** c) Hop count  
**Explanation:**  
- **RIP:** Hop count (max 15 hops)
- **OSPF:** Cost (based on bandwidth)
- **EIGRP:** Composite (bandwidth + delay by default)

---

## 14. SPANNING TREE PROTOCOL (STP)

**40. What problem does STP solve?**  
a) Routing loops  
b) Switching loops  
c) VLAN conflicts  
d) IP conflicts  

**Answer:** b) Switching loops  
**Explanation:**  
- **STP prevents switching loops** in redundant Layer 2 topologies
- Blocks redundant paths, maintains loop-free active topology
- Essential for network stability with multiple switches

---

**41. What determines the STP Root Bridge?**  
a) Highest priority  
b) Lowest priority  
c) Fastest switch  
d) Switch with most ports  

**Answer:** b) Lowest priority  
**Explanation:**  
- **Bridge Priority + MAC address = Bridge ID**
- **Lowest Bridge ID wins** root election
- Default priority: 32768 (then MAC as tiebreaker)

---

**42. Which STP port state forwards data traffic?**  
a) Blocking  
b) Listening  
c) Learning  
d) Forwarding  

**Answer:** d) Forwarding  
**Explanation:**  
- **Blocking:** No data, receives BPDUs only
- **Listening:** No data, processes BPDUs
- **Learning:** No data, learns MAC addresses
- **Forwarding:** Normal operation - forwards data

---

## 15. DHCP SPOOFING

**43. What is DHCP spoofing?**  
a) Stealing DHCP server credentials  
b) Rogue DHCP server providing false network info  
c) Blocking DHCP traffic  
d) Changing DHCP lease time  

**Answer:** b) Rogue DHCP server providing false network info  
**Explanation:**  
- **Attacker sets up fake DHCP server**
- Provides wrong gateway, DNS, or network settings
- Can redirect traffic through attacker's machine

---

**44. How does DHCP Snooping prevent DHCP spoofing?**  
a) Encrypts DHCP traffic  
b) Designates trusted/untrusted ports  
c) Blocks all DHCP traffic  
d) Changes DHCP server password  

**Answer:** b) Designates trusted/untrusted ports  
**Explanation:**  
- **Trusted ports:** Can send DHCP Offer/ACK (legitimate servers)
- **Untrusted ports:** Can only send DHCP Discover/Request (clients)
- Blocks rogue DHCP servers on untrusted ports

---

**45. Which DHCP message is blocked on untrusted ports with DHCP snooping?**  
a) DHCP Discover  
b) DHCP Request  
c) DHCP Offer  
d) All DHCP messages  

**Answer:** c) DHCP Offer  
**Explanation:**  
- **DHCP Offer/ACK:** Blocked on untrusted ports (only servers send these)
- **DHCP Discover/Request:** Allowed on untrusted ports (clients send these)
- Prevents rogue servers while allowing legitimate clients

---

## 16. BPDU GUARD

**46. What does BPDU Guard do?**  
a) Filters BPDU content  
b) Encrypts BPDUs  
c) Shuts down port receiving BPDUs  
d) Speeds up BPDU processing  

**Answer:** c) Shuts down port receiving BPDUs  
**Explanation:**  
- **BPDU Guard:** Protects PortFast ports
- If BPDU received on PortFast port → port goes err-disabled
- Prevents accidental connection of switches to access ports

---

**47. Where should you enable BPDU Guard?**  
a) Trunk ports  
b) Access ports with PortFast  
c) Root bridge ports  
d) All ports  

**Answer:** b) Access ports with PortFast  
**Explanation:**  
- **Access ports connecting end devices** (PCs, servers)
- PortFast + BPDU Guard = security best practice
- Prevents network topology disruption

---

**48. What command enables BPDU Guard globally on all PortFast ports?**  
a) `spanning-tree bpduguard enable`  
b) `spanning-tree portfast bpduguard default`  
c) `bpdu guard enable`  
d) `portfast bpduguard enable`  

**Answer:** b) `spanning-tree portfast bpduguard default`  
**Explanation:**  
- **Global command:** Applies to all PortFast-enabled ports
- **Interface command:** `spanning-tree bpduguard enable`
- Security best practice for access layer

---

## 17. READING ROUTING TABLES

**49. What does this routing table entry mean: `R 192.168.2.0/24 [120/2] via 10.0.0.1`**  
a) Static route with cost 2  
b) RIP route, 2 hops away  
c) OSPF route with cost 120  
d) Connected route  

**Answer:** b) RIP route, 2 hops away  
**Explanation:**  
- **R:** RIP protocol
- **[120/2]:** Administrative distance 120, metric 2 hops
- **via 10.0.0.1:** Next-hop IP address

---

**50. Which routing table entry will be used for destination 172.16.50.100?**
```
C 172.16.0.0/16 is directly connected, Fa0/0
S 172.16.50.0/24 via 192.168.1.1
D 172.16.0.0/8 [90/2172416] via 192.168.1.2
```
a) First entry (Connected)  
b) Second entry (Static)  
c) Third entry (EIGRP)  
d) Default route  

**Answer:** b) Second entry (Static)  
**Explanation:**  
- **Longest prefix match wins**
- /24 is more specific than /16 or /8
- Static route 172.16.50.0/24 matches destination exactly

---

**51. What does 'C' mean in a routing table?**  
a) Cost  
b) Connected  
c) Configured  
d) Calculated  

**Answer:** b) Connected  
**Explanation:**  
- **C:** Directly connected network
- **S:** Static route
- **D:** EIGRP
- **O:** OSPF
- **R:** RIP

---

## 18. DEFAULT GATEWAY

**52. Why do hosts need a default gateway?**  
a) To access the internet  
b) To communicate with devices on other networks  
c) To get IP addresses  
d) To resolve domain names  

**Answer:** b) To communicate with devices on other networks  
**Explanation:**  
- **Default gateway = router interface** on local network
- Forwards traffic destined for other networks/subnets
- Without it, host can only talk to same subnet

---

**53. A PC with IP 192.168.1.100/24 needs to reach 10.0.0.50. What does it do first?**  
a) Broadcasts ARP for 10.0.0.50  
b) Sends packet directly to 10.0.0.50  
c) Sends ARP for default gateway MAC  
d) Drops the packet  

**Answer:** c) Sends ARP for default gateway MAC  
**Explanation:**  
- **Different subnet** (192.168.1.x vs 10.0.0.x)
- PC knows it needs router, so ARPs for gateway MAC
- Then sends packet to gateway with destination IP 10.0.0.50

---

**54. What happens if a host has no default gateway configured?**  
a) It cannot communicate with any device  
b) It can only communicate within its own subnet  
c) It will automatically find one  
d) It will use broadcast for everything  

**Answer:** b) It can only communicate within its own subnet  
**Explanation:**  
- **Same subnet:** Direct communication works
- **Different subnet:** No way to reach router
- Host doesn't know where to send external traffic

---

## 19. CALCULATING CIDR

**55. How many hosts can a /26 network support?**  
a) 32 hosts  
b) 30 usable hosts  
c) 62 usable hosts  
d) 64 hosts  

**Answer:** c) 62 usable hosts  
**Explanation:**  
- **/26 = 6 host bits** (32-26=6)
- **2^6 = 64 total addresses**
- **64 - 2 = 62 usable** (subtract network & broadcast)

---

**56. What subnet mask corresponds to /28?**  
a) 255.255.255.224  
b) 255.255.255.240  
c) 255.255.255.248  
d) 255.255.255.252  

**Answer:** b) 255.255.255.240  
**Explanation:**  
- **/28 = 28 ones, 4 zeros**
- **Last octet:** 11110000 = 240
- **Full mask:** 255.255.255.240

---

**57. Which networks can be summarized as 192.168.0.0/22?**  
a) 192.168.0.0/24 through 192.168.3.0/24  
b) 192.168.0.0/24 and 192.168.1.0/24 only  
c) 192.168.0.0/24 through 192.168.255.0/24  
d) Only 192.168.0.0/24  

**Answer:** a) 192.168.0.0/24 through 192.168.3.0/24  
**Explanation:**  
- **/22 = 10 host bits** (32-22=10)
- **2^10 = 1024 addresses = 4 x /24 networks**
- Covers 192.168.0.0 to 192.168.3.255

---

## 20. TELNET & SSH

**58. What is the main difference between Telnet and SSH?**  
a) Telnet is faster  
b) SSH is encrypted, Telnet is not  
c) Telnet uses TCP, SSH uses UDP  
d) SSH is older protocol  

**Answer:** b) SSH is encrypted, Telnet is not  
**Explanation:**  
- **Telnet:** Clear text, port 23, insecure
- **SSH:** Encrypted, port 22, secure
- **Both use TCP**
- SSH is newer and recommended

---

**59. What port does SSH use by default?**  
a) 21  
b) 22  
c) 23  
d) 80  

**Answer:** b) 22  
**Explanation:**  
- **SSH:** Port 22
- **Telnet:** Port 23  
- **FTP:** Port 21
- **HTTP:** Port 80

---

**60. Which command enables SSH on a Cisco device?**  
a) `enable ssh`  
b) `ip ssh version 2`  
c) `transport input ssh`  
d) `crypto key generate rsa`  

**Answer:** d) `crypto key generate rsa`  
**Explanation:**  
**SSH Setup Steps:**
1. **`hostname` and `ip domain-name`** (for key generation)
2. **`crypto key generate rsa`** (creates encryption keys)
3. **`ip ssh version 2`** (optional, forces SSHv2)
4. **`line vty 0 4` → `transport input ssh`** (restrict to SSH only)

---

## QUICK REFERENCE TABLES

### Cable Types Quick Guide
| Distance | Cable Type | Max Speed |
|----------|------------|-----------|
| 0-100m | Cat5e/6 UTP | 1Gbps |
| 0-500m | Coaxial | 10Mbps-1Gbps |
| 0-2km | Multimode Fiber | 10Gbps+ |
| 0-40km+ | Single-mode Fiber | 