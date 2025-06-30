# Basic to Intermediate Networking Practice Questions
**Comprehensive Study Guide with Clear Explanations**

---

## 1. ROUTER ID SELECTION - BASIC TO INTERMEDIATE

### 🔹 QUESTION 1A: Basic Router ID Selection
**Network Diagram:**
```
Router R1:
Physical Interfaces:
- Gi0/0: 10.1.1.1/24 (UP)
- Gi0/1: 172.16.1.1/24 (UP)
- Fa0/0: 192.168.1.1/24 (UP)

Loopback Interfaces:
- Lo0: 1.1.1.1/32 (UP)

Manual Configuration: NONE
```

**❓ Question:** What will be Router R1's OSPF Router ID?

**💡 Answer:** Router ID will be **1.1.1.1**

**📚 Explanation:**
Router ID selection follows this priority order:
1. **Manual configuration** (highest priority)
2. **Highest Loopback IP** (if no manual config)
3. **Highest Physical Interface IP** (if no loopbacks)

Since there's no manual configuration, but Lo0 (1.1.1.1) exists, it becomes the Router ID.

**🔧 Configuration:**
```cisco
R1(config)# interface loopback 0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# description "Router ID Interface"

R1(config)# router ospf 1
R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0

! Verify Router ID
R1# show ip ospf | include Router ID
```

---

### 🔹 QUESTION 1B: Intermediate Router ID Selection
**Network Diagram:**
```
Router R2:
Physical Interfaces:
- Gi0/0: 192.168.10.1/24 (UP)
- Gi0/1: 10.10.10.1/24 (DOWN)
- Fa0/0: 172.16.50.1/24 (UP)

Loopback Interfaces:
- Lo0: 2.2.2.2/32 (DOWN)
- Lo1: 3.3.3.3/32 (UP)
- Lo5: 1.1.1.1/32 (UP)

Manual Configuration: router-id 5.5.5.5
```

**❓ Question:** What will be Router R2's OSPF Router ID?

**💡 Answer:** Router ID will be **5.5.5.5**

**📚 Explanation:**
Even though multiple loopbacks exist (Lo1=3.3.3.3 and Lo5=1.1.1.1), and physical interfaces are available, the **manually configured Router ID always takes precedence**.

**🔧 Configuration:**
```cisco
R2(config)# router ospf 1
R2(config-router)# router-id 5.5.5.5
R2(config-router)# network 192.168.10.0 0.0.0.255 area 0

! Even if Lo1 (3.3.3.3) is highest active loopback,
! manual configuration overrides everything
R2# show ip ospf | include Router ID
 Router ID: 5.5.5.5
```

---

### 🔹 QUESTION 1C: Router ID Change Scenario
**Scenario:** Router boots up, then administrator adds configuration

**Initial State:**
```
Router R3:
- Gi0/0: 10.1.1.1/24 (UP) ← Current Router ID
- No loopbacks
- No manual config
```

**After Configuration Added:**
```cisco
R3(config)# interface loopback 0
R3(config-if)# ip address 99.99.99.99 255.255.255.255
R3(config)# router ospf 1
R3(config-router)# router-id 88.88.88.88
```

**❓ Question:** What is the Router ID before and after configuration?

**💡 Answer:**
- **Before:** 10.1.1.1 (highest physical interface)
- **After:** Still 10.1.1.1 (Router ID doesn't change automatically)

**📚 Explanation:**
OSPF Router ID **does NOT change automatically** once OSPF process starts. You must manually restart the OSPF process.

**🔧 Solution:**
```cisco
! Force Router ID change
R3# clear ip ospf process
Reset ALL OSPF processes? [no]: yes

! Now Router ID becomes 88.88.88.88
R3# show ip ospf | include Router ID
 Router ID: 88.88.88.88
```

---

## 2. COLLISION DOMAIN & BROADCAST DOMAIN - BASIC TO INTERMEDIATE

### 🔹 QUESTION 2A: Basic Domain Analysis
**Network Diagram:**
```
                [Router R1]
           Gi0/0 |        | Gi0/1
        +--------+        +--------+
        |                          |
   192.168.1.0/24               192.168.2.0/24
        |                          |
    [Switch SW1]                [Hub H1]
    12 ports                    4 ports
        |                          |
   [PC1][PC2][PC3]           [PC4][PC5][PC6][PC7]
```

**❓ Question:** Count collision domains and broadcast domains.

**💡 Answer:**
- **Collision Domains:** 6
- **Broadcast Domains:** 2

**📚 Detailed Analysis:**

**🔍 Collision Domains:**
1. Router R1 Gi0/0 interface = 1 domain
2. Router R1 Gi0/1 interface = 1 domain
3. Switch SW1 - PC1 port = 1 domain
4. Switch SW1 - PC2 port = 1 domain
5. Switch SW1 - PC3 port = 1 domain
6. Hub H1 - All connected devices = 1 domain (shared)

**🔍 Broadcast Domains:**
1. Left side: R1-Gi0/0, SW1, PC1, PC2, PC3 = 1 domain
2. Right side: R1-Gi0/1, Hub H1, PC4, PC5, PC6, PC7 = 1 domain

**💭 Key Points:**
- **Switch:** Each port = separate collision domain
- **Hub:** All ports = single collision domain
- **Router:** Separates broadcast domains

---

### 🔹 QUESTION 2B: Intermediate Domain Analysis with VLANs
**Network Diagram:**
```
                [Router R1]
                   Gi0/0
                     |
                [Switch SW1]
           Gi0/1  |  |  |  Gi0/24
              +---+  +  +---+
              |       |      |
           [Hub H1] [PC1]  [PC2]
           4 ports    |      |
              |    VLAN 10 VLAN 20
        [PC3][PC4][PC5]
```

**VLAN Configuration:**
```cisco
SW1(config)# vlan 10
SW1(config-vlan)# name Sales
SW1(config)# vlan 20
SW1(config-vlan)# name Engineering

SW1(config)# interface fastethernet 0/2
SW1(config-if)# switchport access vlan 10
SW1(config)# interface fastethernet 0/3
SW1(config-if)# switchport access vlan 20

SW1(config)# interface gigabitethernet 0/1
SW1(config-if)# switchport mode trunk
```

**❓ Question:** How many collision and broadcast domains exist?

**💡 Answer:**
- **Collision Domains:** 5
- **Broadcast Domains:** 4

**📚 Detailed Analysis:**

**🔍 Collision Domains:**
1. Router R1 Gi0/0 = 1 domain
2. Switch SW1 Fa0/2 (PC1) = 1 domain
3. Switch SW1 Fa0/3 (PC2) = 1 domain
4. Switch SW1 Gi0/1 (to Hub) = 1 domain
5. Hub H1 (PC3, PC4, PC5) = 1 domain

**🔍 Broadcast Domains:**
1. VLAN 1 (default) = 1 domain
2. VLAN 10 (Sales - PC1) = 1 domain
3. VLAN 20 (Engineering - PC2) = 1 domain
4. Hub segment = 1 domain

---

## 3. STP ROOT BRIDGE, ROOT PORT, DESIGNATED PORT - BASIC TO INTERMEDIATE

### 🔹 QUESTION 3A: Basic STP Analysis
**Network Diagram:**
```
     [SW1]                    [SW2]
  Priority: 32768          Priority: 32768
  MAC: 0001.1111.1111      MAC: 0002.2222.2222
       |                        |
    Gi0/1                    Gi0/1
       |                        |
       +----------+-------------+
                  |
               [SW3]
            Priority: 32768
            MAC: 0003.3333.3333
```

**Port Details:**
- All links are Gigabit Ethernet (Cost = 4)
- All switches have default priority (32768)

**❓ Question:** Identify Root Bridge, Root Ports, and Designated Ports.

**💡 Answer:**

**🏆 Root Bridge Selection:**
All have same priority (32768), so lowest MAC wins:
- SW1: 32768.0001.1111.1111 ✅ **ROOT BRIDGE**
- SW2: 32768.0002.2222.2222
- SW3: 32768.0003.3333.3333

**🔍 Root Ports (path to root bridge):**
- **SW2:** Gi0/1 (direct to SW1, cost=4)
- **SW3:** Gi0/1 (direct to SW1, cost=4)

**🔍 Designated Ports (forwarding on each segment):**
- **SW1:** Gi0/1 (to SW2) - Root bridge wins
- **SW1:** Gi0/2 (to SW3) - Root bridge wins
- **SW2:** No designated ports
- **SW3:** No designated ports

**🔧 Verification Commands:**
```cisco
SW1# show spanning-tree vlan 1
SW2# show spanning-tree interface gigabitethernet 0/1
SW3# show spanning-tree root
```

---

### 🔹 QUESTION 3B: Intermediate STP with Mixed Port Types
**Network Diagram:**
```
        [SW1 - Root]
     Priority: 4096
     MAC: 0001.1111.1111
           |
        Gi0/1 (Cost=4)
           |
        [SW2]
     Priority: 32768
     MAC: 0002.2222.2222
     /                 \
 Gi0/2(Cost=4)      Fa0/3(Cost=19)
   /                     \
[SW3]                   [SW4]
Priority: 32768         Priority: 32768
MAC: 0003.3333.3333     MAC: 0004.4444.4444
```

**❓ Question:** Complete the STP analysis.

**💡 Answer:**

**🏆 Root Bridge:** SW1 (Priority 4096 - lowest)

**🔍 Root Ports:**
- **SW2:** Gi0/1 (to SW1, cost=4)
- **SW3:** Gi0/2 (to SW2, total cost=8)
- **SW4:** Fa0/3 (to SW2, total cost=23)

**🔍 Designated Ports:**
- **SW1:** Gi0/1 (to SW2)
- **SW2:** Gi0/2 (to SW3) and Fa0/3 (to SW4)

**📚 Explanation:**
SW3 and SW4 both connect to SW2, but:
- SW3 via Gigabit (cost=4) → Total cost to root = 4+4 = 8
- SW4 via FastEthernet (cost=19) → Total cost to root = 4+19 = 23

**🔧 Configuration to Set SW1 as Root:**
```cisco
SW1(config)# spanning-tree vlan 1 priority 4096
! OR alternatively:
SW1(config)# spanning-tree vlan 1 root primary

! Verify
SW1# show spanning-tree vlan 1 brief
```

---

### 🔹 QUESTION 3C: STP Port States and Transitions
**Scenario:** New switch added to network

**❓ Question:** What are the STP port states and their purposes?

**💡 Answer:**

**🔄 STP Port States (802.1D):**

1. **🚫 DISABLED**
   - Administratively down
   - No BPDUs sent/received
   - No data forwarded

2. **🔒 BLOCKING**
   - Receives BPDUs only
   - Prevents loops
   - Duration: Until topology change

3. **👂 LISTENING**
   - Processes BPDUs
   - Determines port role
   - Duration: 15 seconds (Forward Delay)

4. **📚 LEARNING**
   - Builds MAC address table
   - Still no data forwarding
   - Duration: 15 seconds (Forward Delay)

5. **✅ FORWARDING**
   - Normal operation
   - Forwards data frames
   - Duration: Until topology change

**🔧 Example Configuration:**
```cisco
! View port states
SW1# show spanning-tree interface gigabitethernet 0/1 detail

! Manually set port to forwarding (PortFast)
SW1(config)# interface fastethernet 0/1
SW1(config-if)# spanning-tree portfast
```

---

## 4. ETHERCHANNEL, PAGP, LACP - BASIC TO INTERMEDIATE

### 🔹 QUESTION 4A: Basic EtherChannel Concepts
**❓ Question:** What is EtherChannel and why use it?

**💡 Answer:**

**🔗 EtherChannel Definition:**
EtherChannel bundles multiple physical links into one logical link.

**✅ Benefits:**
1. **Increased Bandwidth:** 2×1Gbps = 2Gbps total
2. **Redundancy:** If one link fails, others continue
3. **Load Balancing:** Traffic distributed across links
4. **STP Friendly:** Seen as single logical link

**📊 Example:**
```
Before EtherChannel:
[SW1] ----1Gbps---- [SW2]  (Single link, single point of failure)

After EtherChannel:
[SW1] ====2Gbps==== [SW2]  (Two 1Gbps links bundled)
  |                    |
Gi0/1                Gi0/1
Gi0/2                Gi0/2
```

---

### 🔹 QUESTION 4B: PAgP vs LACP Comparison
**❓ Question:** Compare PAgP and LACP protocols.

**💡 Answer:**

**📋 Protocol Comparison:**

| Feature | PAgP | LACP |
|---------|------|------|
| **Standard** | Cisco Proprietary | IEEE 802.3ad (Industry Standard) |
| **Compatibility** | Cisco devices only | Multi-vendor |
| **Modes** | desirable, auto, on | active, passive, on |
| **Max Links** | 8 active | 8 active |
| **Load Balancing** | Yes | Yes |

**🔧 PAgP Modes:**
- **desirable:** Actively negotiates
- **auto:** Passively waits for negotiation
- **on:** Forces channel (no negotiation)

**🔧 LACP Modes:**
- **active:** Actively negotiates
- **passive:** Passively waits for negotiation
- **on:** Forces channel (no negotiation)

---

### 🔹 QUESTION 4C: EtherChannel Configuration Example
**Network Diagram:**
```
[Switch SW1]                    [Switch SW2]
Gi0/1 ←------------------→ Gi0/1
Gi0/2 ←------------------→ Gi0/2
(Bundle these into EtherChannel)
```

**❓ Question:** Configure LACP EtherChannel between switches.

**💡 Answer:**

**🔧 SW1 Configuration (LACP Active):**
```cisco
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# description "LACP Bundle to SW2"
SW1(config-if-range)# switchport mode trunk
SW1(config-if-range)# switchport trunk allowed vlan 10,20,30
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# no shutdown

SW1(config)# interface port-channel 1
SW1(config-if)# description "LACP Bundle - 2x1Gbps"
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30
```

**🔧 SW2 Configuration (LACP Passive):**
```cisco
SW2(config)# interface range gigabitethernet 0/1-2
SW2(config-if-range)# description "LACP Bundle to SW1"
SW2(config-if-range)# switchport mode trunk
SW2(config-if-range)# switchport trunk allowed vlan 10,20,30
SW2(config-if-range)# channel-group 1 mode passive
SW2(config-if-range)# no shutdown

SW2(config)# interface port-channel 1
SW2(config-if)# description "LACP Bundle - 2x1Gbps"
SW2(config-if)# switchport mode trunk
SW2(config-if)# switchport trunk allowed vlan 10,20,30
```

**🔍 Verification Commands:**
```cisco
SW1# show etherchannel summary
SW1# show etherchannel 1 detail
SW1# show lacp neighbor
SW1# show interfaces port-channel 1
```

**📋 Expected Output:**
```
SW1# show etherchannel summary
Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)         LACP      Gi0/1(P)    Gi0/2(P)

Legend: S - Layer2    U - in use    P - bundled in port-channel
```

---

## 5. DYNAMIC ROUTING - BASIC TO INTERMEDIATE

### 🔹 QUESTION 5A: OSPF Basic Configuration
**Network Diagram:**
```
[R1]                    [R2]                    [R3]
Gi0/0                   Gi0/0  Gi0/1            Gi0/0
.1 |                    .2 |    | .1            | .2
   |                       |    |               |
   +-------10.1.1.0/24----+    +--10.1.2.0/24--+
   
R1 LAN: 192.168.1.0/24
R2 LAN: 192.168.2.0/24  
R3 LAN: 192.168.3.0/24
```

**❓ Question:** Configure OSPF on all routers for full connectivity.

**💡 Answer:**

**🔧 R1 Configuration:**
```cisco
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0

R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# no shutdown

R1(config)# interface gigabitethernet 0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
```

**🔧 R2 Configuration:**
```cisco
R2(config)# router ospf 1
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 10.1.1.0 0.0.0.255 area 0
R2(config-router)# network 10.1.2.0 0.0.0.255 area 0
R2(config-router)# network 192.168.2.0 0.0.0.255 area 0

R2(config)# interface gigabitethernet 0/0
R2(config-if)# ip address 10.1.1.2 255.255.255.0
R2(config-if)# no shutdown

R2(config)# interface gigabitethernet 0/1
R2(config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# no shutdown

R2(config)# interface gigabitethernet 0/2
R2(config-if)# ip address 192.168.2.1 255.255.255.0
R2(config-if)# no shutdown
```

**🔧 R3 Configuration:**
```cisco
R3(config)# router ospf 1
R3(config-router)# router-id 3.3.3.3
R3(config-router)# network 10.1.2.0 0.0.0.255 area 0
R3(config-router)# network 192.168.3.0 0.0.0.255 area 0

R3(config)# interface gigabitethernet 0/0
R3(config-if)# ip address 10.1.2.2 255.255.255.0
R3(config-if)# no shutdown

R3(config)# interface gigabitethernet 0/1
R3(config-if)# ip address 192.168.3.1 255.255.255.0
R3(config-if)# no shutdown
```

**🔍 Verification:**
```cisco
R1# show ip ospf neighbor
R1# show ip route ospf
R1# ping 192.168.3.1 source 192.168.1.1
```

---

### 🔹 QUESTION 5B: EIGRP Basic Configuration
**Same Network Diagram as 5A**

**❓ Question:** Configure EIGRP instead of OSPF.

**💡 Answer:**

**🔧 R1 Configuration:**
```cisco
R1(config)# router eigrp 100
R1(config-router)# eigrp router-id 1.1.1.1
R1(config-router)# network 10.1.1.0 0.0.0.255
R1(config-router)# network 192.168.1.0 0.0.0.255
R1(config-router)# no auto-summary
```

**🔧 R2 Configuration:**
```cisco
R2(config)# router eigrp 100
R2(config-router)# eigrp router-id 2.2.2.2
R2(config-router)# network 10.1.1.0 0.0.0.255
R2(config-router)# network 10.1.2.0 0.0.0.255
R2(config-router)# network 192.168.2.0 0.0.0.255
R2(config-router)# no auto-summary
```

**🔧 R3 Configuration:**
```cisco
R3(config)# router eigrp 100
R3(config-router)# eigrp router-id 3.3.3.3
R3(config-router)# network 10.1.2.0 0.0.0.255
R3(config-router)# network 192.168.3.0 0.0.0.255
R3(config-router)# no auto-summary
```

**📊 OSPF vs EIGRP Comparison:**

| Feature | OSPF | EIGRP |
|---------|------|-------|
| **Type** | Link State | Advanced Distance Vector |
| **Standard** | Open Standard | Cisco Proprietary |
| **Metric** | Cost (Bandwidth) | Bandwidth + Delay |
| **Areas** | Hierarchical | Flat (with summarization) |
| **Convergence** | Good | Excellent |

---

## 6. SUBNET MASKS AND CIDR - BASIC TO INTERMEDIATE

### 🔹 QUESTION 6A: Basic CIDR Calculations
**❓ Question:** Calculate network details for 192.168.1.0/26

**💡 Answer:**

**🔢 Calculation Steps:**

**Step 1: Understand /26**
- /26 means 26 network bits, 6 host bits
- Network bits: 11111111.11111111.11111111.11000000
- Subnet mask: 255.255.255.192

**Step 2: Calculate addresses**
- Total addresses: 2^6 = 64
- Usable hosts: 64 - 2 = 62 (subtract network and broadcast)

**Step 3: Network details**
- **Network Address:** 192.168.1.0
- **First Host:** 192.168.1.1
- **Last Host:** 192.168.1.62
- **Broadcast:** 192.168.1.63
- **Next Network:** 192.168.1.64

**📋 Summary Table:**
```
Network: 192.168.1.0/26
Subnet Mask: 255.255.255.192
Host Range: 192.168.1.1 - 192.168.1.62
Broadcast: 192.168.1.63
Total Hosts: 62
```

---

### 🔹 QUESTION 6B: Subnetting Practice
**❓ Question:** Subnet 172.16.0.0/16 for the following requirements:
- Department A: 1000 hosts
- Department B: 500 hosts  
- Department C: 250 hosts
- Point-to-Point links: 6 links (2 hosts each)

**💡 Answer:**

**🔢 Step 1: Determine subnet sizes**

**Department A (1000 hosts):**
- Need: 2^10 = 1024 addresses
- Subnet: /22 (gives 1022 usable hosts)

**Department B (500 hosts):**
- Need: 2^9 = 512 addresses
- Subnet: /23 (gives 510 usable hosts)

**Department C (250 hosts):**
- Need: 2^8 = 256 addresses
- Subnet: /24 (gives 254 usable hosts)

**Point-to-Point links (2 hosts each):**
- Need: 2^2 = 4 addresses per link
- Subnet: /30 (gives 2 usable hosts per link)

**🎯 Step 2: Allocate subnets (largest first)**

```
Original: 172.16.0.0/16

1. Department A (/22):
   Network: 172.16.0.0/22
   Range: 172.16.0.1 - 172.16.3.254
   Hosts: 1022

2. Department B (/23):
   Network: 172.16.4.0/23
   Range: 172.16.4.1 - 172.16.5.254
   Hosts: 510

3. Department C (/24):
   Network: 172.16.6.0/24
   Range: 172.16.6.1 - 172.16.6.254
   Hosts: 254

4. P2P Links (/30):
   Link 1: 172.16.7.0/30  (.1 and .2 usable)
   Link 2: 172.16.7.4/30  (.5 and .6 usable)
   Link 3: 172.16.7.8/30  (.9 and .10 usable)
   Link 4: 172.16.7.12/30 (.13 and .14 usable)
   Link 5: 172.16.7.16/30 (.17 and .18 usable)
   Link 6: 172.16.7.20/30 (.21 and .22 usable)
```

**🔧 Router Configuration Example:**
```cisco
! Department A VLAN interface
Router(config)# interface gigabitethernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 172.16.0.1 255.255.252.0
Router(config-subif)# description "Department A - 1000 hosts"

! Department B VLAN interface
Router(config)# interface gigabitethernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 172.16.4.1 255.255.254.0
Router(config-subif)# description "Department B - 500 hosts"

! Point-to-Point WAN link
Router(config)# interface serial 0/0/0
Router(config-if)# ip address 172.16.7.1 255.255.255.252
Router(config-if)# description "P2P Link to Branch Office"
```

---

### 🔹 QUESTION 6C: VLSM (Variable Length Subnet Mask)
**❓ Question:** What is VLSM and why use it?

**💡 Answer:**

**📚 VLSM Definition:**
Variable Length Subnet Mask allows using different subnet mask lengths within the same network to optimize address space.

**❌ Without VLSM (Fixed Length):**
```
Using /24 for everything:
- Department A: 172.16.1.0/24 (254 hosts) - WASTEFUL for 1000 hosts needed
- Department B: 172.16.2.0/24 (254 hosts) - WASTEFUL for 500 hosts needed
- P2P Link: 172.16.3.0/24 (254 hosts) - VERY WASTEFUL for 2 hosts needed
```

**✅ With VLSM (Variable Length):**
```
Optimized allocation:
- Department A: 172.16.0.0/22 (1022 hosts) - RIGHT SIZE
- Department B: 172.16.4.0/23 (510 hosts) - RIGHT SIZE  
- P2P Link: 172.16.6.0/30 (2 hosts) - PERFECT SIZE
```

**📊 Benefits:**
1. **Efficient address utilization**
2. **Reduced routing table size**
3. **Better network design**
4. **Scalability**

**🔧 Summary Commands:**
```cisco
! View routing table
Router# show ip route

! View interface summaries
Router# show ip interface brief

! Verify subnet calculations
Router# show ip route | include /
```

---

## 📚 STUDY TIPS AND KEY TAKEAWAYS

### 🎯 Router ID Priority Order:
1. Manual configuration (router-id command)
2. Highest active loopback interface IP
3. Highest active physical interface IP

### 🔗 Domain Rules:
- **Collision Domain:** Segment where collisions can occur
- **Broadcast Domain:** Segment where broadcasts are received
- **Hubs:** Share collision domains
- **Switches:** Separate collision domains
- **Routers:** Separate broadcast domains

### 🌳 STP Priority Order:
1. Lowest Bridge Priority
2. Lowest MAC address (if priorities equal)
3. Root ports have lowest cost to root bridge
4. Designated ports are chosen per segment

### 📦 EtherChannel Modes:
- **PAgP:** desirable (active) + auto (passive)
- **LACP:** active (sends) + passive (waits)
- **On:** Forces bundling (no negotiation)

### 🔢 Subnetting Formula:
- **Hosts needed:** 2^(host bits) - 2
- **Networks available:** 2^(borrowed bits)
- **Subnet mask:** Original + borrowed bits

This comprehensive guide covers all the essential networking concepts from basic to intermediate level with clear explanations and practical examples!