# ACL and NAT Fundamentals Guide
**Date: 2025-07-01 15:11:37 | User: nix-shadow**

---

## ACCESS CONTROL LISTS (ACLs)

### 1. ACL BASICS

**Definition:**
Access Control Lists (ACLs) are sequential lists of permit or deny statements that control traffic flow through a network device. They function as packet filters that allow or block traffic based on specified criteria.

**Purpose:**
- Filter network traffic
- Limit network access
- Implement security policies
- Control traffic flow
- Classify traffic for QoS

**How ACLs Work:**
```
┌───────────────────────────────────────────────────────┐
│                   PACKET ARRIVES                      │
│                         │                             │
│                         ▼                             │
│               CHECK AGAINST FIRST ACE                 │
│            (Access Control Entry) in ACL              │
│                         │                             │
│                         ▼                             │
│            ┌─────────────────────────┐                │
│            │  Does packet match ACE? │                │
│            └───────────┬─────────────┘                │
│                        │                              │
│                  ┌─────┴─────┐                        │
│                  │           │                        │
│                 YES          NO                       │
│                  │           │                        │
│                  ▼           ▼                        │
│            ┌──────────┐  ┌─────────────────────┐     │
│            │APPLY ACE │  │CHECK AGAINST NEXT ACE│     │
│            │ACTION    │  └──────────┬──────────┘     │
│            │(permit/  │             │                │
│            │deny)     │             │                │
│            └──────────┘     ┌───────┴───────┐        │
│                             │               │        │
│                             │ End of ACL?   │        │
│                             │               │        │
│                             └───┬───────────┘        │
│                                 │                    │
│                              ┌──┴──┐                 │
│                              │     │                 │
│                              NO   YES                │
│                                    │                 │
│                                    ▼                 │
│                            ┌─────────────┐           │
│                            │ IMPLICIT    │           │
│                            │ DENY        │           │
│                            └─────────────┘           │
└───────────────────────────────────────────────────────┘
```

### 2. TYPES OF ACLs

**Standard ACLs:**
- Filter based on source IP address only
- Numbers 1-99 and 1300-1999
- Simple but less granular control
- Applied close to destination

**Extended ACLs:**
- Filter based on source/destination IP, protocol, port numbers
- Numbers 100-199 and 2000-2699
- More granular control
- Applied close to source

**Named ACLs:**
- Use descriptive names instead of numbers
- Can be standard or extended
- Easier to understand and maintain
- Allow removal of individual entries

**IPv6 ACLs:**
- Specifically for IPv6 traffic
- Always named (no numbered IPv6 ACLs)
- Similar structure to named extended ACLs

### 3. ACL SYNTAX & COMPONENTS

**Wildcard Masks:**
Wildcard masks specify which bits of an IP address to check:
- 0 = bit must match exactly
- 1 = bit can be anything

```
Examples:
0.0.0.0 = Match exact IP address
0.0.0.255 = Match network portion of Class C (/24)
0.0.255.255 = Match network portion of Class B (/16)
255.255.255.255 = Match any address

Calculation: Wildcard mask = 255.255.255.255 - subnet mask
```

**Common Keywords:**
- `any`: Matches any address
- `host`: Matches a single host (shorthand for 0.0.0.0 wildcard)
- `eq`: Equal to port number
- `neq`: Not equal to port number
- `lt`: Less than port number
- `gt`: Greater than port number
- `range`: Range of port numbers
- `established`: Matches established TCP connections (return traffic)
- `log`: Logs matches to the ACL entry

### 4. ACL CONFIGURATION EXAMPLES

**Standard ACL Example:**
```
! Create standard ACL
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 10 deny any

! Apply to interface
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip access-group 10 out
```

**Extended ACL Example:**
```
! Create extended ACL
Router(config)# access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80
Router(config)# access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config)# access-list 101 deny tcp any any eq 23
Router(config)# access-list 101 permit ip any any

! Apply to interface
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip access-group 101 in
```

**Named ACL Example:**
```
! Create named extended ACL
Router(config)# ip access-list extended INTERNET_FILTER
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq www
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config-ext-nacl)# deny tcp any any eq telnet log
Router(config-ext-nacl)# permit ip any any
Router(config-ext-nacl)# exit

! Apply to interface
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip access-group INTERNET_FILTER in
```

**Named ACL Editing:**
```
! Add entry to existing named ACL
Router(config)# ip access-list extended INTERNET_FILTER
Router(config-ext-nacl)# 15 permit tcp 192.168.1.0 0.0.0.255 any eq 8080
Router(config-ext-nacl)# exit

! Remove specific entry
Router(config)# ip access-list extended INTERNET_FILTER
Router(config-ext-nacl)# no 15
Router(config-ext-nacl)# exit
```

### 5. ACL PLACEMENT & DIRECTION

**Direction:**
```
INBOUND ACL:
- Applied using "in" keyword
- Filters traffic entering the interface
- Processed before routing decision
- More efficient for filtering unwanted traffic

OUTBOUND ACL:
- Applied using "out" keyword
- Filters traffic leaving the interface
- Processed after routing decision
- Good for filtering outgoing traffic
```

**Strategic Placement:**
```
Standard ACLs: Place close to destination
Extended ACLs: Place close to source

┌───────────────────────────────────────────────────────────┐
│                      ROUTER                               │
│                                                           │
│   ┌─────────┐                             ┌─────────┐     │
│   │         │                             │         │     │
│ ──┼──►[ACL IN]──►[ROUTING]──────────────►│         │───► │
│   │         │      PROCESS                │         │     │
│   │ G0/0    │                             │ G0/1    │     │
│   │         │                             │         │     │
│   │         │◄──────────────[ROUTING]◄───│         │◄──── │
│   │         │                 PROCESS    [ACL OUT]◄─┼──── │
│   └─────────┘                             └─────────┘     │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

### 6. ACL VERIFICATION & TROUBLESHOOTING

**Verification Commands:**
```
! Show all ACLs
Router# show access-lists

! Show specific ACL
Router# show access-list 101
Router# show access-list INTERNET_FILTER

! Show ACLs applied to interfaces
Router# show ip interface

! Show ACL on specific interface
Router# show ip interface GigabitEthernet0/0
```

**Common Issues & Solutions:**
```
1. IMPLICIT DENY PROBLEM
   - Symptom: All traffic blocked
   - Solution: Add "permit any" or necessary permit statements

2. ORDER ISSUES
   - Symptom: Wrong traffic filtered
   - Solution: Reorder ACL with specific entries first

3. DIRECTION MISTAKES
   - Symptom: ACL not filtering as expected
   - Solution: Verify and correct direction (in vs. out)

4. WRONG WILDCARD MASK
   - Symptom: Incorrect matching
   - Solution: Double-check wildcard mask calculation
```

### 7. ACL BEST PRACTICES

1. **Plan before implementing**
   - Document requirements
   - Test in lab environment
   - Consider impact on existing traffic

2. **Use named ACLs**
   - More descriptive and easier to understand
   - Easier to edit individual entries
   - Self-documenting

3. **Order matters**
   - Place specific entries before general ones
   - Most frequently matched entries first (performance)
   - Group related entries together

4. **Security considerations**
   - Standard ACLs close to destination
   - Extended ACLs close to source
   - Use "log" keyword for security-related entries

5. **Management**
   - Document ACL purpose and entries
   - Review periodically
   - Remove unused entries

---

## NETWORK ADDRESS TRANSLATION (NAT)

### 1. NAT BASICS

**Definition:**
Network Address Translation (NAT) is a process that modifies network address information in packet headers while in transit across a routing device, typically to map private IP addresses to public IP addresses.

**Purpose:**
- Conserve public IP addresses
- Hide internal network structure (security)
- Connect private networks to the Internet
- Merge networks with overlapping addresses
- Provide load balancing for servers

**Private IP Address Ranges:**
```
Class A: 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
Class B: 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
Class C: 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
```

### 2. NAT TERMINOLOGY

```
INSIDE LOCAL: Private IP address of an inside device
INSIDE GLOBAL: Public IP address that represents an inside device to the outside
OUTSIDE LOCAL: IP address of an outside device as it appears to the inside
OUTSIDE GLOBAL: Actual IP address of an outside device
```

**Visual Representation:**
```
┌───────────────────┐              ┌───────────────────┐
│  INSIDE NETWORK   │              │ OUTSIDE NETWORK   │
│  (PRIVATE)        │              │ (PUBLIC)          │
│                   │              │                   │
│  ┌────────────┐   │ NAT DEVICE   │                   │
│  │Inside Host │   │ ┌─────────┐  │                   │
│  │           │◄──┼─┼─┤TRANSLATE│◄─┼───┐              │
│  │192.168.1.10│   │ │ │        │  │   │              │
│  │(Inside    │   │ │ │        │  │   │              │
│  │ Local)    │   │ │ │        │  │   │              │
│  └────────────┘   │ │ │        │  │   │              │
│        ▲          │ │ │        │  │   │              │
│        │          │ │ │        │  │   │              │
│        │          │ │ │        │  │   │              │
│        │          │ │ │        │  │   │              │
│        ▼          │ │ │        │  │   ▼              │
│  ┌────────────┐   │ │ │        │  │ ┌────────────┐   │
│  │Inside Host │   │ │ │        │  │ │Outside Host│   │
│  │as seen from│   │ └─┤TRANSLATE├──┼─┤as seen from│   │
│  │Outside     │   │   │        │  │ │Inside      │   │
│  │203.0.113.5 │   │   └─────────┘  │ │209.165.200.1│   │
│  │(Inside     │   │                │ │(Outside    │   │
│  │ Global)    │   │                │ │ Global)    │   │
│  └────────────┘   │                │ └────────────┘   │
│                   │                │                   │
└───────────────────┘                └───────────────────┘
```

### 3. TYPES OF NAT

**Static NAT:**
```
ONE-TO-ONE MAPPING:
- Each internal IP has permanent dedicated external IP
- Bidirectional connectivity (outside can initiate to inside)
- Typically used for servers that need consistent external address

┌─────────────────┐             ┌─────────────────┐
│  INSIDE         │             │  OUTSIDE        │
│                 │             │                 │
│ 192.168.1.10 ───┼─────────────┼──► 203.0.113.10 │
│ 192.168.1.11 ───┼─────────────┼──► 203.0.113.11 │
│ 192.168.1.12 ───┼─────────────┼──► 203.0.113.12 │
│                 │             │                 │
└─────────────────┘             └─────────────────┘
```

**Dynamic NAT:**
```
MANY-TO-MANY MAPPING:
- Pool of internal IPs mapped to pool of external IPs
- First come, first served allocation of external addresses
- Entries time out when not in use
- Can result in address exhaustion if pool too small

┌─────────────────┐             ┌─────────────────┐
│  INSIDE         │             │  OUTSIDE        │
│                 │             │                 │
│ 192.168.1.10 ───┼─────────────┼──► 203.0.113.5  │
│ 192.168.1.11 ───┼─────────────┼──► 203.0.113.6  │
│ 192.168.1.12 ───┼─────────────┼──► 203.0.113.7  │
│ 192.168.1.13 ───┼─────────────┼─X► (WAITING)    │
│                 │             │                 │
└─────────────────┘             └─────────────────┘
```

**PAT (Port Address Translation) / NAT Overload:**
```
MANY-TO-ONE MAPPING:
- Multiple internal IPs share single external IP
- Uses different TCP/UDP ports to distinguish connections
- Most common NAT implementation
- Most efficient use of public IP addresses

┌─────────────────────────┐      ┌────────────────────────────┐
│  INSIDE                 │      │  OUTSIDE                   │
│                         │      │                            │
│ 192.168.1.10:1234 ──────┼──────┼──► 203.0.113.1:10001      │
│ 192.168.1.11:1234 ──────┼──────┼──► 203.0.113.1:10002      │
│ 192.168.1.12:1234 ──────┼──────┼──► 203.0.113.1:10003      │
│ 192.168.1.13:5678 ──────┼──────┼──► 203.0.113.1:10004      │
│                         │      │                            │
└─────────────────────────┘      └────────────────────────────┘
```

### 4. NAT CONFIGURATION EXAMPLES

**Static NAT Configuration:**
```
! Define inside and outside interfaces
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 203.0.113.1 255.255.255.0
Router(config-if)# ip nat outside
Router(config-if)# exit

! Configure static NAT translations
Router(config)# ip nat inside source static 192.168.1.10 203.0.113.10
Router(config)# ip nat inside source static 192.168.1.11 203.0.113.11
Router(config)# ip nat inside source static 192.168.1.12 203.0.113.12
```

**Dynamic NAT Configuration:**
```
! Define inside and outside interfaces
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

! Configure dynamic NAT
Router(config)# ip nat pool MY_POOL 203.0.113.5 203.0.113.10 netmask 255.255.255.0
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 pool MY_POOL
```

**PAT (NAT Overload) Configuration:**
```
! Define inside and outside interfaces
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

! Configure PAT (NAT with overload)
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload
```

### 5. NAT VERIFICATION & TROUBLESHOOTING

**Verification Commands:**
```
! Show active NAT translations
Router# show ip nat translations

! Show NAT statistics
Router# show ip nat statistics

! Clear NAT translations
Router# clear ip nat translation *

! Debug NAT operations
Router# debug ip nat
```

**Example Output:**
```
Router# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
tcp 203.0.113.1:10001  192.168.1.10:1234  209.165.200.1:80   209.165.200.1:80
tcp 203.0.113.1:10002  192.168.1.11:1234  209.165.200.1:443  209.165.200.1:443
```

**Common Issues & Solutions:**
```
1. ONE-WAY CONNECTIVITY
   - Symptom: Can initiate outbound but not inbound
   - Solution: Check for static NAT or port forwarding

2. NO CONNECTIVITY
   - Symptom: NAT translations not created
   - Solution: Verify inside/outside interface designation

3. NAT POOL EXHAUSTION
   - Symptom: Some clients can't get translations
   - Solution: Increase pool size or use PAT

4. APPLICATION COMPATIBILITY
   - Symptom: Specific applications not working
   - Solution: Configure NAT ALG (Application Layer Gateway)
```

### 6. NAT OPERATION PROCESS

**Outbound Traffic Flow:**
```
1. INSIDE HOST SENDS PACKET
   Source: 192.168.1.10, Destination: 8.8.8.8

2. PACKET REACHES NAT DEVICE
   Device checks if translation needed

3. NAT CREATES TRANSLATION ENTRY
   Maps 192.168.1.10 to 203.0.113.1:port

4. NAT MODIFIES PACKET HEADER
   Changes source from 192.168.1.10 to 203.0.113.1:port

5. MODIFIED PACKET FORWARDED
   Source: 203.0.113.1:port, Destination: 8.8.8.8
```

**Inbound Response Flow:**
```
1. EXTERNAL HOST SENDS RESPONSE
   Source: 8.8.8.8, Destination: 203.0.113.1:port

2. PACKET REACHES NAT DEVICE
   Device looks up translation in NAT table

3. NAT MODIFIES PACKET HEADER
   Changes destination from 203.0.113.1:port to 192.168.1.10

4. MODIFIED PACKET FORWARDED
   Source: 8.8.8.8, Destination: 192.168.1.10
```

### 7. NAT BEST PRACTICES

1. **Document NAT design**
   - Keep records of all static mappings
   - Document public IP address usage
   - Maintain NAT policies

2. **Security considerations**
   - Use PAT when possible for added security
   - Implement access control with NAT
   - Consider using a DMZ for public services

3. **Performance optimization**
   - Monitor NAT translation table size
   - Set appropriate timeouts
   - Consider hardware with dedicated NAT processing

4. **High availability**
   - Implement redundant NAT devices
   - Configure NAT to work with failover
   - Test failover scenarios

5. **Troubleshooting preparation**
   - Enable logging
   - Establish monitoring
   - Create troubleshooting procedures

---

## COMMON ACL & NAT SCENARIOS

### 1. SECURING A NETWORK WITH ACLs

**Inbound Filtering (Extended ACL):**
```
Router(config)# ip access-list extended INBOUND_FILTER
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 80
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 443
Router(config-ext-nacl)# permit tcp any host 203.0.113.11 eq 25
Router(config-ext-nacl)# permit icmp any any echo-reply
Router(config-ext-nacl)# deny ip any any log
Router(config-ext-nacl)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group INBOUND_FILTER in
```

### 2. PUBLIC SERVER WITH STATIC NAT

**Web Server Access Configuration:**
```
! Configure static NAT for web server
Router(config)# ip nat inside source static 192.168.1.100 203.0.113.10

! Configure ACL to allow web traffic
Router(config)# ip access-list extended WEB_ACCESS
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 80
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 443
Router(config-ext-nacl)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group WEB_ACCESS in
```

### 3. SMALL OFFICE INTERNET ACCESS

**Office Internet Configuration:**
```
! Configure PAT for Internet access
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

! Basic outbound security with ACL
Router(config)# ip access-list extended OUTBOUND_FILTER
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 80
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config-ext-nacl)# permit udp 192.168.1.0 0.0.0.255 any eq 53
Router(config-ext-nacl)# permit icmp 192.168.1.0 0.0.0.255 any
Router(config-ext-nacl)# deny ip any any log
Router(config-ext-nacl)# exit

Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip access-group OUTBOUND_FILTER out
```

---

## ACL & NAT INTEGRATION

NAT and ACLs often work together to provide secure, controlled access to resources:

### 1. ACL CONSIDERATIONS WITH NAT

- ACLs applied to outside interfaces should reference global addresses
- ACLs applied to inside interfaces should reference local addresses
- Order matters: ACLs are processed before NAT on inbound traffic
- Order matters: NAT is processed before ACLs on outbound traffic

### 2. NAT AND ACL PROCESSING ORDER

```
INBOUND TRAFFIC (Outside to Inside):
1. Inbound ACL on outside interface
2. NAT translation (global to local)
3. Routing decision
4. Outbound ACL on inside interface
5. Forward to destination

OUTBOUND TRAFFIC (Inside to Outside):
1. Inbound ACL on inside interface
2. Routing decision
3. NAT translation (local to global)
4. Outbound ACL on outside interface
5. Forward to destination
```

### 3. COMBINED EXAMPLE

**Secure DMZ with NAT and ACLs:**
```
! Define interfaces
Router(config)# interface GigabitEthernet0/0
Router(config-if)# description Internal Network
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# description DMZ Network
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/2
Router(config-if)# description Internet Connection
Router(config-if)# ip address 203.0.113.1 255.255.255.0
Router(config-if)# ip nat outside
Router(config-if)# exit

! Configure NAT for internal network
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/2 overload

! Static NAT for DMZ servers
Router(config)# ip nat inside source static 192.168.2.10 203.0.113.10 
Router(config)# ip nat inside source static 192.168.2.20 203.0.113.20

! ACL to protect DMZ servers
Router(config)# ip access-list extended PROTECT_DMZ
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 80
Router(config-ext-nacl)# permit tcp any host 203.0.113.10 eq 443
Router(config-ext-nacl)# permit tcp any host 203.0.113.20 eq 25
Router(config-ext-nacl)# deny ip any 192.168.2.0 0.0.0.255 log
Router(config-ext-nacl)# permit ip any any
Router(config-ext-nacl)# exit

! Apply ACL to outside interface
Router(config)# interface GigabitEthernet0/2
Router(config-if)# ip access-group PROTECT_DMZ in

! ACL to protect internal network
Router(config)# ip access-list extended INTERNAL_PROTECTION
Router(config-ext-nacl)# permit ip 192.168.1.0 0.0.0.255 any
Router(config-ext-nacl)# permit ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
Router(config-ext-nacl)# deny ip any any log
Router(config-ext-nacl)# exit

! Apply ACL to internal interface
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip access-group INTERNAL_PROTECTION in
```

---

## CONCLUSION

ACLs and NAT are fundamental networking technologies that, when properly understood and implemented, provide essential functionality for modern networks:

- **ACLs** provide traffic filtering and security policy enforcement
- **NAT** enables address conservation and network boundary protection

The effective use of both technologies requires understanding:
1. The different types of ACLs and NAT implementations
2. How traffic flows through the network
3. The order of operations as packets are processed
4. Best practices for security and management

By mastering these concepts, network administrators can build secure, efficient networks that balance accessibility with protection.