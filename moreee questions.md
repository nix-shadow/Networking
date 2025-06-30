# Additional Networking Practice Questions
**Extended Study Guide - Basic to Intermediate Level**

---

## 7. ADVANCED ROUTER ID SCENARIOS

### 🔹 QUESTION 7A: Router ID with Interface Flapping
**Network Scenario:**
```
Router R1:
Boot Sequence Timeline:
  T=0s: Router starts, only Gi0/0 is UP
  T=30s: OSPF process starts 
  T=60s: Loopback0 configured and comes UP
  T=90s: Gi0/0 goes DOWN due to cable issue
  T=120s: Gi0/0 comes back UP

Interfaces:
- Gi0/0: 10.1.1.1/24 (UP→DOWN→UP)
- Gi0/1: 192.168.1.1/24 (Always DOWN)
- Lo0: 99.99.99.99/32 (Added at T=60s)
```

**❓ Question:** What is the Router ID at each time interval?

**💡 Answer:**
- **T=30s (OSPF starts):** 10.1.1.1 (only active interface)
- **T=60s (Loopback added):** Still 10.1.1.1 (Router ID doesn't auto-change)
- **T=90s (Gi0/0 fails):** Still 10.1.1.1 (Router ID persists)
- **T=120s (Gi0/0 recovers):** Still 10.1.1.1 (Router ID unchanged)

**📚 Key Learning:** Router ID is "sticky" - once set, it doesn't change automatically even if higher priority interfaces become available.

**🔧 To Force Router ID Update:**
```cisco
R1# clear ip ospf process
Reset ALL OSPF processes? [no]: yes

! Now Router ID becomes 99.99.99.99
R1# show ip ospf | include Router ID
Router ID: 99.99.99.99
```

---

### 🔹 QUESTION 7B: Multiple Loopbacks Priority
**Network Scenario:**
```
Router R2:
Loopback Interfaces:
- Lo0: 1.1.1.1/32 (UP)
- Lo1: 5.5.5.5/32 (UP) 
- Lo10: 10.10.10.10/32 (UP)
- Lo100: 2.2.2.2/32 (DOWN)

Physical Interfaces:
- Gi0/0: 172.16.1.1/24 (UP)
- Gi0/1: 192.168.1.1/24 (UP)

Manual Configuration: NONE
```

**❓ Question:** Which interface IP becomes the Router ID and why?

**💡 Answer:** **10.10.10.10** (from Lo10)

**📚 Explanation:**
When multiple loopbacks exist, OSPF selects the **highest IP address** among active loopbacks:
- Lo0: 1.1.1.1 ❌
- Lo1: 5.5.5.5 ❌  
- Lo10: 10.10.10.10 ✅ **HIGHEST**
- Lo100: 2.2.2.2 ❌ (Interface DOWN)

**🔧 Configuration Check:**
```cisco
R2# show ip ospf | include Router ID
Router ID: 10.10.10.10

R2# show ip interface brief | include Loopback
Lo0    1.1.1.1     YES manual up     up
Lo1    5.5.5.5     YES manual up     up  
Lo10   10.10.10.10 YES manual up     up
Lo100  2.2.2.2     YES manual administratively down down
```

---

## 8. COMPLEX COLLISION & BROADCAST DOMAIN SCENARIOS

### 🔹 QUESTION 8A: Mixed Network Topology
**Network Diagram:**
```
                    [Router R1]
               Gi0/0 |        | Gi0/1
          192.168.1.1|        |192.168.2.1
                     |        |
            [Switch SW1]    [Switch SW2]
            24 ports        24 ports
                |              |
        +-------+-------+      +-------+-------+
        |       |       |      |       |       |
     [Hub1]   [PC1]   [PC2]  [Hub2]   [PC3]   [PC4]
     4-port     |       |    8-port     |       |
        |       |       |       |       |       |
   [PC5-PC8] [VLAN10][VLAN20] [PC9-PC16][VLAN30][VLAN40]
```

**VLAN Configuration:**
```cisco
SW1(config)# vlan 10
SW1(config)# vlan 20
SW1(config)# interface fa0/2
SW1(config-if)# switchport access vlan 10
SW1(config)# interface fa0/3  
SW1(config-if)# switchport access vlan 20

SW2(config)# vlan 30
SW2(config)# vlan 40
SW2(config)# interface fa0/3
SW2(config-if)# switchport access vlan 30
SW2(config)# interface fa0/4
SW2(config-if)# switchport access vlan 40
```

**❓ Question:** Calculate total collision domains and broadcast domains.

**💡 Answer:**
- **Collision Domains:** 12
- **Broadcast Domains:** 8

**🔍 Detailed Breakdown:**

**Collision Domains Analysis:**
1. Router R1 Gi0/0 interface = 1
2. Router R1 Gi0/1 interface = 1
3. SW1 to Hub1 connection = 1
4. Hub1 shared (PC5-PC8) = 1 domain total
5. SW1 to PC1 (VLAN 10) = 1
6. SW1 to PC2 (VLAN 20) = 1
7. SW2 to Hub2 connection = 1
8. Hub2 shared (PC9-PC16) = 1 domain total
9. SW2 to PC3 (VLAN 30) = 1
10. SW2 to PC4 (VLAN 40) = 1
11. SW1 remaining ports = 2 additional active domains
12. SW2 remaining ports = 1 additional active domain

**Broadcast Domains Analysis:**
1. SW1 Default VLAN 1 (Hub1, other devices) = 1
2. SW1 VLAN 10 (PC1) = 1  
3. SW1 VLAN 20 (PC2) = 1
4. SW2 Default VLAN 1 (Hub2, other devices) = 1
5. SW2 VLAN 30 (PC3) = 1
6. SW2 VLAN 40 (PC4) = 1
7. Router R1 left side network = 1 (total for left side)
8. Router R1 right side network = 1 (total for right side)

**Wait! Correction - More accurate count:**
- **Broadcast Domains:** 6 (VLAN 1 left, VLAN 10, VLAN 20, VLAN 1 right, VLAN 30, VLAN 40)

---

### 🔹 QUESTION 8B: Performance Impact Scenario
**Problem Statement:**
Network experiencing performance issues. Current setup:

```
Hub Network Segment:
- 1 Hub with 20 connected devices
- 100Mbps shared bandwidth
- Collision rate: 25%
- Average utilization: 80%

Switch Network Segment:  
- 1 Switch with 20 connected devices
- 100Mbps per port
- Collision rate: 0%
- Average utilization: 40% per port
```

**❓ Question:** Calculate effective throughput per device and recommend improvements.

**💡 Answer:**

**📊 Current Performance:**

**Hub Segment:**
- Shared bandwidth: 100Mbps
- Collision overhead: 25%
- Effective bandwidth: 100Mbps × (1 - 0.25) = 75Mbps
- Per device: 75Mbps ÷ 20 = **3.75Mbps per device**

**Switch Segment:**
- Dedicated bandwidth: 100Mbps per port
- No collisions: 0% overhead
- Per device: **100Mbps per device**

**🚀 Improvement Recommendations:**

1. **Replace Hub with Switch**
   ```
   Before: 3.75Mbps per device
   After:  100Mbps per device
   Improvement: 26.7x performance increase
   ```

2. **Upgrade to Gigabit Switch**
   ```
   Current: 100Mbps per device
   After:   1000Mbps per device  
   Improvement: 10x performance increase
   ```

3. **Implement VLANs for traffic segmentation**
   ```cisco
   ! Separate departments into VLANs
   SW1(config)# vlan 10
   SW1(config-vlan)# name Engineering
   SW1(config)# vlan 20  
   SW1(config-vlan)# name Sales
   
   ! Reduce broadcast domain size
   ! Improve security isolation
   ```

---

## 9. ADVANCED STP SCENARIOS

### 🔹 QUESTION 9A: STP with Mixed Link Speeds
**Network Diagram:**
```
                [SW1]
             Priority: 8192
             MAC: 0001.1111.1111
                 |
           Gi0/1 (Cost=4)
                 |
             [SW2]  
          Priority: 16384
          MAC: 0002.2222.2222
            /              \
     Gi0/2 /                \ Fa0/3
    (Cost=4)              (Cost=19)
         /                    \
    [SW3]                    [SW4]
 Priority: 32768          Priority: 32768
 MAC: 0003.3333.3333      MAC: 0004.4444.4444
        |                      |
    Gi0/1(Cost=4)          Fa0/1(Cost=19)
        |                      |
       [PC1]                 [PC2]
```

**❓ Question:** Determine complete STP topology including all port roles.

**💡 Answer:**

**🏆 Root Bridge Selection:**
- SW1: 8192.0001.1111.1111 ✅ **ROOT**
- SW2: 16384.0002.2222.2222
- SW3: 32768.0003.3333.3333  
- SW4: 32768.0004.4444.4444

**🔍 Root Port Selection:**

**SW2 Root Port:**
- Path to SW1: Direct via Gi0/1, Cost = 4
- **SW2 Root Port: Gi0/1**

**SW3 Root Port:**
- Path to SW1: SW3→SW2→SW1 = 4 + 4 = 8
- **SW3 Root Port: Gi0/2**

**SW4 Root Port:**  
- Path to SW1: SW4→SW2→SW1 = 19 + 4 = 23
- **SW4 Root Port: Fa0/3**

**🔍 Designated Port Selection:**

**SW1 (Root Bridge):**
- **Gi0/1: Designated Port** (all root bridge ports are DP)

**SW2:**
- **Gi0/2: Designated Port** (to SW3)
- **Fa0/3: Designated Port** (to SW4)

**🚫 Blocked Ports:** None in this topology (tree structure)

**🔧 Verification Commands:**
```cisco
SW1# show spanning-tree vlan 1 brief
SW2# show spanning-tree interface gigabitethernet 0/2
SW3# show spanning-tree root
SW4# show spanning-tree interface fastethernet 0/3
```

---

### 🔹 QUESTION 9B: STP Convergence Timing
**Scenario:** Link failure and convergence

**Initial Topology:**
```
[SW1-Root] ←→ [SW2] ←→ [SW3]
     ↑                    ↓
     +← ← ← ← ← ← ← ← ← ← +
   (Backup path - BLOCKED)
```

**❓ Question:** When the SW1↔SW2 link fails, how long does convergence take?

**💡 Answer:**

**⏱️ Traditional STP (802.1D) Convergence:**

1. **Failure Detection:** 0-20 seconds
   - Max Age timer expires (20 seconds)
   - Or immediate if physical link down

2. **Listening State:** 15 seconds
   - Port unblocks and starts listening
   - Forward Delay timer

3. **Learning State:** 15 seconds  
   - Builds MAC address table
   - Forward Delay timer

4. **Forwarding State:** Ready
   - Normal operation begins

**Total Convergence Time: 30-50 seconds**

**🚀 Rapid-PVST+ (802.1w) Convergence:**
```cisco
! Enable Rapid-PVST+ for faster convergence
SW1(config)# spanning-tree mode rapid-pvst
SW2(config)# spanning-tree mode rapid-pvst
SW3(config)# spanning-tree mode rapid-pvst

! Convergence Time: 1-6 seconds (much faster!)
```

**📊 Comparison Table:**
| STP Version | Convergence Time | States |
|-------------|------------------|--------|
| **802.1D (STP)** | 30-50 seconds | 5 states |
| **802.1w (RSTP)** | 1-6 seconds | 3 states |
| **MST** | 1-6 seconds | 3 states |

---

## 10. ETHERCHANNEL TROUBLESHOOTING SCENARIOS

### 🔹 QUESTION 10A: EtherChannel Not Forming
**Problem Scenario:**
```cisco
SW1 Configuration:
interface range gigabitethernet 0/1-2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 channel-group 1 mode desirable
 
SW2 Configuration:  
interface range gigabitethernet 0/1-2
 switchport mode access
 switchport access vlan 10
 channel-group 1 mode auto
```

**❓ Question:** Why won't the EtherChannel form? List all issues.

**💡 Answer:**

**🚨 Configuration Issues:**

1. **Trunk/Access Mismatch:**
   - SW1: trunk mode
   - SW2: access mode
   - **Fix:** Both must be trunk or both access

2. **VLAN Configuration Mismatch:**
   - SW1: allowed vlans 10,20
   - SW2: access vlan 10 only
   - **Fix:** Match VLAN configuration

3. **PAgP Mode Mismatch:**
   - SW1: desirable (active)
   - SW2: auto (passive)  
   - **Status:** This actually works! desirable + auto = OK

**🔧 Corrected Configuration:**

**Option 1: Both Trunk**
```cisco
! SW1 (no change needed)
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# switchport mode trunk
SW1(config-if-range)# switchport trunk allowed vlan 10,20
SW1(config-if-range)# channel-group 1 mode desirable

! SW2 (fix configuration)
SW2(config)# interface range gigabitethernet 0/1-2
SW2(config-if-range)# switchport mode trunk
SW2(config-if-range)# switchport trunk allowed vlan 10,20
SW2(config-if-range)# channel-group 1 mode auto
```

**Option 2: Both Access (same VLAN)**
```cisco
! SW1 (change to access)
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 10
SW1(config-if-range)# channel-group 1 mode desirable

! SW2 (no change needed)
SW2(config)# interface range gigabitethernet 0/1-2  
SW2(config-if-range)# switchport mode access
SW2(config-if-range)# switchport access vlan 10
SW2(config-if-range)# channel-group 1 mode auto
```

---

### 🔹 QUESTION 10B: LACP vs PAgP Interoperability
**Network Scenario:**
```
[Cisco SW1] ←→ [HP ProCurve SW2]
```

**Current Configuration:**
```cisco
! Cisco SW1
interface range gigabitethernet 0/1-2
 channel-group 1 mode desirable

! HP ProCurve SW2  
trunk 1-2 trk1 lacp
```

**❓ Question:** Will this EtherChannel work? If not, how to fix it?

**💡 Answer:**

**❌ This will NOT work!**

**🚨 Problem:** Protocol mismatch
- Cisco SW1: PAgP (desirable mode)
- HP SW2: LACP
- **These protocols cannot negotiate with each other**

**🔧 Solutions:**

**Option 1: Use LACP on both (Recommended)**
```cisco
! Cisco SW1 - Change to LACP
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# channel-group 1 mode active

! HP SW2 - Keep LACP (no change)
! trunk 1-2 trk1 lacp
```

**Option 2: Use "On" mode (No negotiation)**
```cisco
! Cisco SW1 - Force bundling
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# channel-group 1 mode on

! HP SW2 - Force bundling  
trunk 1-2 trk1
```

**📋 EtherChannel Mode Compatibility:**

| SW1 Mode | SW2 Mode | Result |
|----------|----------|--------|
| active | active | ✅ Forms |
| active | passive | ✅ Forms |
| passive | passive | ❌ No negotiation |
| desirable | auto | ✅ Forms |
| desirable | desirable | ⚠️ May conflict |
| on | on | ✅ Forms |
| active | desirable | ❌ Different protocols |

---

## 11. DYNAMIC ROUTING ADVANCED SCENARIOS

### 🔹 QUESTION 11A: OSPF Area Design
**Network Requirements:**
```
Company Network Design:
- Headquarters: 500 users
- Branch A: 100 users  
- Branch B: 50 users
- Branch C: 25 users
- WAN Links: Point-to-point T1 lines
- Requirement: Minimize routing updates to branches
```

**❓ Question:** Design OSPF area structure and explain benefits.

**💡 Answer:**

**🏗️ Recommended OSPF Design:**

```
                [HQ - ABR]
                  Area 0
               (Backbone)
                    |
        +-----------+-----------+
        |           |           |
   [Branch A]  [Branch B]  [Branch C]
    Area 1      Area 2      Area 3
   (Stub)      (Stub)      (Stub)
```

**🔧 Configuration:**

**Headquarters (Area Border Router):**
```cisco
HQ-R1(config)# router ospf 1
HQ-R1(config-router)# router-id 1.1.1.1
HQ-R1(config-router)# network 10.0.0.0 0.255.255.255 area 0
HQ-R1(config-router)# area 1 stub
HQ-R1(config-router)# area 2 stub  
HQ-R1(config-router)# area 3 stub

! Summarize each branch network
HQ-R1(config-router)# area 1 range 192.168.1.0 255.255.255.0
HQ-R1(config-router)# area 2 range 192.168.2.0 255.255.255.0
HQ-R1(config-router)# area 3 range 192.168.3.0 255.255.255.0
```

**Branch A Router:**
```cisco
BR-A-R1(config)# router ospf 1
BR-A-R1(config-router)# router-id 2.2.2.2
BR-A-R1(config-router)# network 10.1.1.0 0.0.0.3 area 0
BR-A-R1(config-router)# network 192.168.1.0 0.0.0.255 area 1
BR-A-R1(config-router)# area 1 stub
```

**✅ Benefits of Stub Areas:**
1. **Reduced LSA flooding** - No external LSAs
2. **Smaller routing tables** - Default route only
3. **Faster convergence** - Less SPF calculations
4. **Lower memory usage** - Fewer OSPF entries

---

### 🔹 QUESTION 11B: EIGRP Load Balancing
**Network Diagram:**
```
[R1] ──────────── [R3]
  |               |
  |    Primary    |
  |   Path Cost   |
  |      30       |
  |               |
[R2] ──────────── [R4]
      Backup
     Path Cost
        90

Destination: 10.1.1.0/24 behind R4
```

**❓ Question:** Configure EIGRP unequal cost load balancing.

**💡 Answer:**

**🔧 EIGRP Configuration:**

**Router R1:**
```cisco
R1(config)# router eigrp 100
R1(config-router)# network 10.0.0.0 0.255.255.255
R1(config-router)# variance 3
R1(config-router)# maximum-paths 4

! Variance calculation:
! Primary path cost: 30
! Backup path cost: 90  
! Ratio: 90/30 = 3
! Variance must be ≥ 3 to include backup path
```

**📊 Load Balancing Behavior:**
```
Traffic Distribution:
- Primary path (cost 30): 75% of traffic
- Backup path (cost 90): 25% of traffic

Calculation:
- Primary ratio: 90/30 = 3 parts
- Backup ratio: 90/90 = 1 part
- Total: 4 parts
- Primary: 3/4 = 75%
- Backup: 1/4 = 25%
```

**🔍 Verification:**
```cisco
R1# show ip eigrp topology 10.1.1.0/24
P 10.1.1.0/24, 2 successors, FD is 30
        via 10.0.1.2 (30/5), GigabitEthernet0/0
        via 10.0.2.2 (90/15), GigabitEthernet0/1

R1# show ip route 10.1.1.0
Routing entry for 10.1.1.0/24
  Known via "eigrp 100", distance 90, metric 30
  Redistributing via eigrp 100
  Last update from 10.0.2.2 on GigabitEthernet0/1, 00:01:23 ago
  Routing Descriptor Blocks:
  * 10.0.1.2, from 10.0.1.2, 00:01:23 ago, via GigabitEthernet0/0
      Route metric is 30, traffic share count is 3
    10.0.2.2, from 10.0.2.2, 00:01:23 ago, via GigabitEthernet0/1  
      Route metric is 90, traffic share count is 1
```

---

## 12. ADVANCED SUBNETTING SCENARIOS

### 🔹 QUESTION 12A: Complex VLSM Design
**Organization Requirements:**
```
TechCorp Network Plan:
Given: 10.0.0.0/8

Departments:
- Data Center: 2000 servers
- Engineering: 800 users
- Sales: 600 users  
- Marketing: 400 users
- Guest WiFi: 200 users
- Management: 50 users
- Point-to-Point WAN: 20 links
- Future expansion: 50% growth
```

**❓ Question:** Design complete VLSM scheme with 50% growth factor.

**💡 Answer:**

**📊 Requirements with Growth Factor:**
```
Data Center: 2000 × 1.5 = 3000 hosts
Engineering: 800 × 1.5 = 1200 hosts  
Sales: 600 × 1.5 = 900 hosts
Marketing: 400 × 1.5 = 600 hosts
Guest WiFi: 200 × 1.5 = 300 hosts
Management: 50 × 1.5 = 75 hosts
WAN Links: 20 × 2 hosts = 40 hosts
```

**🎯 Subnet Size Calculation:**
```
Data Center: 3000 hosts → /20 (4094 hosts)
Engineering: 1200 hosts → /21 (2046 hosts)
Sales: 900 hosts → /22 (1022 hosts)
Marketing: 600 hosts → /22 (1022 hosts)  
Guest WiFi: 300 hosts → /23 (510 hosts)
Management: 75 hosts → /25 (126 hosts)
WAN Links: 2 hosts each → /30 (2 hosts each)
```

**🔢 Subnet Allocation (Largest First):**
```
Network: 10.0.0.0/8

1. Data Center (/20):
   Network: 10.0.0.0/20
   Range: 10.0.0.1 - 10.0.15.254
   Hosts: 4094

2. Engineering (/21):  
   Network: 10.0.16.0/21
   Range: 10.0.16.1 - 10.0.23.254
   Hosts: 2046

3. Sales (/22):
   Network: 10.0.24.0/22
   Range: 10.0.24.1 - 10.0.27.254  
   Hosts: 1022

4. Marketing (/22):
   Network: 10.0.28.0/22
   Range: 10.0.28.1 - 10.0.31.254
   Hosts: 1022

5. Guest WiFi (/23):
   Network: 10.0.32.0/23
   Range: 10.0.32.1 - 10.0.33.254
   Hosts: 510

6. Management (/25):
   Network: 10.0.34.0/25  
   Range: 10.0.34.1 - 10.0.34.126
   Hosts: 126

7. WAN Links (/30):
   Link 1: 10.0.34.128/30 (.129-.130)
   Link 2: 10.0.34.132/30 (.133-.134)
   ...continuing pattern...
   Link 20: 10.0.34.204/30 (.205-.206)
```

**🔧 Router Configuration Example:**
```cisco
! Data Center VLAN
interface GigabitEthernet0/0.100
 encapsulation dot1Q 100  
 ip address 10.0.0.1 255.255.240.0
 description "Data Center - 4094 hosts"

! Engineering VLAN
interface GigabitEthernet0/0.200
 encapsulation dot1Q 200
 ip address 10.0.16.1 255.255.248.0
 description "Engineering - 2046 hosts"

! Sales VLAN  
interface GigabitEthernet0/0.300
 encapsulation dot1Q 300
 ip address 10.0.24.1 255.255.252.0
 description "Sales - 1022 hosts"

! WAN Interface
interface Serial0/0/0
 ip address 10.0.34.129 255.255.255.252
 description "WAN Link 1 to Branch A"
```

---

### 🔹 QUESTION 12B: Subnet Aggregation and Summarization
**Scenario:** Multiple branch networks need summarization

**Branch Networks:**
```
Branch A: 172.16.0.0/24
Branch B: 172.16.1.0/24  
Branch C: 172.16.2.0/24
Branch D: 172.16.3.0/24
Branch E: 172.16.4.0/24
Branch F: 172.16.5.0/24
Branch G: 172.16.6.0/24
Branch H: 172.16.7.0/24
```

**❓ Question:** Find the most efficient summary route and configure it.

**💡 Answer:**

**📊 Binary Analysis:**
```
172.16.0.0/24 = 172.16.00000000.0/24
172.16.1.0/24 = 172.16.00000001.0/24
172.16.2.0/24 = 172.16.00000010.0/24  
172.16.3.0/24 = 172.16.00000011.0/24
172.16.4.0/24 = 172.16.00000100.0/24
172.16.5.0/24 = 172.16.00000101.0/24
172.16.6.0/24 = 172.16.00000110.0/24
172.16.7.0/24 = 172.16.00000111.0/24

Common bits: 172.16.00000xxx.0
Network bits: 21 bits
Summary: 172.16.0.0/21
```

**✅ Summary Route:** 172.16.0.0/21
- **Covers:** 172.16.0.0 through 172.16.7.255
- **Hosts per summary:** 2048 total addresses
- **Efficiency:** Perfect fit (no wasted addresses)

**🔧 Configuration:**

**OSPF Summarization:**
```cisco
Router(config)# router ospf 1
Router(config-router)# area 1 range 172.16.0.0 255.255.248.0
```

**EIGRP Summarization:**
```cisco
Router(config)# interface serial 0/0/0
Router(config-if)# ip summary-address eigrp 100 172.16.0.0 255.255.248.0
```

**Static Route Summarization:**
```cisco
Router(config)# ip route 172.16.0.0 255.255.248.0 next-hop-ip
```

**🔍 Verification:**
```cisco
Router# show ip route summary
Router# show ip ospf database summary  
Router# show ip eigrp topology summary
```

---

## 📚 COMPREHENSIVE REVIEW QUESTIONS

### 🔹 FINAL CHALLENGE: Integrated Scenario
**Company Network Integration:**

**Network Topology:**
```
                    [Internet]
                        |
                   [Edge Router]
                   203.0.113.1/30
                        |
               [Core Router - OSPF Area 0]
               Router ID: 1.1.1.1
                        |
         +──────────────┼──────────────+
         |              |              |
    [SW1 - Root]    [SW2]          [SW3]
    Priority: 4096  Priority: 8192 Priority: 12288
         |              |              |
    EtherChannel    Single Link    VLAN Trunk
    (LACP Active)   (Backup)       (Access Layer)
         |              |              |
    [SW4 - Access] [SW5 - Access] [SW6 - Access]
```

**Requirements:**
1. Configure Router ID using loopback
2. Set up proper STP hierarchy  
3. Create LACP EtherChannel between SW1-SW4
4. Design VLSM for 4 departments (500, 200, 100, 50 hosts)
5. Configure OSPF for full connectivity

**❓ Question:** Provide complete configuration for all devices.

**💡 Complete Solution:**

**🔧 Core Router Configuration:**
```cisco
Core-R1(config)# hostname Core-Router
Core-R1(config)# interface loopback 0
Core-R1(config-if)# ip address 1.1.1.1 255.255.255.255
Core-R1(config-if)# description "Router ID Interface"

Core-R1(config)# router ospf 1  
Core-R1(config-router)# router-id 1.1.1.1
Core-R1(config-router)# network 192.168.0.0 0.0.255.255 area 0
Core-R1(config-router)# default-information originate

Core-R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

**🔧 Switch Hierarchy Configuration:**

**SW1 (Root Bridge):**
```cisco
SW1(config)# hostname Core-SW1
SW1(config)# spanning-tree vlan 1-4094 priority 4096

! LACP EtherChannel to SW4
SW1(config)# interface range gigabitethernet 0/1-2
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# description "LACP to SW4"

SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk
SW1(config-if)# description "LACP Bundle to Access Layer"
```

**SW4 (Access Layer):**
```cisco
SW4(config)# hostname Access-SW4
SW4(config)# spanning-tree vlan 1-4094 priority 32768

! LACP EtherChannel to SW1  
SW4(config)# interface range gigabitethernet 0/23-24
SW4(config-if-range)# channel-group 1 mode passive
SW4(config-if-range)# description "LACP to Core-SW1"

SW4(config)# interface port-channel 1
SW4(config-if)# switchport mode trunk
```

**🔢 VLSM Design:**
```
Given: 192.168.0.0/16

Department A (500 hosts): 192.168.0.0/23 (510 hosts)
Department B (200 hosts): 192.168.2.0/24 (254 hosts)  
Department C (100 hosts): 192.168.3.0/25 (126 hosts)
Department D (50 hosts):  192.168.3.128/26 (62 hosts)
```

This comprehensive practice guide provides extensive scenarios covering all networking fundamentals from basic to intermediate level, with real-world applications and complete configurations!
