# 11 - Implementation and Configuration

## 1. Purpose

This document describes the implementation of the Rural Digital Access Network in Cisco Packet Tracer.

The implementation converts the network design into a functioning multi-VLAN routed network with DHCP, controlled inter-VLAN communication, and external connectivity.

The implementation covers:

- VLAN configuration
- Access-port configuration
- Trunk configuration
- Router-on-a-Stick
- DHCP
- Inter-VLAN routing
- Static routing
- Extended ACLs
- Infrastructure / Management VLAN
- Unused-port security

---

## 2. Network Implementation Overview

The network uses a hierarchical structure consisting of:

**End Devices → Access Switches → Core Switch → Main Router → ISP Router → External Network**

The Core Switch provides central Layer 2 connectivity.

The Main Router provides:

- Inter-VLAN routing
- DHCP
- Layer 3 gateway services
- ACL traffic filtering
- External network routing

The ISP Router provides connectivity between the internal network and the external network.

---

## 3. VLAN Implementation

The following VLANs are implemented:

| VLAN | Service Area | Network | Gateway |
|------|--------------|---------|---------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 192.172.99.1 |

Each VLAN represents a separate logical network.

---

## 4. Access Port Configuration

End devices are connected to access ports.

An access port carries traffic for a single VLAN.

For example, a port assigned to the Clinic network is configured for VLAN 20.

The general configuration is:

    interface fa0/5
     switchport mode access
     switchport access vlan 20

The exact interface number depends on the physical topology.

Access-port configuration is verified using:

    show vlan brief

---

## 5. Trunk Configuration

Trunk links are used where a single physical connection must carry multiple VLANs.

The Core Switch uses trunk connectivity toward the Main Router and relevant Access Switches.

A basic trunk configuration is:

    interface fa0/24
     switchport mode trunk

Where required, the allowed VLAN list can be explicitly configured:

    switchport trunk allowed vlan 10,20,30,40,50,99

The trunk configuration is verified using:

    show interfaces trunk

---

## 6. Router-on-a-Stick

Inter-VLAN routing is implemented using Router-on-a-Stick.

The Main Router uses a physical interface connected to the Core Switch through a trunk.

Subinterfaces are created for the VLANs.

The general structure is:

    interface g0/0.10
     encapsulation dot1Q 10
     ip address 192.172.10.1 255.255.255.0

Additional subinterfaces follow the same structure for the other VLANs.

Example:

    interface g0/0.20
     encapsulation dot1Q 20
     ip address 192.172.20.1 255.255.255.0

    interface g0/0.30
     encapsulation dot1Q 30
     ip address 192.172.30.1 255.255.255.0

    interface g0/0.40
     encapsulation dot1Q 40
     ip address 192.172.40.1 255.255.255.0

    interface g0/0.50
     encapsulation dot1Q 50
     ip address 192.172.50.1 255.255.255.0

    interface g0/0.99
     encapsulation dot1Q 99
     ip address 192.172.99.1 255.255.255.0

The physical router interface must also be operational.

---

## 7. DHCP Implementation

The Main Router provides DHCP services for the internal VLANs.

The first ten addresses of each network are reserved.

For example:

    ip dhcp excluded-address 192.172.10.1 192.172.10.10

A DHCP pool is then created for the VLAN.

Example:

    ip dhcp pool SCHOOL
     network 192.172.10.0 255.255.255.0
     default-router 192.172.10.1

The same structure is applied to the other required VLAN networks.

DHCP operation is verified using:

    show ip dhcp pool
    show ip dhcp binding

---

## 8. Inter-VLAN Routing

The Main Router provides the default gateway for each VLAN.

For example:

**VLAN 10 → 192.172.10.1**

**VLAN 20 → 192.172.20.1**

**VLAN 30 → 192.172.30.1**

**VLAN 40 → 192.172.40.1**

**VLAN 50 → 192.172.50.1**

**VLAN 99 → 192.172.99.1**

A client sends traffic for another network to its default gateway.

The Main Router then determines the appropriate route.

Inter-VLAN routing is therefore performed at Layer 3.

---

## 9. Internal Network Routing

The Main Router has directly connected routes for the VLAN networks.

These routes are created automatically when the corresponding router subinterfaces are operational.

The routing table can be inspected using:

    show ip route

The expected connected networks include:

    192.172.10.0/24
    192.172.20.0/24
    192.172.30.0/24
    192.172.40.0/24
    192.172.50.0/24
    192.172.99.0/24

---

## 10. ISP Connectivity

The Main Router connects to the ISP Router using the external transit network.

The project uses:

**ISP Router:** 10.0.0.1

**Main Router:** 10.0.0.2

The Main Router uses the ISP Router as the next hop for external connectivity.

The exact static or default route configuration depends on the final Packet Tracer topology.

Routing should be verified using:

    show ip route

---

## 11. ISP Return Routes

The ISP Router requires routes back toward the internal VLAN networks.

The internal networks are reached through the Main Router.

The routing relationship is:

**Internal VLANs → Main Router → ISP Router**

and:

**External Network → ISP Router → Main Router → Internal VLAN**

Return routing is necessary for two-way communication.

---

## 12. Extended ACL Implementation

Extended ACLs are used to restrict unnecessary communication between service-area VLANs.

The ACL policy blocks traffic between restricted internal networks while allowing required traffic to permitted destinations.

The general structure is:

    ip access-list extended VLAN10-FILTER
     deny ip 192.172.10.0 0.0.0.255 192.172.20.0 0.0.0.255
     deny ip 192.172.10.0 0.0.0.255 192.172.30.0 0.0.0.255
     deny ip 192.172.10.0 0.0.0.255 192.172.40.0 0.0.0.255
     deny ip 192.172.10.0 0.0.0.255 192.172.50.0 0.0.0.255
     permit ip any any

The actual ACL configuration used in the Packet Tracer file should remain the authoritative configuration.

ACLs are verified using:

    show ip access-lists

---

## 13. ACL Application

ACLs must be applied to the appropriate router interfaces in the correct direction.

The placement of an ACL determines which traffic is filtered.

Before modifying an ACL, verify:

- Source network
- Destination network
- Protocol
- Direction
- Interface
- Existing permit and deny statements

The running configuration can be inspected using:

    show running-config

---

## 14. Infrastructure / Management VLAN

VLAN 99 is used for infrastructure and management purposes.

**Network:** 192.172.99.0/24

**Gateway:** 192.172.99.1

The VLAN provides logical separation between infrastructure traffic and normal service-area users.

VLAN 99 is not configured as a custom native VLAN.

---

## 15. Unused Port Security

Unused switch interfaces are disabled where applicable.

This reduces unnecessary physical access to the switching infrastructure.

The interface state can be checked using:

    show interfaces status

A disabled unused interface should not be used for normal end-device connectivity unless intentionally reconfigured.

---

## 16. Configuration Verification

After implementation, each device should be checked.

### Core Switch

Use:

    show vlan brief
    show interfaces trunk
    show interfaces status

### Main Router

Use:

    show ip interface brief
    show ip route
    show ip dhcp pool
    show ip dhcp binding
    show ip access-lists

### ISP Router

Use:

    show ip interface brief
    show ip route

These commands provide a basic operational view of the network.

---

## 17. End-Device Configuration

End devices are configured to obtain their network information automatically where DHCP is provided.

The expected client configuration includes:

- IPv4 address
- Subnet mask
- Default gateway
- DNS information where configured

A client should receive an address corresponding to the VLAN assigned to its access port.

---

## 18. Implementation Verification Sequence

After configuration changes, the following sequence is used:

### Step 1

Verify interfaces are operational.

### Step 2

Verify VLANs.

### Step 3

Verify access ports.

### Step 4

Verify trunk links.

### Step 5

Verify router subinterfaces.

### Step 6

Verify DHCP pools.

### Step 7

Verify client IP addresses.

### Step 8

Ping the local gateway.

### Step 9

Test restricted inter-VLAN communication.

### Step 10

Test permitted external connectivity.

This sequence confirms that the implemented configuration works from Layer 2 through Layer 3 and external connectivity.

---

## 19. Configuration Management

Configuration changes should be made deliberately and verified after implementation.

Before making major changes, the current configuration should be reviewed using:

    show running-config

After successful testing, the configuration can be saved using:

    copy running-config startup-config

This ensures that the configuration is retained after a device restart within the Packet Tracer environment.

---

## 20. Implementation Challenges

Several areas require particular attention during implementation.

### VLAN Consistency

The same VLAN must exist on the relevant switches and be correctly carried across required trunk links.

### DHCP and VLAN Alignment

The DHCP network must correspond to the VLAN in which the client is located.

### Router Subinterfaces

Each VLAN requiring Layer 3 connectivity must have a corresponding router subinterface.

### ACL Ordering

ACL entries are processed in order, making the placement of permit and deny statements important.

### Return Routing

External connectivity requires routing in both directions.

These areas are common points of failure in multi-VLAN networks.

---

## 21. Implementation Outcome

The implementation converts the logical network design into a functioning Packet Tracer network.

The implemented architecture provides:

- Multiple VLANs
- Layer 2 segmentation
- Trunk connectivity
- Router-on-a-Stick inter-VLAN routing
- DHCP address allocation
- Static or default routing toward the ISP
- Extended ACL traffic filtering
- Infrastructure / Management segmentation
- Disabled unused ports where applicable

The implementation is validated using the testing procedures documented in the previous section.

---

## 22. Summary

The Rural Digital Access Network was implemented using standard Cisco networking concepts.

The implementation combines:

**VLANs + Trunks + Router-on-a-Stick + DHCP + Routing + ACLs**

Each technology performs a specific function:

**VLANs → Network Segmentation**

**Trunks → VLAN Transport**

**Router-on-a-Stick → Inter-VLAN Routing**

**DHCP → Automatic IP Configuration**

**Routing → Path Selection**

**ACLs → Traffic Control**

Together, these components form the operational network architecture used by the project.
