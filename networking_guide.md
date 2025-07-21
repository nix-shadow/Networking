# Networking Fundamentals: Newbie to Intermediate Guide

## 1. Cable Types

### Copper Cables
- **Straight-Through Cable**: Used to connect different device types (PC to switch, switch to router). Wiring: T568A or T568B on both ends
- **Crossover Cable**: Used to connect similar devices (PC to PC, switch to switch). Wiring: T568A on one end, T568B on other. Mostly obsolete due to Auto-MDIX
- **Console Cable**: Blue rollover cable for initial device configuration. Pin 1 connects to pin 8, pin 2 to pin 7, etc.

### Fiber Optic Cables
- **Single-Mode Fiber (SMF)**: Long-distance transmission (up to 100km+), uses laser light, 9-micron core
- **Multi-Mode Fiber (MMF)**: Short-distance transmission (up to 2km), uses LED light, 50/62.5-micron core
- **Common Connectors**: LC (small form factor), SC (square), ST (bayonet twist)

### Cable Categories
- **Cat5e**: 1 Gbps up to 100m, 100 MHz bandwidth
- **Cat6**: 1 Gbps up to 100m, 10 Gbps up to 55m, 250 MHz bandwidth
- **Cat6a**: 10 Gbps up to 100m, 500 MHz bandwidth

## 2. Router vs Switch Differences

### Switches (Layer 2 Device)
- **Function**: Forward frames based on MAC addresses within same network
- **Learning**: Builds MAC address table by examining source MAC addresses
- **Flooding**: Sends unknown unicast frames out all ports except receiving port
- **Broadcast Domain**: All ports share same broadcast domain (unless VLANs configured)
- **Collision Domain**: Each port creates separate collision domain

### Routers (Layer 3 Device)
- **Function**: Route packets between different networks using IP addresses
- **Tables**: Maintains routing table with network destinations
- **Broadcast**: Stops broadcast traffic - each interface is separate broadcast domain
- **Features**: NAT, ACLs, DHCP server, inter-VLAN routing, WAN connectivity

## 3. Broadcast & Collision Domains

### Collision Domain
- **Definition**: Network segment where data collisions can occur
- **Hub Impact**: All ports share single collision domain - causes performance issues
- **Switch Advantage**: Each port creates separate collision domain
- **Modern Reality**: Less relevant with full-duplex switching

### Broadcast Domain
- **Definition**: All devices that receive broadcast frames sent by any device in the domain
- **Switch Behavior**: By default, all ports share same broadcast domain
- **Router Segmentation**: Each router interface creates separate broadcast domain
- **VLAN Benefit**: Allows broadcast domain segmentation within single switch

## 4. DORA Process (DHCP)

### Four-Step DHCP Process
1. **Discover**: Client sends broadcast DHCP Discover (destination 255.255.255.255)
2. **Offer**: DHCP server responds with available IP address offer
3. **Request**: Client broadcasts DHCP Request for specific offered IP
4. **Acknowledge**: Server sends DHCP Ack confirming IP assignment

### Key Details
- **Lease Time**: How long client can use IP address
- **Renewal**: Client attempts to renew at 50% of lease time
- **Relay Agent**: Forwards DHCP messages across subnets (IP helper-address)
- **Reservations**: Static IP assignments based on MAC address

## 5. TCP/IP & OSI Models

### OSI Model (7 Layers)
1. **Physical**: Cables, electrical signals, hubs
2. **Data Link**: MAC addresses, switches, frames
3. **Network**: IP addresses, routers, packets  
4. **Transport**: TCP/UDP, port numbers, segments
5. **Session**: Connection establishment/management
6. **Presentation**: Encryption, compression, formatting
7. **Application**: HTTP, FTP, SMTP, user applications

### TCP/IP Model (4 Layers)
1. **Network Access**: Combines OSI Physical + Data Link
2. **Internet**: Same as OSI Network layer (IP)
3. **Transport**: Same as OSI Transport (TCP/UDP)
4. **Application**: Combines OSI Session + Presentation + Application

### Protocol Examples by Layer
- **Layer 1**: Ethernet physical, fiber optic standards
- **Layer 2**: Ethernet, PPP, Frame Relay
- **Layer 3**: IP, ICMP, OSPF, EIGRP
- **Layer 4**: TCP, UDP
- **Layer 7**: HTTP, FTP, SMTP, DNS, DHCP

## 6. Port Security

### Purpose
Limits number of MAC addresses allowed on switch port to prevent unauthorized access and MAC flooding attacks.

### Configuration Steps
```
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation shutdown
```

### Violation Actions
- **Shutdown**: Disables port (default) - requires manual intervention
- **Restrict**: Drops violating frames, increments violation counter
- **Protect**: Drops violating frames silently

### MAC Address Learning
- **Static**: Manually configured MAC addresses
- **Dynamic**: Learned automatically, lost on reboot
- **Sticky**: Learned automatically, saved to running config

## 7. EtherChannel & Types

### Purpose
Combines multiple physical links into single logical link for increased bandwidth and redundancy.

### Load Balancing Methods
- **src-mac**: Based on source MAC address
- **dst-mac**: Based on destination MAC address  
- **src-dst-mac**: Based on source and destination MAC
- **src-ip**: Based on source IP address
- **dst-ip**: Based on destination IP address
- **src-dst-ip**: Based on source and destination IP

### EtherChannel Protocols

#### PAgP (Port Aggregation Protocol) - Cisco Proprietary
- **Auto**: Passively responds to PAgP packets
- **Desirable**: Actively initiates PAgP negotiation
- **On**: Forces channel without negotiation

#### LACP (Link Aggregation Control Protocol) - IEEE 802.3ad
- **Passive**: Responds to LACP packets
- **Active**: Initiates LACP negotiation  
- **On**: Forces channel without negotiation

### Configuration Example
```
Switch(config)# interface range fa0/1-2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
```

## 8. NAT & Types

### Network Address Translation Purpose
Translates private IP addresses to public IP addresses for internet connectivity.

### NAT Types

#### Static NAT (One-to-One)
- Maps single private IP to single public IP
- Permanent mapping
- Used for servers requiring consistent external IP

#### Dynamic NAT (Many-to-Many)
- Maps private IPs to pool of public IPs
- First-come, first-served basis
- Mapping removed when session ends

#### PAT/NAT Overload (Many-to-One)
- Maps multiple private IPs to single public IP
- Uses port numbers to track connections
- Most common in home/small business routers

### Configuration Example (PAT)
```
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface serial0/0 overload
Router(config)# interface fa0/0
Router(config-if)# ip nat inside
Router(config)# interface serial0/0  
Router(config-if)# ip nat outside
```

## 9. ACL & Types

### Access Control Lists Purpose
Filter network traffic based on defined criteria (IP addresses, ports, protocols).

### Standard ACLs (1-99, 1300-1999)
- **Filter Criteria**: Source IP address only
- **Placement**: Close to destination
- **Range**: 1-99 and 1300-1999

### Extended ACLs (100-199, 2000-2699)
- **Filter Criteria**: Source IP, destination IP, protocol, port numbers
- **Placement**: Close to source
- **Range**: 100-199 and 2000-2699

### Wildcard Masks
- **0 = Must Match**: Bit must match exactly
- **1 = Don't Care**: Bit can be anything
- **Example**: 192.168.1.0 0.0.0.255 matches 192.168.1.0-192.168.1.255

### Implicit Deny
Every ACL ends with implicit "deny any" - must explicitly permit desired traffic.

## 10. ACL Configuration

### Standard ACL Example
```
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 10 deny 192.168.2.0 0.0.0.255
Router(config)# access-list 10 permit any
Router(config)# interface serial0/0
Router(config-if)# ip access-group 10 out
```

### Extended ACL Example
```
Router(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
Router(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config)# access-list 100 deny ip 192.168.1.0 0.0.0.255 any
Router(config)# interface fa0/0
Router(config-if)# ip access-group 100 in
```

### Named ACL Example
```
Router(config)# ip access-list extended WEB_TRAFFIC
Router(config-ext-nacl)# permit tcp any any eq 80
Router(config-ext-nacl)# permit tcp any any eq 443
Router(config-ext-nacl)# deny ip any any
Router(config-ext-nacl)# exit
Router(config)# interface fa0/1
Router(config-if)# ip access-group WEB_TRAFFIC in
```

## 11. VTP & DTP

### VLAN Trunking Protocol (VTP)
Synchronizes VLAN information across switches in same VTP domain.

#### VTP Modes
- **Server**: Creates, modifies, deletes VLANs; sends updates
- **Client**: Cannot create VLANs; receives and forwards updates
- **Transparent**: Creates local VLANs; forwards VTP messages but doesn't participate

#### VTP Configuration
```
Switch(config)# vtp domain COMPANY
Switch(config)# vtp mode server
Switch(config)# vtp password cisco123
Switch(config)# vtp version 2
```

### Dynamic Trunking Protocol (DTP)
Automatically negotiates trunk links between switches.

#### DTP Modes
- **Dynamic Auto**: Becomes trunk if other side is desirable or on
- **Dynamic Desirable**: Actively tries to become trunk
- **Trunk/On**: Forces trunk mode
- **Access**: Forces access mode
- **Nonegotiate**: Disables DTP negotiation

## 12. VLANs & Types

### Virtual LAN Purpose
Logically segments network into separate broadcast domains regardless of physical location.

### VLAN Types

#### Data VLANs (1-1005, 1006-4094)
- **Normal Range**: 1-1005 (stored in vlan.dat)
- **Extended Range**: 1006-4094 (stored in running-config)
- **Default**: VLAN 1 (cannot be deleted)

#### Management VLAN
- Used for switch management traffic
- Should not be VLAN 1 for security
- Configured with IP address for remote access

#### Native VLAN
- Untagged VLAN on trunk links
- Default is VLAN 1
- Should be changed for security

### VLAN Configuration
```
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name ENGINEERING
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

### Trunk Configuration
```
Switch(config)# interface fa0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,30
```

## 13. Routing Protocols & Router ID

### Routing Protocol Types

#### Distance Vector
- **RIP**: Hop count metric (max 15), slow convergence
- **EIGRP**: Composite metric, fast convergence, Cisco proprietary

#### Link State  
- **OSPF**: Cost metric based on bandwidth, fast convergence, open standard

#### Path Vector
- **BGP**: Used for internet routing between autonomous systems

### Router ID Selection (OSPF/EIGRP)
1. **Manual Configuration**: router-id command (highest priority)
2. **Highest Loopback IP**: If no manual configuration
3. **Highest Physical Interface IP**: If no loopback exists
4. **Must be up/up**: Interface must be active

### OSPF Configuration Example
```
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# passive-interface fa0/0
```

## 14. Spanning Tree Protocol (STP)

### Purpose
Prevents loops in switched networks by blocking redundant paths while maintaining connectivity.

### STP Process
1. **Root Bridge Election**: Lowest bridge ID becomes root
2. **Root Port Selection**: Lowest cost path to root bridge
3. **Designated Port Selection**: Lowest cost path on each segment
4. **Blocking State**: All other ports to prevent loops

### Bridge ID Components
- **Priority**: 0-65535 (default 32768), increments of 4096
- **MAC Address**: Switch MAC address (tie-breaker)

### Port States
- **Blocking**: Blocks traffic, listens to BPDUs (20 seconds)
- **Listening**: Processes BPDUs, no forwarding (15 seconds)  
- **Learning**: Builds MAC table, no forwarding (15 seconds)
- **Forwarding**: Normal operation
- **Disabled**: Administratively down

### STP Improvements
- **RSTP (802.1w)**: Faster convergence, compatibility with STP
- **PVST+**: Per-VLAN spanning tree (Cisco)
- **MSTP**: Maps multiple VLANs to single instance

### Configuration
```
Switch(config)# spanning-tree vlan 1 priority 4096
Switch(config)# interface fa0/1
Switch(config-if)# spanning-tree cost 10
Switch(config-if)# spanning-tree port-priority 64
```

## 15. DHCP Spoofing

### Attack Description
Rogue DHCP server provides malicious network configuration to clients, potentially redirecting traffic through attacker-controlled systems.

### Attack Process
1. Attacker sets up rogue DHCP server
2. Clients broadcast DHCP Discover
3. Both legitimate and rogue servers respond
4. Client may accept rogue offer first
5. Attacker provides malicious gateway/DNS settings

### Prevention: DHCP Snooping
```
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20
Switch(config)# interface fa0/1
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# interface range fa0/2-24
Switch(config-if-range)# ip dhcp snooping limit rate 5
```

### DHCP Snooping Features
- **Trusted Ports**: Allow DHCP server responses (uplinks, server ports)
- **Untrusted Ports**: Block DHCP server responses (client ports)
- **Rate Limiting**: Prevents DHCP Discover flooding
- **Binding Table**: Tracks IP-to-MAC bindings

## 16. BPDU Guard

### Purpose
Protects spanning tree topology by shutting down PortFast-enabled ports that receive BPDUs, preventing accidental loops.

### How It Works
1. Port configured with PortFast (assumes end device)
2. BPDU Guard enabled on port
3. If BPDU received, port immediately goes to err-disabled state
4. Manual intervention required to re-enable port

### Configuration
```
# Global configuration (affects all PortFast ports)
Switch(config)# spanning-tree portfast bpduguard default

# Interface-specific configuration
Switch(config)# interface fa0/1
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable

# Recovery from err-disabled
Switch(config)# errdisable recovery cause bpduguard
Switch(config)# errdisable recovery interval 300
```

### Related Features
- **PortFast**: Immediately transitions to forwarding state
- **Root Guard**: Prevents inferior BPDUs from becoming root
- **Loop Guard**: Prevents unidirectional link failures

## 17. Reading Routing Tables

### Routing Table Components
```
R     192.168.2.0/24 [120/1] via 10.1.1.2, 00:00:15, Serial0/0
```

### Code Breakdown
- **R**: Route source (RIP)
- **192.168.2.0/24**: Destination network/subnet mask
- **[120/1]**: Administrative distance/Metric
- **via 10.1.1.2**: Next-hop IP address
- **00:00:15**: Time since last update
- **Serial0/0**: Outgoing interface

### Route Source Codes
- **C**: Directly connected
- **S**: Static route
- **R**: RIP
- **D**: EIGRP
- **O**: OSPF
- **B**: BGP
- **i**: IS-IS

### Administrative Distance (Trustworthiness)
- **Directly Connected**: 0
- **Static**: 1
- **EIGRP**: 90
- **OSPF**: 110
- **RIP**: 120
- **External EIGRP**: 170

### Reading Example
```
Gateway of last resort is 192.168.1.1 to network 0.0.0.0

C    192.168.1.0/24 is directly connected, FastEthernet0/0
S    192.168.2.0/24 [1/0] via 192.168.1.2
D    192.168.3.0/24 [90/156160] via 10.1.1.2, 00:01:30, Serial0/1
O    192.168.4.0/24 [110/65] via 10.1.1.3, 00:02:45, Serial0/2
S*   0.0.0.0/0 [1/0] via 192.168.1.1
```

## 18. Default Gateway

### Purpose
Router IP address that devices use to reach networks outside their local subnet.

### How It Works
1. Device compares destination IP to its own subnet
2. If destination is local, sends directly
3. If destination is remote, sends to default gateway
4. Gateway routes packet toward destination

### Configuration Examples

#### Windows
```cmd
ipconfig /all                    # View current gateway
route add 0.0.0.0 mask 0.0.0.0 192.168.1.1  # Set default route
```

#### Cisco Router as Gateway
```
Router(config)# interface fa0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
```

#### Client Configuration
- **IP Address**: 192.168.1.100
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.1.1
- **DNS**: 8.8.8.8, 8.8.4.4

### Multiple Gateways
- Primary gateway handles most traffic
- Secondary gateways provide redundancy
- Routing table determines path selection
- Default route (0.0.0.0/0) catches all unmatched traffic

## 19. Calculating CIDR

### CIDR Notation
Classless Inter-Domain Routing expresses subnet mask as number of network bits.

### Common CIDR Values
- **/8** = 255.0.0.0 (Class A default)
- **/16** = 255.255.0.0 (Class B default)  
- **/24** = 255.255.255.0 (Class C default)
- **/30** = 255.255.255.252 (Point-to-point links)

### Subnet Calculation Process

#### Example: 192.168.1.0/26
1. **Network Bits**: 26 bits for network
2. **Host Bits**: 32 - 26 = 6 bits for hosts
3. **Subnet Mask**: 255.255.255.192
4. **Subnets**: 2^(borrowed bits) = 2^2 = 4 subnets
5. **Hosts**: 2^6 - 2 = 62 usable hosts per subnet

### Subnet Boundaries
**192.168.1.0/26** creates these subnets:
- **Subnet 0**: 192.168.1.0/26 (192.168.1.1-192.168.1.62)
- **Subnet 1**: 192.168.1.64/26 (192.168.1.65-192.168.1.126)
- **Subnet 2**: 192.168.1.128/26 (192.168.1.129-192.168.1.190)
- **Subnet 3**: 192.168.1.192/26 (192.168.1.193-192.168.1.254)

### Quick Calculation Method
1. Identify subnet mask from CIDR
2. Find subnet increment (256 - subnet octet)
3. List subnet boundaries
4. Determine network, broadcast, and usable range

### Variable Length Subnet Masking (VLSM)
Allows different subnet sizes within same major network:
- **/30** for point-to-point (2 hosts)
- **/28** for small networks (14 hosts)  
- **/24** for medium networks (254 hosts)

## 20. Telnet & SSH

### Remote Access Protocols
Both provide command-line access to network devices from remote locations.

### Telnet (Insecure)
- **Port**: 23/TCP
- **Security**: No encryption (plaintext)
- **Authentication**: Password only
- **Modern Use**: Lab environments only

### SSH (Secure)
- **Port**: 22/TCP
- **Security**: Encrypted communication
- **Authentication**: Username/password or key-based
- **Versions**: SSH v1 (deprecated), SSH v2 (current)

### Telnet Configuration
```
Router(config)# line vty 0 4
Router(config-line)# password cisco123
Router(config-line)# login
Router(config-line)# transport input telnet
```

### SSH Configuration
```
# Prerequisites
Router(config)# hostname R1
Router(config)# ip domain-name company.com
Router(config)# crypto key generate rsa modulus 1024

# User account
Router(config)# username admin privilege 15 secret cisco123

# VTY configuration  
Router(config)# line vty 0 4
Router(config-line)# transport input ssh
Router(config-line)# login local
Router(config-line)# exec-timeout 5 0

# SSH settings
Router(config)# ip ssh version 2
Router(config)# ip ssh time-out 60
Router(config)# ip ssh authentication-retries 3
```

### Client Connection
```bash
# SSH connection
ssh admin@192.168.1.1

# Telnet connection  
telnet 192.168.1.1

# SSH with specific options
ssh -l admin -p 22 192.168.1.1
```

### Security Best Practices
- Always use SSH instead of Telnet in production
- Use strong passwords or key-based authentication
- Limit VTY access with ACLs
- Configure exec-timeout to prevent idle sessions
- Use privilege levels to limit access
- Log all administrative access

---

*This guide covers fundamental networking concepts essential for network administrators and engineers. Practice these concepts in lab environments to reinforce understanding.*