# Networking Test Questions - Question Bank

## 1. Cable Types

### Objective Questions:
1. Which cable type is commonly used for backbone connections in enterprise networks?
   a) Cat5e UTP  b) Cat6 UTP  c) Fiber optic  d) Coaxial

2. The maximum transmission distance for Cat6 UTP cable is:
   a) 100 meters  b) 185 meters  c) 500 meters  d) 2000 meters

3. Which connector is used with fiber optic cables?
   a) RJ45  b) BNC  c) SC/LC  d) DB9

### Subjective Questions:
1. Compare and contrast the advantages and disadvantages of fiber optic cables versus copper cables in enterprise networking.

2. Explain the difference between straight-through and crossover cables. When would you use each type?

## 2. Difference Between Router & Switch

### Objective Questions:
1. Routers operate at which OSI layer?
   a) Layer 1  b) Layer 2  c) Layer 3  d) Layer 4

2. Switches primarily use which address type for forwarding decisions?
   a) IP address  b) MAC address  c) Port number  d) VLAN ID

3. Which device can connect different network segments with different subnet addresses?
   a) Hub  b) Switch  c) Router  d) Repeater

### Subjective Questions:
1. Explain the fundamental differences between a router and a switch in terms of functionality, OSI layers, and network segmentation.

2. Describe scenarios where you would use a router versus a switch in network design.

## 3. Broadcast Domain & Collision Domain

### Objective Questions:
1. How many collision domains are created by a 24-port switch?
   a) 1  b) 12  c) 24  d) 48

2. How many broadcast domains are created by a router with 4 interfaces?
   a) 1  b) 2  c) 4  d) 8

3. Which device separates collision domains but not broadcast domains?
   a) Hub  b) Switch  c) Router  d) Repeater

### Subjective Questions:
1. Define broadcast domain and collision domain. Explain how different networking devices affect these domains.

2. Analyze the impact of collision domains and broadcast domains on network performance and design considerations.

## 4. DORA Process

### Objective Questions:
1. In the DHCP DORA process, what does the 'O' stand for?
   a) Open  b) Offer  c) Option  d) Order

2. Which DHCP message is sent as a broadcast by the client initially?
   a) DHCP Offer  b) DHCP Request  c) DHCP Discover  d) DHCP ACK

3. The DHCP server responds to DHCP Discover with:
   a) DHCP Request  b) DHCP ACK  c) DHCP Offer  d) DHCP NAK

### Subjective Questions:
1. Explain the complete DHCP DORA process step by step, including the purpose of each message and whether it's unicast or broadcast.

2. Describe what happens when a DHCP client renews its lease and how this differs from the initial DORA process.

## 5. TCP/IP and OSI Model

### Objective Questions:
1. How many layers does the TCP/IP model have?
   a) 4  b) 5  c) 7  d) 9

2. Which TCP/IP layer corresponds to OSI layers 5, 6, and 7?
   a) Network Interface  b) Internet  c) Transport  d) Application

3. TCP operates at which OSI layer?
   a) Layer 2  b) Layer 3  c) Layer 4  d) Layer 5

### Subjective Questions:
1. Compare and contrast the OSI and TCP/IP models, explaining the purpose of each layer and how they map to each other.

2. Explain the encapsulation process as data moves down the TCP/IP stack, including the headers added at each layer.

## 6. Port Security

### Objective Questions:
1. What is the default violation mode for port security?
   a) Protect  b) Restrict  c) Shutdown  d) Monitor

2. Maximum number of MAC addresses that can be learned on a port with default port security:
   a) 1  b) 2  c) 4  d) Unlimited

3. Which command enables port security on a switch port?
   a) switchport port-security  b) switchport security  c) port-security enable  d) security port enable

### Subjective Questions:
1. Explain the different violation modes in port security (shutdown, restrict, protect) and their respective behaviors when a violation occurs.

2. Describe the process of implementing port security on a Cisco switch, including sticky MAC address learning.

## 7. EtherChannel and Its Types

### Objective Questions:
1. Which protocols can be used to form EtherChannel?
   a) LACP only  b) PAgP only  c) Both LACP and PAgP  d) STP only

2. Maximum number of active links in an EtherChannel:
   a) 4  b) 6  c) 8  d) 16

3. LACP is defined in which IEEE standard?
   a) 802.1Q  b) 802.1D  c) 802.3ad  d) 802.1w

### Subjective Questions:
1. Explain the concept of EtherChannel, its benefits, and the difference between LACP and PAgP protocols.

2. Describe the configuration steps for setting up LACP EtherChannel between two switches.

## 8. NAT and Its Types

### Objective Questions:
1. Which type of NAT uses a pool of public IP addresses?
   a) Static NAT  b) Dynamic NAT  c) PAT  d) Inside NAT

2. PAT is also known as:
   a) Network Overload  b) Port Overload  c) NAT Overload  d) Address Overload

3. Which command shows NAT translations on a Cisco router?
   a) show nat  b) show ip nat translations  c) show ip nat  d) show translations

### Subjective Questions:
1. Explain the three types of NAT (Static, Dynamic, and PAT/NAT Overload) with examples and use cases for each.

2. Describe the NAT translation process and discuss the advantages and disadvantages of using NAT in modern networks.

## 9. ACL and Its Types

### Objective Questions:
1. Standard ACLs filter traffic based on:
   a) Source IP only  b) Destination IP only  c) Both source and destination IP  d) Port numbers

2. Extended ACLs use which number range?
   a) 1-99  b) 100-199  c) 1300-1999  d) 2000-2699

3. Where should standard ACLs be placed?
   a) Close to source  b) Close to destination  c) On core switches  d) On edge routers

### Subjective Questions:
1. Compare standard and extended ACLs, explaining their capabilities, number ranges, and best practice placement in network topology.

2. Explain the difference between named and numbered ACLs, and describe scenarios where each would be preferred.

## 10. ACL Configuration

### Objective Questions:
1. Which keyword is used to apply an ACL to an interface for incoming traffic?
   a) in  b) input  c) inbound  d) inside

2. What happens to traffic that doesn't match any ACL statement?
   a) Permitted  b) Denied  c) Logged  d) Forwarded

3. Which command removes a specific line from a named ACL?
   a) no access-list  b) remove  c) no [sequence-number]  d) delete line

### Subjective Questions:
1. Provide a step-by-step configuration example of both standard and extended ACLs, including application to interfaces.

2. Explain ACL processing order, wildcard masks, and common troubleshooting steps for ACL-related connectivity issues.

## 11. VTP & DTP

### Objective Questions:
1. Which VTP mode can create, modify, and delete VLANs?
   a) Client  b) Server  c) Transparent  d) Off

2. DTP frames are sent every:
   a) 30 seconds  b) 60 seconds  c) 90 seconds  d) 120 seconds

3. Which switchport mode will never form a trunk?
   a) access  b) trunk  c) dynamic auto  d) dynamic desirable

### Subjective Questions:
1. Explain VTP modes (Server, Client, Transparent, Off) and their roles in VLAN management across a switched network.

2. Describe DTP negotiation process and explain when you would disable DTP in a production environment.

## 12. VLANs and Its Types (Native and Default)

### Objective Questions:
1. What is the default VLAN ID on Cisco switches?
   a) VLAN 0  b) VLAN 1  c) VLAN 1002  d) VLAN 4094

2. The native VLAN carries:
   a) Tagged traffic only  b) Untagged traffic only  c) Both tagged and untagged  d) Management traffic only

3. Which VLAN range is reserved for extended VLANs?
   a) 1-1005  b) 1006-4094  c) 4095-8191  d) 1002-1005

### Subjective Questions:
1. Explain the concept of VLANs, their benefits, and the significance of native and default VLANs in trunk configuration.

2. Describe VLAN types (data, voice, management, native) and best practices for VLAN design and security.

## 13. Routing Protocols & Router ID (OSPF mainly)

### Objective Questions:
1. OSPF uses which algorithm for path calculation?
   a) Bellman-Ford  b) Dijkstra  c) Distance Vector  d) DUAL

2. OSPF Router ID is determined by (in order of preference):
   a) Highest IP, Loopback, Manual  b) Manual, Loopback, Highest IP  c) Loopback, Manual, Highest IP  d) Manual, Highest IP, Loopback

3. OSPF Hello packets are sent every:
   a) 10 seconds  b) 30 seconds  c) 40 seconds  d) 90 seconds

### Subjective Questions:
1. Explain OSPF operation including LSA flooding, SPF calculation, and the role of areas in reducing routing overhead.

2. Describe the OSPF neighbor establishment process and the significance of Router ID in OSPF operations.

## 14. STP (Subjective Question)

### Subjective Questions:
1. Explain the Spanning Tree Protocol (STP) operation, including the role of Bridge Priority, Port Cost, and Root Bridge selection process.

2. Compare the different STP variants (STP, RSTP, MST) and explain the enhancements each provides over the original 802.1D standard.

3. Describe STP port states and roles, and explain how STP prevents loops while maintaining redundancy in switched networks.

## 15. DHCP Spoofing

### Objective Questions:
1. DHCP snooping protects against which attack?
   a) ARP spoofing  b) DHCP spoofing  c) DNS spoofing  d) IP spoofing

2. Trusted ports in DHCP snooping can:
   a) Receive DHCP messages only  b) Send DHCP messages only  c) Send and receive DHCP messages  d) Block all DHCP messages

3. Which database does DHCP snooping maintain?
   a) CAM table  b) ARP table  c) DHCP binding table  d) MAC table

### Subjective Questions:
1. Explain DHCP spoofing attacks, their impact on network security, and how DHCP snooping provides protection.

2. Describe the configuration and operation of DHCP snooping, including trusted/untrusted ports and rate limiting.

## 16. BPDU Guard

### Objective Questions:
1. When BPDU Guard is triggered, what happens to the port?
   a) Blocks traffic  b) Goes to err-disabled state  c) Continues forwarding  d) Restarts

2. BPDU Guard is typically enabled on:
   a) Trunk ports  b) Access ports  c) Root ports  d) Designated ports

3. Which command enables BPDU Guard globally?
   a) spanning-tree bpduguard enable  b) spanning-tree portfast bpduguard default  c) bpdu guard enable  d) portfast bpduguard

### Subjective Questions:
1. Explain the purpose of BPDU Guard, scenarios where it should be implemented, and its relationship with PortFast.

2. Describe the security implications of unauthorized switches connecting to the network and how BPDU Guard helps mitigate these risks.

## 17. Reading Routing Tables

### Objective Questions:
1. In a routing table entry, what does the metric value represent?
   a) Hop count  b) Cost of the route  c) Administrative distance  d) Time to reach destination

2. Which routing table entry has the highest priority?
   a) Static route (AD 1)  b) OSPF (AD 110)  c) RIP (AD 120)  d) EIGRP (AD 90)

3. What does a routing table entry of "S*" indicate?
   a) Static route  b) Default static route  c) Directly connected  d) OSPF route

### Subjective Questions:
1. Explain how to read and interpret routing table entries, including route codes, administrative distance, and metric values.

2. Describe the route selection process when multiple paths to the same destination exist with different administrative distances and metrics.

## 18. Default Gateway

### Objective Questions:
1. The default gateway must be:
   a) The nearest router  b) In the same subnet as the host  c) The root bridge  d) A layer 2 device

2. When a host doesn't know the path to a destination, it sends traffic to:
   a) Broadcast address  b) Default gateway  c) DHCP server  d) DNS server

3. The default route is represented as:
   a) 0.0.0.0/32  b) 255.255.255.255/32  c) 0.0.0.0/0  d) 192.168.1.0/24

### Subjective Questions:
1. Explain the concept of default gateway, its role in routing decisions, and how hosts determine when to use it.

2. Describe the relationship between default gateway configuration on hosts and default route configuration on routers.

## 19. Calculating CIDR

### Objective Questions:
1. How many host addresses are available in a /26 network?
   a) 62  b) 64  c) 126  d) 128

2. What is the subnet mask for /28?
   a) 255.255.255.240  b) 255.255.255.248  c) 255.255.255.252  d) 255.255.255.224

3. How many /30 subnets can be created from a /28 network?
   a) 2  b) 4  c) 8  d) 16

### Subjective Questions:
1. Explain CIDR notation and demonstrate how to calculate network address, broadcast address, and number of hosts for different subnet masks.

2. Provide examples of subnetting a Class B network into smaller subnets, showing the mathematical process and resulting network ranges.

## 20. Telnet & SSH

### Objective Questions:
1. Which port does SSH use by default?
   a) 21  b) 22  c) 23  d) 80

2. Telnet traffic is:
   a) Encrypted  b) Compressed  c) Clear text  d) Hashed

3. Which version of SSH is considered secure?
   a) SSH v1 only  b) SSH v2 only  c) Both v1 and v2  d) Neither v1 nor v2

### Subjective Questions:
1. Compare Telnet and SSH protocols, explaining their security differences and why SSH is preferred for remote device management.

2. Describe the SSH handshake process and explain how to configure SSH on Cisco devices for secure remote access.