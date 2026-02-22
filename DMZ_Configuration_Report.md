# DMZ Configuration Report with Cisco Packet Tracer

## 1. Lab Objective

The objective of this lab was to configure and secure a DMZ using a
Cisco ISR router.\
This included assigning IP addresses to all devices, implementing static
NAT to expose the DMZ web server to the external network, and
configuring extended ACLs to control traffic between the LAN, DMZ, and
External networks.

------------------------------------------------------------------------

## 2. Implemented Topology

### Devices Used

-   1 Cisco ISR Router (Router_FW)
-   3 Switches (LAN, DMZ, External)
-   PC_Internal
-   Server_Web_DMZ
-   PC_External

### Network Zones

-   LAN: 192.168.1.0/24\
-   DMZ: 192.168.2.0/24\
-   External: 192.168.3.0/24

### Zone Function

-   **LAN:** Internal trusted user network\
-   **DMZ:** Public-facing web server network\
-   **External:** Simulated Internet network

------------------------------------------------------------------------

## 3. IP Addressing Plan

  Device           IP Address     Subnet Mask     Default Gateway
  ---------------- -------------- --------------- -----------------
  PC_Internal      192.168.1.10   255.255.255.0   192.168.1.1
  Server_Web_DMZ   192.168.2.10   255.255.255.0   192.168.2.1
  PC_External      192.168.3.10   255.255.255.0   192.168.3.1

### Router Interfaces

-   G0/0 (LAN): 192.168.1.1\
-   G0/1 (DMZ): 192.168.2.1\
-   G0/2 (External): 192.168.3.1

------------------------------------------------------------------------

## 4. Applied Configuration (Summary)

### Static NAT

    ip nat inside source static 192.168.2.10 192.168.3.1

### ACL -- Allow HTTP from Internet to DMZ

    access-list 100 permit tcp any host 192.168.3.1 eq 80

Applied inbound on **GigabitEthernet0/2 (External/WAN)**.

### ACL -- Block DMZ to LAN

    access-list 110 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
    access-list 110 permit ip any any

Applied inbound on **GigabitEthernet0/1 (DMZ)**.

------------------------------------------------------------------------

## 5. Verifications Performed

-   PC_Internal successfully pinged its gateway.\
-   PC_External successfully accessed the DMZ web server via HTTP.\
-   ICMP from PC_External to 192.168.3.1 was blocked.\
-   Ping from Server_Web_DMZ to PC_Internal failed (correctly blocked).\
-   All grading checks returned **100% completion**.

------------------------------------------------------------------------

## 6. Conclusions and Recommendations

In this lab, I learned how to configure static NAT and extended ACLs to
secure a multi-zone network. I also learned the importance of verifying
connectivity before applying ACLs, since a small configuration mistake
can block all traffic. NAT and ACLs together allow necessary services
while protecting internal networks.
