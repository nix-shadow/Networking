# Networking Test Answers - Answer Key with Explanations

## 1. Cable Types

### Objective Answers:
1. **c) Fiber optic** - Fiber optic cables are preferred for backbone connections due to their high bandwidth capacity, long-distance capabilities, and immunity to electromagnetic interference.

2. **a) 100 meters** - Cat6 UTP cable has a maximum transmission distance of 100 meters (328 feet) for Ethernet applications at full speed.

3. **c) SC/LC** - SC (Subscriber Connector) and LC (Local Connector) are common fiber optic connectors. ST connectors are also used but less common in modern installations.

### Subjective Answers:

1. **Fiber vs. Copper Comparison:**
   
   **Fiber Optic Advantages:**
   - Higher bandwidth capacity (Gbps to Tbps)
   - Longer transmission distances (up to 40km+)
   - Immune to electromagnetic interference
   - More secure (difficult to tap)
   - Lower signal attenuation
   
   **Fiber Optic Disadvantages:**
   - Higher initial cost
   - Requires specialized equipment and expertise
   - More fragile than copper
   - Difficult to splice and terminate in field
   
   **Copper Advantages:**
   - Lower cost for short distances
   - Easier to install and terminate
   - Can carry power (PoE)
   - Widely available expertise
   
   **Copper Disadvantages:**
   - Limited bandwidth and distance
   - Susceptible to EMI
   - Security concerns (easier to tap)
   - Higher power consumption for active equipment

2. **Straight-through vs. Crossover Cables:**
   
   **Straight-through Cable:**
   - Pin 1-1, 2-2, 3-3, etc. (same wiring on both ends)
   - Used for: PC to switch, router to switch, switch to hub
   - Most common cable type in modern networks
   
   **Crossover Cable:**
   - Pins 1&2 cross with 3&6 on opposite end
   - Used for: PC to PC, switch to switch, router to router
   - Less common due to Auto-MDIX feature in modern devices

## 2. Router vs. Switch

### Objective Answers:
1. **c) Layer 3** - Routers operate at the Network layer (Layer 3) of the OSI model, making routing decisions based on IP addresses.

2. **b) MAC address** - Switches use MAC addresses for forwarding decisions, maintaining a MAC address table to learn device locations.

3. **c) Router** - Routers can connect different network segments with different subnet addresses, providing inter-VLAN and inter-network communication.

### Subjective Answers:

1. **Router vs. Switch Differences:**
   
   **Router:**
   - OSI Layer 3 (Network layer)
   - Routes packets between different networks/subnets
   - Uses IP addresses for forwarding decisions
   - Creates separate broadcast domains
   - Provides NAT, DHCP, firewall capabilities
   - Lower port density, higher cost per port
   
   **Switch:**
   - OSI Layer 2 (Data Link layer)
   - Forwards frames within the same network segment
   - Uses MAC addresses for forwarding decisions
   - Single broadcast domain (unless VLANs configured)
   - Provides VLAN segmentation, STP, port security
   - Higher port density, lower cost per port

2. **Usage Scenarios:**
   
   **Use Router when:**
   - Connecting different networks/subnets
   - Need routing protocols (OSPF, EIGRP)
   - Require NAT functionality
   - Need to control broadcast domains
   - Implementing security policies between networks
   
   **Use Switch when:**
   - Connecting devices within same subnet
   - Need high-speed local connectivity
   - Require VLAN segmentation
   - Building access layer in hierarchical design
   - Cost-effective port multiplication

## 3. Broadcast & Collision Domains

### Objective Answers:
1. **c) 24** - Each port on a switch creates its own collision domain, so a 24-port switch creates 24 collision domains.

2. **c) 4** - Each interface on a router creates a separate broadcast domain, so 4 interfaces = 4 broadcast domains.

3. **b) Switch** - Switches separate collision domains (each port is its own collision domain) but maintain a single broadcast domain unless VLANs are configured.

### Subjective Answers:

1. **Definitions and Device Impact:**
   
   **Collision Domain:**
   - Network segment where data collisions can occur
   - All devices share the same medium and compete for access
   - Smaller domains = better performance
   
   **Broadcast Domain:**
   - Network segment where broadcast frames are propagated
   - All devices receive broadcast traffic
   - Larger domains = more broadcast traffic
   
   **Device Impact:**
   - **Hub:** Single collision domain, single broadcast domain
   - **Switch:** Multiple collision domains (one per port), single broadcast domain
   - **Router:** Multiple collision domains, multiple broadcast domains

2. **Performance Impact:**
   
   **Collision Domains:**
   - Large collision domains increase collision probability
   - Reduce effective bandwidth utilization
   - Cause retransmissions and delays
   
   **Broadcast Domains:**
   - Large broadcast domains increase broadcast traffic
   - Consume bandwidth with unnecessary traffic
   - Create security and performance issues
   
   **Design Considerations:**
   - Use switches to minimize collision domains
   - Use routers/VLANs to control broadcast domains
   - Implement hierarchical network design

## 4. DORA Process

### Objective Answers:
1. **b) Offer** - DORA stands for Discover, Offer, Request, Acknowledge.

2. **c) DHCP Discover** - The client broadcasts a DHCP Discover message to find available DHCP servers.

3. **c) DHCP Offer** - The server responds to DHCP Discover with a DHCP Offer containing an available IP address.

### Subjective Answers:

1. **Complete DORA Process:**
   
   **1. DHCP Discover (Client → Broadcast):**
   - Client broadcasts to 255.255.255.255
   - Source: 0.0.0.0, Destination: 255.255.255.255
   - Requests IP configuration from any DHCP server
   
   **2. DHCP Offer (Server → Client):**
   - Server responds with available IP address
   - Unicast or broadcast (depending on client capabilities)
   - Includes IP address, subnet mask, lease time, server ID
   
   **3. DHCP Request (Client → Broadcast):**
   - Client broadcasts acceptance of offered IP
   - Informs other servers that offer was accepted
   - Requests additional configuration options
   
   **4. DHCP Acknowledge (Server → Client):**
   - Server confirms IP address assignment
   - Provides additional configuration (gateway, DNS)
   - Establishes lease agreement

2. **DHCP Lease Renewal:**
   
   **Renewal Process:**
   - Client attempts renewal at 50% of lease time
   - Sends unicast DHCP Request to original server
   - If successful, server responds with DHCP ACK
   - If unsuccessful, tries at 87.5% of lease time
   - At lease expiration, starts new DORA process
   
   **Differences from Initial DORA:**
   - Uses unicast instead of broadcast (when possible)
   - Skips Discover and Offer phases during successful renewal
   - Maintains same IP address if possible

## 5. TCP/IP and OSI Models

### Objective Answers:
1. **a) 4** - TCP/IP model has 4 layers: Network Interface, Internet, Transport, and Application.

2. **d) Application** - The TCP/IP Application layer corresponds to OSI layers 5 (Session), 6 (Presentation), and 7 (Application).

3. **c) Layer 4** - TCP operates at the Transport layer (Layer 4) of the OSI model.

### Subjective Answers:

1. **Model Comparison:**
   
   **OSI Model (7 layers):**
   1. Physical - Bits, cables, hardware
   2. Data Link - Frames, MAC addresses, switches
   3. Network - Packets, IP addresses, routers
   4. Transport - Segments, TCP/UDP, port numbers
   5. Session - Connections, sessions
   6. Presentation - Encryption, compression, formatting
   7. Application - User interfaces, applications
   
   **TCP/IP Model (4 layers):**
   1. Network Interface (OSI 1-2) - Physical network access
   2. Internet (OSI 3) - IP routing and addressing
   3. Transport (OSI 4) - TCP/UDP protocols
   4. Application (OSI 5-7) - Application protocols
   
   **Mapping:**
   - TCP/IP is more practical and widely implemented
   - OSI is more theoretical and detailed
   - TCP/IP was developed based on real-world implementation

2. **Encapsulation Process:**
   
   **Application Layer:**
   - Data created by application
   - HTTP, FTP, SMTP headers added
   
   **Transport Layer:**
   - TCP/UDP header added
   - Source/destination ports, sequence numbers
   - Creates segments
   
   **Internet Layer:**
   - IP header added
   - Source/destination IP addresses
   - Creates packets
   
   **Network Interface Layer:**
   - Ethernet header and trailer added
   - Source/destination MAC addresses
   - Creates frames

## 6. Port Security

### Objective Answers:
1. **c) Shutdown** - The default violation mode for port security is shutdown, which puts the port in err-disabled state.

2. **a) 1** - By default, port security allows only 1 MAC address to be learned on a secured port.

3. **a) switchport port-security** - This command enables port security on a switch port.

### Subjective Answers:

1. **Violation Modes:**
   
   **Shutdown Mode:**
   - Port goes to err-disabled state
   - All traffic stopped
   - Manual intervention required to re-enable
   - Most secure option
   
   **Restrict Mode:**
   - Violating traffic dropped
   - SNMP trap and syslog message generated
   - Port remains operational for known MAC addresses
   - Violation counter incremented
   
   **Protect Mode:**
   - Violating traffic dropped silently
   - No notifications generated
   - Port remains operational for known MAC addresses
   - Least secure option

2. **Implementation Process:**
   
   **Basic Configuration:**
   ```
   interface fastethernet 0/1
   switchport mode access
   switchport port-security
   switchport port-security maximum 2
   switchport port-security violation restrict
   switchport port-security mac-address sticky
   ```
   
   **Sticky MAC Learning:**
   - Automatically learns connected MAC addresses
   - Stores them in running configuration
   - Survives switch reboot when saved
   - Combines convenience with security

## 7. EtherChannel

### Objective Answers:
1. **c) Both LACP and PAgP** - EtherChannel can be formed using LACP (IEEE 802.3ad) or PAgP (Cisco proprietary).

2. **c) 8** - A single EtherChannel can have a maximum of 8 active links.

3. **c) 802.3ad** - LACP is defined in IEEE 802.3ad standard (now part of 802.3).

### Subjective Answers:

1. **EtherChannel Concepts:**
   
   **Benefits:**
   - Increased bandwidth by bundling multiple links
   - Load distribution across member links
   - Redundancy - failure of one link doesn't break connectivity
   - STP sees EtherChannel as single logical link
   
   **LACP vs. PAgP:**
   
   **LACP (Link Aggregation Control Protocol):**
   - IEEE standard (802.3ad)
   - Industry standard, vendor-neutral
   - Modes: active, passive, on
   - More widely supported
   
   **PAgP (Port Aggregation Protocol):**
   - Cisco proprietary
   - Only works between Cisco devices
   - Modes: desirable, auto, on
   - Being phased out in favor of LACP

2. **LACP Configuration:**
   
   **Switch 1:**
   ```
   interface range fastethernet 0/1-2
   channel-group 1 mode active
   exit
   interface port-channel 1
   switchport mode trunk
   switchport trunk allowed vlan 1,10,20
   ```
   
   **Switch 2:**
   ```
   interface range fastethernet 0/1-2
   channel-group 1 mode active
   exit
   interface port-channel 1
   switchport mode trunk
   switchport trunk allowed vlan 1,10,20
   ```
   
   **Verification:**
   - show etherchannel summary
   - show interfaces port-channel 1

## 8. NAT Types

### Objective Answers:
1. **b) Dynamic NAT** - Dynamic NAT uses a pool of public IP addresses and assigns them on a first-come, first-served basis.

2. **c) NAT Overload** - PAT (Port Address Translation) is also known as NAT Overload.

3. **b) show ip nat translations** - This command displays active NAT translations on a Cisco router.

### Subjective Answers:

1. **NAT Types Explained:**
   
   **Static NAT:**
   - One-to-one mapping between private and public IP
   - Permanent mapping configured manually
   - Use case: Servers that need consistent external IP
   - Example: 192.168.1.10 always translates to 203.0.113.10
   
   **Dynamic NAT:**
   - Pool of public IPs assigned dynamically
   - First-come, first-served basis
   - Use case: When you have multiple public IPs but fewer than private IPs
   - Mapping changes each session
   
   **PAT/NAT Overload:**
   - Many private IPs share one public IP
   - Uses port numbers to distinguish sessions
   - Most common in SOHO environments
   - Example: Multiple devices share single ISP-provided IP

2. **NAT Translation Process and Implications:**
   
   **Translation Process:**
   1. Internal host sends packet to external destination
   2. NAT device receives packet on inside interface
   3. Replaces source IP (and port for PAT) with public IP
   4. Creates translation entry in NAT table
   5. Forwards packet to external network
   6. Return traffic follows reverse process
   
   **Advantages:**
   - Conserves public IP addresses
   - Provides security through address hiding
   - Allows network redesign without renumbering
   - Enables multiple private networks to use same addressing
   
   **Disadvantages:**
   - Breaks end-to-end connectivity principle
   - Complicates certain applications (FTP, VoIP)
   - Creates single point of failure
   - Impacts troubleshooting and monitoring

## 9. ACL Types

### Objective Answers:
1. **a) Source IP only** - Standard ACLs can only filter based on source IP address.

2. **b) 100-199** - Extended ACLs use numbers 100-199 and 2000-2699.

3. **b) Close to destination** - Standard ACLs should be placed close to the destination because they only filter on source IP.

### Subjective Answers:

1. **Standard vs. Extended ACLs:**
   
   **Standard ACLs:**
   - Filter only on source IP address
   - Number range: 1-99, 1300-1999
   - Limited filtering capability
   - Placement: Close to destination
   - Use case: Simple source-based filtering
   
   **Extended ACLs:**
   - Filter on source IP, destination IP, protocol, ports
   - Number range: 100-199, 2000-2699
   - Granular filtering capability
   - Placement: Close to source
   - Use case: Complex traffic filtering
   
   **Best Practice Placement:**
   - Standard ACLs near destination (avoid blocking too much)
   - Extended ACLs near source (block unwanted traffic early)

2. **Named vs. Numbered ACLs:**
   
   **Named ACLs:**
   - Use descriptive names instead of numbers
   - Allow editing individual entries
   - Support sequence numbers for ordering
   - Easier to manage and understand
   - Preferred in modern networks
   
   **Numbered ACLs:**
   - Use predefined number ranges
   - Must remove entire ACL to edit
   - Legacy approach
   - Still supported for backward compatibility
   
   **When to Use Each:**
   - Named ACLs: Complex environments, frequent changes
   - Numbered ACLs: Simple environments, legacy compatibility

## 10. ACL Configuration

### Objective Answers:
1. **a) in** - The "in" keyword applies an ACL to incoming traffic on an interface.

2. **b) Denied** - Traffic that doesn't match any ACL statement is denied by the implicit deny all at the end.

3. **c) no [sequence-number]** - In named ACLs, you can remove specific lines using their sequence numbers.

### Subjective Answers:

1. **Configuration Examples:**
   
   **Standard ACL Configuration:**
   ```
   ! Numbered Standard ACL
   access-list 10 permit 192.168.1.0 0.0.0.255
   access-list 10 deny any
   
   interface serial 0/0
   ip access-group 10 out
   
   ! Named Standard ACL
   ip access-list standard SALES_FILTER
   permit 192.168.10.0 0.0.0.255
   deny any
   
   interface fastethernet 0/1
   ip access-group SALES_FILTER in
   ```
   
   **Extended ACL Configuration:**
   ```
   ! Numbered Extended ACL
   access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
   access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
   access-list 100 deny ip any any
   
   interface fastethernet 0/0
   ip access-group 100 in
   
   ! Named Extended ACL
   ip access-list extended WEB_FILTER
   10 permit tcp 192.168.1.0 0.0.0.255 any eq www
   20 permit tcp 192.168.1.0 0.0.0.255 any eq 443
   30 deny ip any any log
   
   interface gigabitethernet 0/1
   ip access-group WEB_FILTER out
   ```

2. **ACL Processing and Troubleshooting:**
   
   **Processing Order:**
   - ACLs processed top-down
   - First match wins (no further processing)
   - Implicit deny all at the end
   - Empty ACL permits all traffic
   
   **Wildcard Masks:**
   - Inverse of subnet mask
   - 0 = must match exactly
   - 255 = ignore completely
   - Example: 0.0.0.255 matches any host in /24 subnet
   
   **Common Troubleshooting:**
   - Check ACL applied to correct interface and direction
   - Verify wildcard mask calculations
   - Review ACL order (specific to general)
   - Use log keyword to monitor matches
   - show access-lists command for hit counters

## 11. VTP & DTP

### Objective Answers:
1. **b) Server** - VTP Server mode can create, modify, and delete VLANs. Changes are synchronized to other switches.

2. **a) 30 seconds** - DTP frames are sent every 30 seconds to maintain trunk negotiations.

3. **a) access** - Access mode will never form a trunk; it's dedicated to a single VLAN.

### Subjective Answers:

1. **VTP Modes Explained:**
   
   **Server Mode:**
   - Can create, modify, delete VLANs
   - Synchronizes VLAN database to other switches
   - Stores VLAN information in NVRAM
   - Default mode on most Cisco switches
   
   **Client Mode:**
   - Cannot create, modify, delete VLANs
   - Receives VLAN information from servers
   - Does not store VLAN info in NVRAM
   - Forwards VTP advertisements
   
   **Transparent Mode:**
   - Can create, modify, delete VLANs locally
   - Does not participate in VTP domain
   - Forwards VTP advertisements unchanged
   - Stores VLAN information locally
   
   **Off Mode:**
   - Does not participate in VTP
   - Does not forward VTP advertisements
   - Functions like transparent but doesn't forward

2. **DTP and Security Considerations:**
   
   **DTP Negotiation:**
   - Dynamic Trunk Protocol negotiates trunk formation
   - Sends keepalive every 30 seconds
   - Can automatically form trunks between switches
   
   **Security Issues:**
   - VLAN hopping attacks possible
   - Unauthorized trunk formation
   - Unnecessary trunk links
   
   **Best Practices:**
   - Disable DTP on access ports: switchport nonegotiate
   - Manually configure trunk ports
   - Use VTP transparent or off mode
   - Implement proper VLAN security

## 12. VLANs

### Objective Answers:
1. **b) VLAN 1** - VLAN 1 is the default VLAN on Cisco switches and cannot be deleted.

2. **b) Untagged traffic only** - The native VLAN carries untagged traffic on trunk links.

3. **b) 1006-4094** - Extended VLANs use the range 1006-4094 on Cisco switches.

### Subjective Answers:

1. **VLAN Concepts and Significance:**
   
   **VLAN Benefits:**
   - Logical segmentation of broadcast domains
   - Enhanced security through isolation
   - Improved performance by reducing broadcast traffic
   - Flexible network design independent of physical layout
   - Cost reduction through shared infrastructure
   
   **Native VLAN:**
   - Handles untagged traffic on trunk links
   - Default is VLAN 1 but should be changed
   - Security risk if left as VLAN 1
   - Should be unused VLAN for security
   
   **Default VLAN:**
   - VLAN 1 is default on Cisco switches
   - All ports initially in VLAN 1
   - Cannot be deleted or modified
   - Should not be used for user traffic

2. **VLAN Types and Design:**
   
   **Data VLANs:**
   - Carry user-generated traffic
   - Separated by function, department, or security level
   - Example: VLAN 10 for Sales, VLAN 20 for Engineering
   
   **Voice VLANs:**
   - Dedicated to VoIP traffic
   - Priority handling for voice packets
   - Usually configured with data VLAN on same port
   
   **Management VLANs:**
   - Used for switch management traffic
   - Should be separate from user VLANs
   - Limited access for security
   
   **Best Practices:**
   - Change native VLAN from default
   - Use dedicated management VLAN
   - Implement VLAN pruning on trunks
   - Document VLAN assignments clearly

## 13. Routing Protocols & OSPF

### Objective Answers:
1. **b) Dijkstra** - OSPF uses Dijkstra's Shortest Path First algorithm to calculate best paths.

2. **b) Manual, Loopback, Highest IP** - Router ID selection order: manually configured, highest loopback IP, highest physical interface IP.

3. **a) 10 seconds** - OSPF Hello packets are sent every 10 seconds on broadcast and point-to-point networks.

### Subjective Answers:

1. **OSPF Operation:**
   
   **Link State Advertisement (LSA) Flooding:**
   - Each router advertises its links and their states
   - LSAs flooded throughout the area
   - All routers have identical link-state database
   - Provides complete network topology view
   
   **SPF Calculation:**
   - Each router runs Dijkstra algorithm
   - Calculates shortest path tree
   - Root of tree is the calculating router
   - Results in routing table entries
   
   **OSPF Areas:**
   - Hierarchical design reduces LSA flooding
   - Area 0 (backbone) connects all other areas
   - Reduces routing table size and convergence time
   - Provides scalability for large networks

2. **OSPF Neighbor Process and Router ID:**
   
   **Neighbor Establishment:**
   1. **Down State** - No Hello packets received
   2. **Init State** - Hello received but not bidirectional
   3. **2-Way State** - Bidirectional communication established
   4. **ExStart State** - Master/slave relationship determined
   5. **Exchange State** - Database Description packets exchanged
   6. **Loading State** - LSR and LSU packets exchanged
   7. **Full State** - Databases synchronized
   
   **Router ID Significance:**
   - Unique identifier for each OSPF router
   - Used in LSAs and SPF calculations
   - Determines DR/BDR election on broadcast networks
   - Must be unique within OSPF domain
   - Best practice: configure manually using loopback

## 14. STP

### Subjective Answers:

1. **STP Operation and Components:**
   
   **Spanning Tree Purpose:**
   - Prevents loops in redundant switched networks
   - Maintains single active path between network segments
   - Provides automatic failover when links fail
   - Uses Bridge Protocol Data Units (BPDUs) for communication
   
   **Root Bridge Selection:**
   - Bridge with lowest Bridge ID becomes root
   - Bridge ID = Bridge Priority + MAC Address
   - Default priority is 32768
   - All decisions made from root bridge perspective
   
   **Port Cost and Roles:**
   - **Root Port:** Best path to root bridge (lowest cost)
   - **Designated Port:** Best path from segment to root
   - **Blocked Port:** Alternate paths (prevents loops)
   - Cost based on link speed (Gigabit = 4, FastEthernet = 19)

2. **STP Variants Comparison:**
   
   **Original STP (802.1D):**
   - 50-second convergence time
   - Five port states: Disabled, Blocking, Listening, Learning, Forwarding
   - Timer-based operation
   - Single spanning tree for all VLANs
   
   **RSTP (802.1w):**
   - Sub-second convergence
   - Three port states: Discarding, Learning, Forwarding
   - Handshake-based operation
   - Backward compatible with STP
   
   **MST (802.1s):**
   - Multiple spanning tree instances
   - Maps multiple VLANs to instances
   - Reduces BPDU overhead
   - Optimal path utilization for different VLANs

3. **STP Port States and Roles:**
   
   **Port States (STP):**
   - **Disabled:** Administratively down
   - **Blocking:** Receives BPDUs, doesn't forward data
   - **Listening:** Processes BPDUs, determines role
   - **Learning:** Builds MAC table, doesn't forward data
   - **Forwarding:** Normal operation, forwards data
   
   **Port Roles:**
   - **Root Port:** One per non-root bridge, path to root
   - **Designated Port:** One per segment, forwards to segment
   - **Alternate Port:** Backup to root port
   - **Backup Port:** Backup to designated port

## 15. DHCP Spoofing

### Objective Answers:
1. **b) DHCP spoofing** - DHCP snooping specifically protects against DHCP spoofing attacks.

2. **c) Send and receive DHCP messages** - Trusted ports can both send and receive all DHCP message types.

3. **c) DHCP binding table** - DHCP snooping maintains a binding database of IP-to-MAC mappings.

### Subjective Answers:

1. **DHCP Spoofing Attack and Protection:**
   
   **DHCP Spoofing Attack:**
   - Attacker sets up rogue DHCP server
   - Responds to DHCP requests faster than legitimate server
   - Provides malicious network configuration
   - Can redirect traffic through attacker's system
   - Enables man-in-the-middle attacks
   
   **Attack Impact:**
   - Clients receive incorrect gateway/DNS settings
   - Traffic redirected through attacker
   - Possible data interception and modification
   - Network connectivity issues
   
   **DHCP Snooping Protection:**
   - Classifies switch ports as trusted or untrusted
   - Only trusted ports can send DHCP server messages
   - Untrusted ports can only send DHCP client messages
   - Maintains binding database for validation
   - Drops unauthorized DHCP responses

2. **DHCP Snooping Configuration:**
   
   **Basic Configuration:**
   ```
   ip dhcp snooping
   ip dhcp snooping vlan 1,10,20
   ip dhcp snooping information option
   
   interface gigabitethernet 0/1
   ip dhcp snooping trust
   
   interface range fastethernet 0/1-24
   ip dhcp snooping limit rate 10
   ```
   
   **Trusted vs. Untrusted Ports:**
   - **Trusted:** DHCP servers, uplink ports to servers
   - **Untrusted:** Client access ports, unknown devices
   
   **Rate Limiting:**
   - Prevents DHCP starvation attacks
   - Limits DHCP requests per second per port
   - Typical rate: 10-15 packets per second

## 16. BPDU Guard

### Objective Answers:
1. **b) Goes to err-disabled state** - When BPDU Guard is triggered, the port immediately goes to err-disabled state.

2. **b) Access ports** - BPDU Guard is typically enabled on access ports where end devices connect.

3. **b) spanning-tree portfast bpduguard default** - This command enables BPDU Guard globally on all PortFast-enabled ports.

### Subjective Answers:

1. **BPDU Guard Purpose and Implementation:**
   
   **Purpose:**
   - Prevents unauthorized switches from connecting to access ports
   - Protects against accidental STP topology changes
   - Enhances network security and stability
   - Works with PortFast to provide fast convergence
   
   **PortFast Relationship:**
   - PortFast bypasses STP learning states on access ports
   - BPDU Guard protects PortFast ports from receiving BPDUs
   - If BPDU received, assumption is that switch connected
   - Port immediately disabled to prevent topology changes
   
   **Implementation:**
   ```
   ! Global configuration
   spanning-tree portfast bpduguard default
   
   ! Interface-specific
   interface fastethernet 0/1
   spanning-tree portfast
   spanning-tree bpduguard enable
   
   ! Recovery
   errdisable recovery cause bpduguard
   errdisable recovery interval 300
   ```

2. **Security Implications:**
   
   **Unauthorized Switch Threats:**
   - STP topology manipulation
   - VLAN hopping through trunk formation
   - Network loops and broadcast storms
   - Bypass of security controls
   
   **BPDU Guard Mitigation:**
   - Immediate port shutdown on BPDU receipt
   - Prevents unauthorized topology participation
   - Alerts administrators to security violations
   - Forces manual intervention for recovery
   
   **Best Practices:**
   - Enable on all access ports
   - Combine with PortFast
   - Configure automatic recovery with appropriate interval
   - Monitor err-disabled ports regularly

## 17. Reading Routing Tables

### Objective Answers:
1. **b) Cost of the route** - The metric represents the cost or preference of the route according to the routing protocol.

2. **a) Static route (AD 1)** - Lower administrative distance means higher priority; static routes have AD of 1.

3. **b) Default static route** - "S*" indicates a static default route (0.0.0.0/0).

### Subjective Answers:

1. **Routing Table Interpretation:**
   
   **Route Entry Components:**
   ```
   R    192.168.1.0/24 [120/2] via 10.1.1.1, 00:00:23, Serial0/0
   ```
   - **R:** Route code (RIP in this case)
   - **192.168.1.0/24:** Destination network
   - **[120/2]:** [Administrative Distance/Metric]
   - **via 10.1.1.1:** Next-hop IP address
   - **00:00:23:** Time since last update
   - **Serial0/0:** Exit interface
   
   **Common Route Codes:**
   - C: Directly connected
   - S: Static route
   - R: RIP
   - D: EIGRP
   - O: OSPF
   - B: BGP
   - *: Default route

2. **Route Selection Process:**
   
   **Selection Criteria (in order):**
   1. **Longest Prefix Match:** Most specific route wins
   2. **Administrative Distance:** Lower AD preferred
   3. **Metric:** Lower metric preferred (within same protocol)
   4. **Load Balancing:** Multiple equal-cost paths
   
   **Administrative Distance Values:**
   - Directly Connected: 0
   - Static Route: 1
   - EIGRP: 90
   - OSPF: 110
   - RIP: 120
   - External EIGRP: 170
   
   **Example Scenario:**
   ```
   Route to 192.168.1.100:
   - 192.168.1.0/24 via OSPF (AD 110, Metric 10)
   - 192.168.0.0/16 via Static (AD 1, Metric 1)
   - 0.0.0.0/0 via Static (AD 1, Metric 1)
   
   Result: 192.168.1.0/24 chosen (longest prefix match)
   ```

## 18. Default Gateway

### Objective Answers:
1. **b) In the same subnet as the host** - The default gateway must be reachable directly (same subnet) by the host.

2. **b) Default gateway** - When a host doesn't know the destination network, it forwards traffic to its configured default gateway.

3. **c) 0.0.0.0/0** - The default route is represented as 0.0.0.0/0, matching all destinations.

### Subjective Answers:

1. **Default Gateway Concept:**
   
   **Definition and Role:**
   - Router interface in same subnet as host
   - Handles traffic destined for remote networks
   - Host's "way out" of local network segment
   - Must be directly reachable (Layer 2 adjacent)
   
   **Routing Decision Process:**
   1. Host checks if destination is local subnet
   2. If local: sends directly using ARP
   3. If remote: sends to default gateway
   4. Gateway routes packet toward destination
   
   **Configuration Examples:**
   ```
   ! Windows
   netsh interface ip set address "Local Area Connection" 
   static 192.168.1.100 255.255.255.0 192.168.1.1
   
   ! Cisco Router Interface
   interface fastethernet 0/0
   ip address 192.168.1.1 255.255.255.0
   ```

2. **Host vs. Router Default Configuration:**
   
   **Host Default Gateway:**
   - Single IP address of local router
   - Used for all non-local traffic
   - Configured via DHCP or static assignment
   - Essential for Internet connectivity
   
   **Router Default Route:**
   - Route to 0.0.0.0/0 (all networks)
   - Used when no specific route exists
   - Points to ISP or upstream router
   - Provides connectivity to unknown destinations
   
   **Relationship:**
   - Host's default gateway = Router's interface IP
   - Router's default route = Next-hop toward Internet
   - Creates hierarchical routing structure
   - Enables scalable Internet connectivity

## 19. CIDR Calculations

### Objective Answers:
1. **a) 62** - /26 has 6 host bits (32-26=6), so 2^6 - 2 = 62 usable host addresses.

2. **a) 255.255.255.240** - /28 means 28 network bits, leaving 4 host bits. Subnet mask: 255.255.255.240.

3. **b) 4** - /28 has 16 addresses, /30 has 4 addresses, so 16/4 = 4 subnets.

### Subjective Answers:

1. **CIDR Calculations Explained:**
   
   **Basic Formula:**
   - Network bits = CIDR value
   - Host bits = 32 - CIDR value
   - Subnet size = 2^(host bits)
   - Usable hosts = 2^(host bits) - 2
   
   **Example: 192.168.1.0/26**
   - Network bits: 26
   - Host bits: 32 - 26 = 6
   - Subnet size: 2^6 = 64 addresses
   - Usable hosts: 64 - 2 = 62
   - Network: 192.168.1.0
   - Broadcast: 192.168.1.63
   - Host range: 192.168.1.1 - 192.168.1.62
   
   **Subnet Mask Calculation:**
   - /26 = 11111111.11111111.11111111.11000000
   - Decimal: 255.255.255.192

2. **Subnetting Example:**
   
   **Scenario: Subnet 172.16.0.0/16 into /24 networks**
   
   **Original Network:**
   - Network: 172.16.0.0/16
   - Addresses: 172.16.0.0 - 172.16.255.255
   - Total hosts: 65,534
   
   **Subnetting to /24:**
   - Borrowed bits: 24 - 16 = 8 bits
   - Number of subnets: 2^8 = 256 subnets
   - Hosts per subnet: 2^8 - 2 = 254 hosts
   
   **Resulting Subnets:**
   - 172.16.0.0/24 (hosts: .1-.254)
   - 172.16.1.0/24 (hosts: .1-.254)
   - 172.16.2.0/24 (hosts: .1-.254)
   - ...
   - 172.16.255.0/24 (hosts: .1-.254)
   
   **VLSM Example:**
   - Sales dept: 50 hosts → /26 (62 hosts)
   - Engineering: 25 hosts → /27 (30 hosts)
   - Management: 10 hosts → /28 (14 hosts)

## 20. Telnet & SSH

### Objective Answers:
1. **b) 22** - SSH uses port 22 by default (Telnet uses port 23).

2. **c) Clear text** - Telnet traffic is transmitted in clear text without encryption.

3. **b) SSH v2 only** - SSH version 2 is considered secure; version 1 has known vulnerabilities.

### Subjective Answers:

1. **Telnet vs. SSH Comparison:**
   
   **Telnet:**
   - Port 23
   - Clear text transmission
   - No encryption or authentication
   - Vulnerable to eavesdropping
   - Simple protocol, low overhead
   - Legacy protocol, deprecated
   
   **SSH:**
   - Port 22
   - Encrypted transmission
   - Strong authentication methods
   - Secure against eavesdropping
   - Additional overhead for encryption
   - Modern standard for remote access
   
   **Security Implications:**
   - Telnet passwords visible in packet captures
   - SSH protects credentials and data
   - SSH prevents man-in-the-middle attacks
   - Telnet should never be used over untrusted networks

2. **SSH Configuration and Handshake:**
   
   **SSH Handshake Process:**
   1. **Protocol Version Exchange**
   2. **Algorithm Negotiation** (encryption, MAC, compression)
   3. **Key Exchange** (Diffie-Hellman)
   4. **Server Authentication** (host key verification)
   5. **User Authentication** (password, key, etc.)
   6. **Secure Channel Establishment**
   
   **Cisco SSH Configuration:**
   ```
   ! Generate RSA key pair
   crypto key generate rsa
   
   ! Configure SSH parameters
   ip ssh version 2
   ip ssh time-out 60
   ip ssh authentication-retries 3
   
   ! Configure user accounts
   username admin privilege 15 secret cisco123
   
   ! Configure VTY lines
   line vty 0 4
   transport input ssh
   login local
   exec-timeout 10 0
   
   ! Enable SSH
   ip domain-name company.com
   ```
   
   **Best Practices:**
   - Use SSH v2 only
   - Disable Telnet completely
   - Use strong authentication
   - Implement access control lists
   - Configure appropriate timeouts