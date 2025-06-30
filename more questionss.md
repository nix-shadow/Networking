# Advanced Networking Scenarios - IP and Diagram Based Questions

## 1. Cable Types & Physical Layer

### Scenario-Based Questions:

**Question 1:** You are designing a network for a 3-story office building. The main server room is on the 1st floor, with IDF closets on each floor. The distance from MDF to 3rd floor IDF is 120 meters. Design the cabling infrastructure.

**Network Diagram:**
```
Floor 3: [IDF-3] ----120m---- [MDF] :Floor 1
           |                    |
      [Workstations]       [Servers]
           |                    |
Floor 2: [IDF-2] ----85m-------+
           |
      [Workstations]
```

**Answer & Configuration:**

**Cabling Design:**
- **Backbone (MDF to IDF):** Fiber optic (Single-mode for future-proofing)
  - Distance exceeds 100m Cat6 limit
  - Higher bandwidth for inter-floor traffic
  - Immune to electrical interference

- **Horizontal (IDF to Workstations):** Cat6A UTP
  - Supports 10Gbps up to 100m
  - Cost-effective for access layer
  - Supports PoE+ for IP phones/APs

**Detailed Implementation:**
```
MDF Equipment:
- Core Switch: Cisco Catalyst 9400 with fiber modules
- Fiber: 12-strand Single-mode OS2
- Connectors: LC/SC type

IDF Equipment:
- Access Switches: Cisco Catalyst 9200 with SFP+ uplinks
- Horizontal: Cat6A UTP with RJ45 connectors
- Patch Panels: 48-port Cat6A

Cable Specifications:
- Backbone: 9/125μm Single-mode fiber
- Horizontal: 23 AWG Cat6A UTP, LSZH rated
- Maximum horizontal run: 90m (plus 10m patch cords)
```

---

## 2. Router vs Switch Configuration Scenarios

### Scenario-Based Questions:

**Question 2:** Design and configure a network with the following requirements:
- 3 VLANs: Sales (10.1.10.0/24), Engineering (10.1.20.0/24), Management (10.1.30.0/24)
- Inter-VLAN routing required
- Internet access through ISP (203.0.113.1/30)

**Network Diagram:**
```
                    ISP Router
                   203.0.113.1/30
                        |
                   [Router R1]
                  203.0.113.2/30
                  10.1.10.1/24 (VLAN 10)
                  10.1.20.1/24 (VLAN 20)  
                  10.1.30.1/24 (VLAN 30)
                        |
                   [Switch SW1]
                   Gi0/1 (Trunk)
          +-------------+-------------+
    VLAN 10        VLAN 20        VLAN 30
   Sales PCs    Engineering    Management
  10.1.10.0/24   10.1.20.0/24   10.1.30.0/24
```

**Complete Configuration:**

**Router R1 Configuration:**
```cisco
Router(config)# hostname R1
R1(config)# interface GigabitEthernet0/0
R1(config-if)# description "WAN to ISP"
R1(config-if)# ip address 203.0.113.2 255.255.255.252
R1(config-if)# no shutdown

R1(config)# interface GigabitEthernet0/1
R1(config-if)# description "LAN Trunk to SW1"
R1(config-if)# no shutdown

! Configure sub-interfaces for inter-VLAN routing
R1(config)# interface GigabitEthernet0/1.10
R1(config-subif)# description "Sales VLAN"
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 10.1.10.1 255.255.255.0
R1(config-subif)# ip helper-address 10.1.30.100

R1(config)# interface GigabitEthernet0/1.20
R1(config-subif)# description "Engineering VLAN"
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 10.1.20.1 255.255.255.0
R1(config-subif)# ip helper-address 10.1.30.100

R1(config)# interface GigabitEthernet0/1.30
R1(config-subif)# description "Management VLAN"
R1(config-subif)# encapsulation dot1Q 30
R1(config-subif)# ip address 10.1.30.1 255.255.255.0

! Default route to ISP
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1

! NAT Configuration
R1(config)# access-list 100 permit ip 10.1.0.0 0.0.255.255 any
R1(config)# ip nat inside source list 100 interface GigabitEthernet0/0 overload
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat outside
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat inside
```

**Switch SW1 Configuration:**
```cisco
Switch(config)# hostname SW1
SW1(config)# vlan 10
SW1(config-vlan)# name Sales
SW1(config)# vlan 20
SW1(config-vlan)# name Engineering
SW1(config)# vlan 30
SW1(config-vlan)# name Management

! Trunk port to router
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# description "Trunk to Router R1"
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30
SW1(config-if)# switchport trunk native vlan 999

! Access ports for different VLANs
SW1(config)# interface range FastEthernet0/1-10
SW1(config-if-range)# description "Sales Department"
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 10
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable

SW1(config)# interface range FastEthernet0/11-20
SW1(config-if-range)# description "Engineering Department"
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 20
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable

SW1(config)# interface range FastEthernet0/21-24
SW1(config-if-range)# description "Management"
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 30
```

---

## 3. Broadcast & Collision Domain Analysis

### Scenario-Based Questions:

**Question 3:** Analyze the following network topology and determine the number of broadcast and collision domains:

**Network Diagram:**
```
                    [Router R1]
                    Gi0/0 | Gi0/1
                    ------+------
                    |           |
               [Switch SW1]  [Switch SW2]
               24-port        24-port
               /    |    \    /    |    \
            [PC1] [PC2] [PC3] [PC4] [PC5] [PC6]
                    |           |
                [Hub H1]    [Hub H2]
                 4-port      4-port
                /  |  \     /  |  \
            [PC7][PC8][PC9][PC10][PC11][PC12]
```

**Analysis and Answer:**

**Collision Domains:**
- **Router R1:** Each interface = separate collision domain
  - Gi0/0: 1 collision domain
  - Gi0/1: 1 collision domain
  - **Total from router: 2**

- **Switch SW1:** Each port = separate collision domain
  - 24 switch ports: 24 collision domains
  - **Connected devices:**
    - PC1, PC2, PC3: 3 collision domains
    - Hub H1: 1 collision domain (shared by PC7, PC8, PC9)
  - **Total from SW1: 4 active collision domains**

- **Switch SW2:** Each port = separate collision domain
  - 24 switch ports: 24 collision domains
  - **Connected devices:**
    - PC4, PC5, PC6: 3 collision domains
    - Hub H2: 1 collision domain (shared by PC10, PC11, PC12)
  - **Total from SW2: 4 active collision domains**

**Total Collision Domains: 10**

**Broadcast Domains:**
- **Router separates broadcast domains**
- **Left side of router (SW1 network):** 1 broadcast domain
- **Right side of router (SW2 network):** 1 broadcast domain

**Total Broadcast Domains: 2**

**Performance Impact Analysis:**
```
Hub H1 Collision Domain:
- Shared 100Mbps between PC7, PC8, PC9
- Half-duplex operation
- Collision detection required
- Effective bandwidth: ~30Mbps per device

Switch Ports:
- Dedicated 100Mbps per port
- Full-duplex operation
- No collisions
- Effective bandwidth: 100Mbps per device

Recommendation: Replace hubs with switches
```

---

## 4. DHCP DORA Process with Packet Analysis

### Scenario-Based Questions:

**Question 4:** Configure DHCP on the network and trace the complete DORA process with packet details:

**Network Diagram:**
```
[DHCP Server]          [Router R1]          [Client PC]
192.168.1.100/24   Gi0/0 | Gi0/1        192.168.2.0/24
VLAN 10          .1 ----+---- .1           VLAN 20
                 192.168.1.0/24  192.168.2.0/24
```

**DHCP Server Configuration:**
```cisco
! DHCP Server (Cisco Router acting as DHCP server)
Router(config)# ip dhcp excluded-address 192.168.2.1 192.168.2.10
Router(config)# ip dhcp excluded-address 192.168.2.250 192.168.2.255

Router(config)# ip dhcp pool VLAN20_POOL
Router(dhcp-config)# network 192.168.2.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.2.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# domain-name company.local
Router(dhcp-config)# lease 7

! DHCP Relay configuration on Router R1
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip helper-address 192.168.1.100
```

**Complete DORA Process Analysis:**

**1. DHCP DISCOVER:**
```
Ethernet Header:
- Source MAC: AA:BB:CC:DD:EE:FF (Client)
- Dest MAC: FF:FF:FF:FF:FF:FF (Broadcast)

IP Header:
- Source IP: 0.0.0.0
- Dest IP: 255.255.255.255
- Protocol: UDP

UDP Header:
- Source Port: 68 (DHCP Client)
- Dest Port: 67 (DHCP Server)

DHCP Discover Payload:
- Message Type: BOOTREQUEST (1)
- Hardware Type: Ethernet (1)
- Hardware Length: 6
- Client Hardware Address: AA:BB:CC:DD:EE:FF
- Transaction ID: 0x12345678
- Options:
  * Option 53: DHCP Message Type = Discover (1)
  * Option 55: Parameter Request List
    - Subnet Mask (1)
    - Router (3)
    - DNS Server (6)
    - Domain Name (15)
```

**2. DHCP OFFER:**
```
Ethernet Header:
- Source MAC: 11:22:33:44:55:66 (DHCP Server)
- Dest MAC: AA:BB:CC:DD:EE:FF (Client)

IP Header:
- Source IP: 192.168.1.100
- Dest IP: 192.168.2.50 (Offered IP)
- Protocol: UDP

DHCP Offer Payload:
- Message Type: BOOTREPLY (2)
- Your IP Address: 192.168.2.50
- Server IP Address: 192.168.1.100
- Gateway IP Address: 192.168.2.1
- Client Hardware Address: AA:BB:CC:DD:EE:FF
- Transaction ID: 0x12345678
- Options:
  * Option 53: DHCP Message Type = Offer (2)
  * Option 1: Subnet Mask = 255.255.255.0
  * Option 3: Router = 192.168.2.1
  * Option 6: DNS Server = 8.8.8.8, 8.8.4.4
  * Option 15: Domain Name = company.local
  * Option 51: Lease Time = 604800 seconds (7 days)
  * Option 54: DHCP Server ID = 192.168.1.100
```

**3. DHCP REQUEST:**
```
Ethernet Header:
- Source MAC: AA:BB:CC:DD:EE:FF (Client)
- Dest MAC: FF:FF:FF:FF:FF:FF (Broadcast)

IP Header:
- Source IP: 0.0.0.0
- Dest IP: 255.255.255.255
- Protocol: UDP

DHCP Request Payload:
- Message Type: BOOTREQUEST (1)
- Requested IP Address: 192.168.2.50
- Client Hardware Address: AA:BB:CC:DD:EE:FF
- Transaction ID: 0x12345678
- Options:
  * Option 53: DHCP Message Type = Request (3)
  * Option 50: Requested IP Address = 192.168.2.50
  * Option 54: DHCP Server ID = 192.168.1.100
```

**4. DHCP ACKNOWLEDGE:**
```
Ethernet Header:
- Source MAC: 11:22:33:44:55:66 (DHCP Server)
- Dest MAC: AA:BB:CC:DD:EE:FF (Client)

IP Header:
- Source IP: 192.168.1.100
- Dest IP: 192.168.2.50
- Protocol: UDP

DHCP ACK Payload:
- Message Type: BOOTREPLY (2)
- Your IP Address: 192.168.2.50
- Server IP Address: 192.168.1.100
- Gateway IP Address: 192.168.2.1
- Client Hardware Address: AA:BB:CC:DD:EE:FF
- Transaction ID: 0x12345678
- Options: (Same as OFFER)
  * Option 53: DHCP Message Type = ACK (5)
  * All configuration parameters confirmed
```

**DHCP Lease Renewal Process:**
```
Timeline:
T1 (50% of lease): Client sends DHCP Request (unicast to server)
T2 (87.5% of lease): Client broadcasts DHCP Request if no response
Lease Expiry: Client stops using IP if no renewal
```

---

## 5. TCP/IP and OSI Model with Packet Encapsulation

### Scenario-Based Questions:

**Question 5:** Trace a web request (HTTP) from client to server through the complete TCP/IP stack:

**Network Diagram:**
```
[Client PC]          [Router]          [Web Server]
192.168.1.100    .1 ---------.1    10.0.0.100
                192.168.1.0/24    10.0.0.0/24
```

**Complete Packet Encapsulation Analysis:**

**Application Layer (HTTP Request):**
```
HTTP Request:
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html,application/xhtml+xml
Connection: keep-alive
```

**Transport Layer (TCP Segment):**
```
TCP Header:
- Source Port: 49152 (Client ephemeral port)
- Destination Port: 80 (HTTP)
- Sequence Number: 1000
- Acknowledgment Number: 0
- Flags: SYN=1 (Connection establishment)
- Window Size: 65535
- Checksum: 0x8B2A
- Options: MSS=1460, Window Scale=7

TCP Three-Way Handshake:
1. Client → Server: SYN (Seq=1000)
2. Server → Client: SYN-ACK (Seq=2000, Ack=1001)  
3. Client → Server: ACK (Seq=1001, Ack=2001)
```

**Network Layer (IP Packet):**
```
IP Header:
- Version: 4
- Header Length: 20 bytes
- Type of Service: 0x00
- Total Length: 1500 bytes
- Identification: 0x1234
- Flags: DF=1 (Don't Fragment)
- Fragment Offset: 0
- TTL: 64
- Protocol: 6 (TCP)
- Header Checksum: 0x4F8C
- Source IP: 192.168.1.100
- Destination IP: 10.0.0.100
```

**Data Link Layer (Ethernet Frame):**
```
Ethernet Header:
- Destination MAC: 00:11:22:33:44:55 (Router's LAN interface)
- Source MAC: AA:BB:CC:DD:EE:FF (Client's NIC)
- EtherType: 0x0800 (IPv4)

Ethernet Trailer:
- Frame Check Sequence (FCS): 0x8FA2B1C7 (CRC-32)
```

**Physical Layer:**
```
- Electrical signals on wire
- Manchester encoding (for 10Mbps Ethernet)
- 4B/5B encoding (for 100Mbps Fast Ethernet)
- 8B/10B encoding (for Gigabit Ethernet)
```

**Complete Configuration for Testing:**

**Client Configuration:**
```bash
# Configure IP address
ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1

# Test connectivity
ping 10.0.0.100
telnet 10.0.0.100 80

# Capture packets
tcpdump -i eth0 -w capture.pcap host 10.0.0.100
```

**Router Configuration:**
```cisco
Router(config)# interface GigabitEthernet0/0
Router(config-if)# description "LAN Side"
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# interface GigabitEthernet0/1  
Router(config-if)# description "WAN Side"
Router(config-if)# ip address 10.0.0.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

---

## 6. Port Security Implementation

### Scenario-Based Questions:

**Question 6:** Implement comprehensive port security on a 24-port access switch with different security levels:

**Network Diagram:**
```
                [Core Switch]
                      |
                [Access Switch SW1]
                      |
    +--------+--------+--------+--------+
    |        |        |        |        |
[Manager] [Sales1] [Sales2] [Guest]  [Server]
 Fa0/1    Fa0/2    Fa0/3    Fa0/4     Fa0/24
High Sec  Med Sec  Med Sec  Low Sec   Static
```

**Complete Port Security Configuration:**

**High Security Ports (Management):**
```cisco
SW1(config)# interface FastEthernet0/1
SW1(config-if)# description "Management - High Security"
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10

! Port Security Configuration
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 1
SW1(config-if)# switchport port-security violation shutdown
SW1(config-if)# switchport port-security mac-address 00:11:22:33:44:55
SW1(config-if)# switchport port-security aging time 1440
SW1(config-if)# switchport port-security aging type inactivity

! Additional Security Features
SW1(config-if)# spanning-tree portfast
SW1(config-if)# spanning-tree bpduguard enable
SW1(config-if)# storm-control broadcast level 10.00
SW1(config-if)# no cdp enable
```

**Medium Security Ports (Sales Department):**
```cisco
SW1(config)# interface range FastEthernet0/2-3
SW1(config-if-range)# description "Sales Department - Medium Security"
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 20

SW1(config-if-range)# switchport port-security
SW1(config-if-range)# switchport port-security maximum 2
SW1(config-if-range)# switchport port-security violation restrict
SW1(config-if-range)# switchport port-security mac-address sticky
SW1(config-if-range)# switchport port-security aging time 720
SW1(config-if-range)# switchport port-security aging type absolute

SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable
```

**Low Security Ports (Guest Access):**
```cisco
SW1(config)# interface FastEthernet0/4
SW1(config-if)# description "Guest Access - Low Security"  
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 30

SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 3
SW1(config-if)# switchport port-security violation protect
SW1(config-if)# switchport port-security mac-address sticky
SW1(config-if)# switchport port-security aging time 60
SW1(config-if)# switchport port-security aging type inactivity
```

**Server Port (Static Configuration):**
```cisco
SW1(config)# interface FastEthernet0/24
SW1(config-if)# description "Server - Static MAC"
SW1(config-if)# switchport mode access  
SW1(config-if)# switchport access vlan 100

SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 1
SW1(config-if)# switchport port-security violation shutdown
SW1(config-if)# switchport port-security mac-address 00:50:56:AA:BB:CC
SW1(config-if)# no switchport port-security aging time
```

**Global Port Security Settings:**
```cisco
! Enable automatic recovery from err-disabled
SW1(config)# errdisable recovery cause psecure-violation
SW1(config)# errdisable recovery interval 300

! Enable SNMP notifications
SW1(config)# snmp-server enable traps port-security trap-rate 10
```

**Monitoring and Troubleshooting Commands:**
```cisco
! View port security status
SW1# show port-security
SW1# show port-security interface fastethernet 0/1
SW1# show port-security address

! Monitor violations
SW1# show interfaces status err-disabled
SW1# show errdisable recovery

! Clear security violations
SW1# clear port-security sticky interface fastethernet 0/2
SW1(config)# interface fastethernet 0/1
SW1(config-if)# shutdown
SW1(config-if)# no shutdown
```

---

## 7. EtherChannel Configuration Scenarios

### Scenario-Based Questions:

**Question 7:** Configure LACP EtherChannel between two distribution switches with redundant links:

**Network Diagram:**
```
[Core Switch]
     |
     | (Single Link)
     |
[Distribution SW1]=====[Distribution SW2]
 Gi0/1-4              Gi0/1-4
    |                    |
    |  (EtherChannel)    |
    |                    |
[Access SW1]        [Access SW2]
```

**Complete LACP EtherChannel Configuration:**

**Distribution Switch 1:**
```cisco
SW1(config)# hostname Dist-SW1

! Configure physical interfaces
SW1(config)# interface range GigabitEthernet0/1-4
SW1(config-if-range)# description "LACP Bundle to Dist-SW2"
SW1(config-if-range)# no switchport
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# no shutdown

! Configure Port-Channel interface
SW1(config)# interface Port-Channel1
SW1(config-if)# description "LACP to Dist-SW2 - 4x1G = 4G"
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30,100
SW1(config-if)# switchport trunk native vlan 999
SW1(config-if)# spanning-tree guard root

! Load balancing configuration
SW1(config)# port-channel load-balance src-dst-mixed-ip-port

! LACP system priority (optional)
SW1(config)# lacp system-priority 100
```

**Distribution Switch 2:**
```cisco
SW2(config)# hostname Dist-SW2

! Configure physical interfaces  
SW2(config)# interface range GigabitEthernet0/1-4
SW2(config-if-range)# description "LACP Bundle to Dist-SW1"
SW2(config-if-range)# no switchport
SW2(config-if-range)# channel-group 1 mode active
SW2(config-if-range)# no shutdown

! Configure Port-Channel interface
SW2(config)# interface Port-Channel1
SW2(config-if)# description "LACP to Dist-SW1 - 4x1G = 4G"
SW2(config-if)# switchport mode trunk
SW2(config-if)# switchport trunk allowed vlan 10,20,30,100
SW2(config-if)# switchport trunk native vlan 999
SW2(config-if)# spanning-tree guard root

SW2(config)# port-channel load-balance src-dst-mixed-ip-port
SW2(config)# lacp system-priority 200
```

**Advanced EtherChannel Configuration:**

**LACP Fast Timers:**
```cisco
! Enable fast LACP timers for faster convergence
SW1(config)# interface range GigabitEthernet0/1-4
SW1(config-if-range)# lacp rate fast
SW1(config-if-range)# lacp port-priority 100

! Minimum links configuration
SW1(config)# interface Port-Channel1
SW1(config-if)# port-channel min-links 2
```

**Load Balancing Methods:**
```cisco
! Different load balancing options
SW1(config)# port-channel load-balance ?
  dst-ip       Dst IP Addr
  dst-mac      Dst Mac Addr  
  dst-mixed-ip-port  Dst IP addr and TCP/UDP Port
  dst-port     Dst TCP/UDP Port
  src-dst-ip   Src XOR Dst IP Addr
  src-dst-mac  Src XOR Dst Mac Addr
  src-dst-mixed-ip-port  Src XOR Dst IP addr and TCP/UDP Port
  src-dst-port Src XOR Dst TCP/UDP Port
  src-ip       Src IP Addr
  src-mac      Src Mac Addr
  src-mixed-ip-port  Src IP addr and TCP/UDP Port
  src-port     Src TCP/UDP Port

! Best practice for mixed traffic
SW1(config)# port-channel load-balance src-dst-mixed-ip-port
```

**Verification and Troubleshooting:**
```cisco
! Check EtherChannel status
SW1# show etherchannel summary
SW1# show etherchannel 1 detail
SW1# show interfaces Port-Channel1
SW1# show lacp neighbor
SW1# show lacp 1 counters

! Troubleshooting commands
SW1# show etherchannel load-balance
SW1# show interfaces GigabitEthernet0/1 etherchannel
SW1# debug etherchannel events
SW1# debug lacp all
```

**EtherChannel Guard Configuration:**
```cisco
! Prevent misconfiguration issues
SW1(config)# spanning-tree etherchannel guard misconfig
SW1(config)# interface range GigabitEthernet0/1-4
SW1(config-if-range)# spanning-tree guard loop
```

---

## 8. NAT Configuration Scenarios

### Scenario-Based Questions:

**Question 8:** Configure NAT for a company with multiple requirements:

**Network Diagram:**
```
Internet
   |
[ISP Router]
203.0.113.1/30
   |
[Company Router R1]
203.0.113.2/30 (Outside)
   |
192.168.1.1/24 (Inside)
   |
+--+--+--+--+
|  |  |  |  |
Sales Engineering DMZ Servers
.10-50  .51-100  .101-150  .151-200
```

**Requirements:**
- Sales: PAT (NAT Overload)
- Engineering: Dynamic NAT pool
- DMZ: Static NAT for web server
- Servers: No NAT (internal only)

**Complete NAT Configuration:**

**Interface Configuration:**
```cisco
R1(config)# interface GigabitEthernet0/0
R1(config-if)# description "WAN to ISP"
R1(config-if)# ip address 203.0.113.2 255.255.255.252
R1(config-if)# ip nat outside
R1(config-if)# no shutdown

R1(config)# interface GigabitEthernet0/1
R1(config-if)# description "LAN Interface"  
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# no shutdown
```

**Static NAT for DMZ Web Server:**
```cisco
! Static NAT for web server (192.168.1.120 → 203.0.113.10)
R1(config)# ip nat inside source static 192.168.1.120 203.0.113.10

! Port-specific static NAT for multiple services
R1(config)# ip nat inside source static tcp 192.168.1.120 80 203.0.113.10 80
R1(config)# ip nat inside source static tcp 192.168.1.120 443 203.0.113.10 443
R1(config)# ip nat inside source static tcp 192.168.1.121 25 203.0.113.10 25
```

**Dynamic NAT Pool for Engineering:**
```cisco
! Create NAT pool for engineering department
R1(config)# ip nat pool ENGINEERING_POOL 203.0.113.20 203.0.113.30 netmask 255.255.255.240

! Define access list for engineering subnet
R1(config)# access-list 101 permit ip 192.168.1.51 0.0.0.49 any

! Apply dynamic NAT
R1(config)# ip nat inside source list 101 pool ENGINEERING_POOL
```

**PAT (NAT Overload) for Sales:**
```cisco
! Define access list for sales subnet  
R1(config)# access-list 102 permit ip 192.168.1.10 0.0.0.39 any

! Configure PAT using outside interface
R1(config)# ip nat inside source list 102 interface GigabitEthernet0/0 overload
```

**Policy-Based NAT (Advanced):**
```cisco
! Route-map for different NAT policies
R1(config)# route-map SALES_NAT permit 10
R1(config-route-map)# match ip address 102
R1(config-route-map)# match interface GigabitEthernet0/0

R1(config)# route-map ENGINEERING_NAT permit 10  
R1(config-route-map)# match ip address 101
R1(config-route-map)# match interface GigabitEthernet0/0

! Apply policy NAT
R1(config)# ip nat inside source route-map SALES_NAT interface GigabitEthernet0/0 overload
R1(config)# ip nat inside source route-map ENGINEERING_NAT pool ENGINEERING_POOL
```

**NAT Monitoring and Troubleshooting:**
```cisco
! View NAT translations
R1# show ip nat translations
R1# show ip nat translations verbose
R1# show ip nat statistics

! Clear NAT translations
R1# clear ip nat translation *
R1# clear ip nat translation inside 192.168.1.50 203.0.113.20

! Debug NAT
R1# debug ip nat
R1# debug ip nat detailed
```

**NAT Translation Table Example:**
```
R1# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
tcp 203.0.113.2:1024  192.168.1.15:1024  74.125.224.72:80   74.125.224.72:80
tcp 203.0.113.2:1025  192.168.1.16:1025  74.125.224.72:80   74.125.224.72:80
--- 203.0.113.10      192.168.1.120      ---                ---
tcp 203.0.113.10:80   192.168.1.120:80   ---                ---
```

---

## 9. ACL Implementation Scenarios

### Scenario-Based Questions:

**Question 9:** Implement comprehensive ACL security for the following network:

**Network Diagram:**
```
Internet (Any)
     |
[Router R1] 203.0.113.1/30
     |
192.168.1.1/24
     |
+----+----+----+
|    |    |    |
Sales Engineering Management DMZ
.10/28 .20/28    .30/28      .100/26
VLAN10 VLAN20   VLAN30    VLAN100
```

**Security Requirements:**
1. Sales can access Internet (HTTP/HTTPS only)
2. Engineering can access Internet (all protocols)
3. Management can access all networks
4. DMZ can only receive HTTP/HTTPS from Internet
5. No inter-VLAN communication except Management
6. Block social media sites for Sales

**Complete ACL Implementation:**

**Extended ACL for Sales (VLAN 10):**
```cisco
! Block social media sites
R1(config)# ip access-list extended SALES_OUTBOUND
R1(config-ext-nacl)# remark "=== Block Social Media ==="
R1(config-ext-nacl)# deny tcp 192.168.1.16 0.0.0.15 host 31.13.64.35
R1(config-ext-nacl)# deny tcp 192.168.1.16 0.0.0.15 host 199.59.148.82
R1(config-ext-nacl)# deny tcp 192.168.1.16 0.0.0.15 host 104.244.42.129

R1(config-ext-nacl)# remark "=== Allow HTTP/HTTPS to Internet ==="
R1(config-ext-nacl)# permit tcp 192.168.1.16 0.0.0.15 any eq 80
R1(config-ext-nacl)# permit tcp 192.168.1.16 0.0.0.15 any eq 443

R1(config-ext-nacl)# remark "=== Allow DNS ==="
R1(config-ext-nacl)# permit udp 192.168.1.16 0.0.0.15 any eq 53

R1(config-ext-nacl)# remark "=== Block inter-VLAN ==="
R1(config-ext-nacl)# deny ip 192.168.1.16 0.0.0.15 192.168.1.32 0.0.0.15
R1(config-ext-nacl)# deny ip 192.168.1.16 0.0.0.15 192.168.1.48 0.0.0.15  
R1(config-ext-nacl)# deny ip 192.168.1.16 0.0.0.15 192.168.1.100 0.0.0.63

R1(config-ext-nacl)# remark "=== Implicit Deny ==="
R1(config-ext-nacl)# deny ip any any log
```

**Extended ACL for Engineering (VLAN 20):**
```cisco
R1(config)# ip access-list extended ENGINEERING_OUTBOUND
R1(config-ext-nacl)# remark "=== Allow all Internet access ==="
R1(config-ext-nacl)# permit ip 192.168.1.32 0.0.0.15 any

R1(config-ext-nacl)# remark "=== Block inter-VLAN except Management ==="
R1(config-ext-nacl)# deny ip 192.168.1.32 0.0.0.15 192.168.1.16 0.0.0.15
R1(config-ext-nacl)# deny ip 192.168.1.32 0.0.0.15 192.168.1.100 0.0.0.63
R1(config-ext-nacl)# permit ip 192.168.1.32 0.0.0.15 192.168.1.48 0.0.0.15

R1(config-ext-nacl)# deny ip any any log
```

**Extended ACL for Management (VLAN 30):**
```cisco
R1(config)# ip access-list extended MANAGEMENT_OUTBOUND
R1(config-ext-nacl)# remark "=== Management has full access ==="
R1(config-ext-nacl)# permit ip 192.168.1.48 0.0.0.15 any
```

**Extended ACL for DMZ Inbound (from Internet):**
```cisco
R1(config)# ip access-list extended DMZ_INBOUND
R1(config-ext-nacl)# remark "=== Allow HTTP/HTTPS to web servers ==="
R1(config-ext-nacl)# permit tcp any 192.168.1.100 0.0.0.63 eq 80
R1(config-ext-nacl)# permit tcp any 192.168.1.100 0.0.0.63 eq 443

R1(config-ext-nacl)# remark "=== Allow established connections ==="
R1(config-ext-nacl)# permit tcp any 192.168.1.100 0.0.0.63 established

R1(config-ext-nacl)# remark "=== Block everything else ==="
R1(config-ext-nacl)# deny ip any any log
```

**Time-Based ACL (Business Hours Only):**
```cisco
! Define time range for business hours
R1(config)# time-range BUSINESS_HOURS
R1(config-time-range)# periodic weekdays 8:00 to 18:00

! Apply time-based restrictions
R1(config)# ip access-list extended SALES_TIME_RESTRICTED
R1(config-ext-nacl)# permit tcp 192.168.1.16 0.0.0.15 any eq 80 time-range BUSINESS_HOURS
R1(config-ext-nacl)# permit tcp 192.168.1.16 0.0.0.15 any eq 443 time-range BUSINESS_HOURS
R1(config-ext-nacl)# deny ip any any log
```

**Apply ACLs to Interfaces:**
```cisco
! Sub-interface for VLAN 10 (Sales)
R1(config)# interface GigabitEthernet0/1.10
R1(config-subif)# ip access-group SALES_OUTBOUND out

! Sub-interface for VLAN 20 (Engineering)  
R1(config)# interface GigabitEthernet0/1.20
R1(config-subif)# ip access-group ENGINEERING_OUTBOUND out

! Sub-interface for VLAN 30 (Management)
R1(config)# interface GigabitEthernet0/1.30
R1(config-subif)# ip access-group MANAGEMENT_OUTBOUND out

! WAN interface (DMZ protection)
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip access-group DMZ_INBOUND in
```

**Object Groups for Scalability:**
```cisco
! Create object groups for easier management
R1(config)# object-group network INTERNAL_NETWORKS
R1(config-network-group)# host 192.168.1.16
R1(config-network-group)# host 192.168.1.32  
R1(config-network-group)# host 192.168.1.48
R1(config-network-group)# host 192.168.1.100

R1(config)# object-group service WEB_SERVICES tcp
R1(config-service-group)# port-object eq 80
R1(config-service-group)# port-object eq 443
R1(config-service-group)# port-object eq 8080

! Use object groups in ACL
R1(config)# ip access-list extended OPTIMIZED_ACL
R1(config-ext-nacl)# permit tcp object-group INTERNAL_NETWORKS any object-group WEB_SERVICES
```

---

## 10. VTP & DTP Configuration

### Scenario-Based Questions:

**Question 10:** Configure VTP domain with multiple switches and secure DTP:

**Network Diagram:**
```
[Core SW1]        [Core SW2]
VTP Server        VTP Server
    |                |
    +-------+--------+
            |
    [Distribution SW1]
    VTP Transparent
            |
    +-------+-------+
    |               |
[Access SW1]    [Access SW2]  
VTP Client      VTP Client
```

**VTP Domain Configuration:**

**Core Switch 1 (VTP Server):**
```cisco
SW1(config)# hostname Core-SW1
SW1(config)# vtp domain COMPANY.LOCAL
SW1(config)# vtp mode server
SW1(config)# vtp password SecureVTP123
SW1(config)# vtp version 2
SW1(config)# vtp pruning

! Create VLANs (will propagate to clients)
SW1(config)# vlan 10
SW1(config-vlan)# name Sales
SW1(config)# vlan 20
SW1(config-vlan)# name Engineering  
SW1(config)# vlan 30
SW1(config-vlan)# name Management
SW1(config)# vlan 100
SW1(config-vlan)# name DMZ
SW1(config)# vlan 999
SW1(config-vlan)# name Native_Unused
```

**Core Switch 2 (VTP Server - Redundant):**
```cisco
SW2(config)# hostname Core-SW2
SW2(config)# vtp domain COMPANY.LOCAL
SW2(config)# vtp mode server
SW2(config)# vtp password SecureVTP123
SW2(config)# vtp version 2
SW2(config)# vtp pruning
```

**Distribution Switch (VTP Transparent):**
```cisco
SW3(config)# hostname Dist-SW1
SW3(config)# vtp domain COMPANY.LOCAL
SW3(config)# vtp mode transparent
SW3(config)# vtp password SecureVTP123
SW3(config)# vtp version 2

! Local VLANs (not propagated)
SW3(config)# vlan 200
SW3(config-vlan)# name Local_Management
SW3(config)# vlan 201  
SW3(config-vlan)# name Local_Monitoring
```

**Access Switches (VTP Client):**
```cisco
! Access Switch 1
SW4(config)# hostname Access-SW1
SW4(config)# vtp domain COMPANY.LOCAL
SW4(config)# vtp mode client
SW4(config)# vtp password SecureVTP123
SW4(config)# vtp version 2

! Access Switch 2  
SW5(config)# hostname Access-SW2
SW5(config)# vtp domain COMPANY.LOCAL
SW5(config)# vtp mode client
SW5(config)# vtp password SecureVTP123
SW5(config)# vtp version 2
```

**Secure DTP Configuration:**

**Trunk Configuration with DTP Security:**
```cisco
! Core to Distribution trunk
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# description "Trunk to Dist-SW1"
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 999
SW1(config-if)# switchport trunk allowed vlan 10,20,30,100
SW1(config-if)# switchport nonegotiate
SW1(config-if)# spanning-tree guard root

! Distribution to Access trunk
SW3(config)# interface GigabitEthernet0/2
SW3(config-if)# description "Trunk to Access-SW1"
SW3(config-if)# switchport mode trunk  
SW3(config-if)# switchport trunk native vlan 999
SW3(config-if)# switchport trunk allowed vlan 10,20,30
SW3(config-if)# switchport nonegotiate
SW3(config-if)# spanning-tree guard root
```

**Access Port Security (Disable DTP):**
```cisco
! Secure access ports
SW4(config)# interface range FastEthernet0/1-24
SW4(config-if-range)# switchport mode access
SW4(config-if-range)# switchport nonegotiate
SW4(config-if-range)# spanning-tree portfast
SW4(config-if-range)# spanning-tree bpduguard enable

! Specific VLAN assignments
SW4(config)# interface range FastEthernet0/1-8
SW4(config-if-range)# switchport access vlan 10

SW4(config)# interface range FastEthernet0/9-16  
SW4(config-if-range)# switchport access vlan 20

SW4(config)# interface range FastEthernet0/17-20
SW4(config-if-range)# switchport access vlan 30
```

**VTP Pruning Configuration:**
```cisco
! Enable VTP pruning (on VTP servers only)
SW1(config)# vtp pruning

! Configure pruning-eligible VLANs
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# switchport trunk pruning vlan remove 10,30

! Monitor pruning
SW1# show interfaces trunk
SW1# show vtp status
```

**VTP Security Best Practices:**
```cisco
! Change VTP to Off mode for security
SW4(config)# vtp mode off

! Disable VTP completely
SW4(config)# no vtp

! Use VTP version 3 (if supported)
SW1(config)# vtp version 3
SW1(config)# vtp primary vlan force
```

**Monitoring Commands:**
```cisco
! VTP status and statistics
SW1# show vtp status
SW1# show vtp counters
SW1# show vlan brief

! DTP negotiation status
SW1# show dtp interface GigabitEthernet0/1
SW1# show interfaces GigabitEthernet0/1 switchport

! Troubleshooting
SW1# show vtp password
SW1# debug vtp events
SW1# debug dtp events
```

This comprehensive guide covers IP-based scenarios with detailed configurations for all the networking topics. Each scenario includes:

1. **Realistic network diagrams**
2. **Complete device configurations**
3. **IP addressing schemes**
4. **Security implementations**
5. **Monitoring and troubleshooting**
6. **Best practices**

The scenarios progress from basic to advanced concepts, providing practical experience with real-world networking challenges. Would you like me to continue with the remaining topics (OSPF, STP, DHCP Snooping, etc.) with similar detailed scenarios?