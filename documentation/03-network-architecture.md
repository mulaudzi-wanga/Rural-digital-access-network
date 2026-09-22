# 03 - Network Architecture

## 1. Architecture Overview

The Rural Digital Access Network uses a centralised network architecture designed to provide structured connectivity, VLAN segmentation, routing, security, and controlled external access.

The architecture is based on a hierarchical approach:

**End Devices → Access Switches → Core Switch → Main Router → ISP Router → External Cloud**

The Core Switch provides central Layer 2 connectivity between the access switches and the Main Router.

The Main Router provides Layer 3 services including:

- VLAN default gateways
- Inter-VLAN routing
- DHCP
- Access Control Lists
- External network routing

The ISP Router provides the connection between the internal network and the external network.

---

## 2. Network Topology

The logical topology can be represented as:

**School Devices**  
↓  
**School Access Switch**  
↓  
**Core Switch**  
↓  
**Main Router**  
↓  
**ISP Router**  
↓  
**External Cloud**

The same central architecture is used for the other service areas:

**Clinic Devices → Clinic Access Switch → Core Switch**

**Business Devices → Business Access Switch → Core Switch**

**Community Devices → Community Access Switch → Core Switch**

**Additional Network Devices → Access Infrastructure → Core Switch**

This design allows each service area to remain logically separated while still using the same central network infrastructure.

---

## 3. Core Network Components

The network is built around the following major components:

| Component | Primary Role |
|-----------|--------------|
| Main Router | Inter-VLAN routing, DHCP, ACL security and external routing |
| Core Switch | Central Layer 2 switching and VLAN connectivity |
| Access Switches | Connect end devices to the appropriate VLAN |
| ISP Router | Provides the path toward the external network |
| External Cloud | Provides an external connectivity test destination |
| End Devices | Represent users and systems within each service area |

---

## 4. Service Areas

The network is divided into several service areas representing different functions within the rural environment.

### School

The School network provides connectivity for school-related users and systems.

**VLAN:** 10

**Network:** 192.172.10.0/24

**Default Gateway:** 192.172.10.1

---

### Clinic

The Clinic network provides connectivity for healthcare-related users and systems.

**VLAN:** 20

**Network:** 192.172.20.0/24

**Default Gateway:** 192.172.20.1

---

### Business

The Business network provides connectivity for business-related users and systems.

**VLAN:** 30

**Network:** 192.172.30.0/24

**Default Gateway:** 192.172.30.1

---

### Community

The Community network provides connectivity for community users and services.

**VLAN:** 40

**Network:** 192.172.40.0/24

**Default Gateway:** 192.172.40.1

---

### Additional Network

The Additional Network provides a separate logical network for additional devices or services required by the project.

**VLAN:** 50

**Network:** 192.172.50.0/24

**Default Gateway:** 192.172.50.1

---

### Infrastructure / Management

VLAN 99 is reserved for infrastructure and management purposes.

**VLAN:** 99

**Network:** 192.172.99.0/24

**Default Gateway:** 192.172.99.1

VLAN 99 is used as the Infrastructure / Management VLAN.

It is not configured as a custom native VLAN.

---

## 5. VLAN Architecture

VLAN segmentation provides logical separation between the service areas.

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 192.172.99.1 |

Each VLAN represents a separate broadcast domain.

This reduces unnecessary Layer 2 traffic between service areas and provides the foundation for applying security policies.

---

## 6. Access Layer

Access switches connect end devices to the network.

End-device ports are configured as access ports and assigned to the VLAN required by the device.

For example:

- School devices use VLAN 10.
- Clinic devices use VLAN 20.
- Business devices use VLAN 30.
- Community devices use VLAN 40.
- Additional Network devices use VLAN 50.

This ensures that an end device receives network connectivity from the correct logical network.

Unused switch ports are disabled where applicable to reduce unnecessary physical access to the network.

---

## 7. Core Layer

The Core Switch acts as the central Layer 2 switching point.

Its responsibilities include:

- Connecting access switches
- Maintaining VLAN information
- Forwarding Ethernet frames
- Providing trunk connectivity where required
- Connecting the switching infrastructure to the Main Router

The Core Switch does not replace the Main Router for Layer 3 routing.

The Main Router remains responsible for routing between VLANs.

---

## 8. Trunk Architecture

Trunk links are used where a connection needs to transport VLAN traffic between network devices.

The project uses controlled VLAN trunking rather than automatically carrying every VLAN across every link.

Only the VLANs required by a particular connection should be permitted.

Examples include:

| Connection | Required VLAN |
|------------|---------------|
| School infrastructure | VLAN 10 |
| Clinic infrastructure | VLAN 20 |
| Business infrastructure | VLAN 30 |
| Community infrastructure | VLAN 40 |
| Additional Network infrastructure | VLAN 50 |

This reduces unnecessary VLAN propagation through the network.

---

## 9. Layer 3 Architecture

The Main Router provides the Layer 3 gateway for each VLAN.

Each VLAN has a corresponding router subinterface that acts as the default gateway for devices within that VLAN.

The routing process is:

**End Device → VLAN Gateway → Main Router → Destination**

For external destinations, traffic continues toward the ISP Router and then the External Cloud.

For restricted internal destinations, ACL policies on the Main Router control whether the traffic is permitted.

---

## 10. External Connectivity

The internal network connects toward an ISP Router.

The ISP Router provides the path from the internal network toward the External Cloud.

The External Cloud is used to validate that the internal network can reach an external destination.

The documented External Cloud address is:

**172.16.0.1**

External connectivity testing is important because it verifies more than local VLAN operation.

A successful external test demonstrates that VLAN configuration, gateway configuration, routing, and the external path are functioning together.

---

## 11. Security Architecture

Security is implemented primarily through network segmentation and ACLs.

Each service area has its own VLAN and IPv4 network.

Extended ACLs are applied to control traffic entering the VLAN routing interfaces.

The intended security model is:

**Internal VLAN → Own Gateway → Controlled Routing → External Network**

Unnecessary communication between internal service areas is restricted.

For example:

**School → Clinic:** Restricted

**School → Business:** Restricted

**School → Community:** Restricted

**School → Additional Network:** Restricted

The same principle is applied to the other VLANs.

External connectivity remains available where permitted by the security policy.

---

## 12. Traffic Flow

### Internal VLAN Traffic

When a device communicates with another device in the same VLAN, the traffic remains within that Layer 2 network.

### Inter-VLAN Traffic

When a device needs to communicate with another VLAN:

1. The device sends traffic to its default gateway.
2. The Main Router receives the traffic.
3. The relevant ACL policy is evaluated.
4. If permitted, the router forwards the traffic toward the destination VLAN.
5. If restricted, the traffic is denied.

### External Traffic

When a device communicates with an external destination:

1. The device sends traffic to its VLAN gateway.
2. The Main Router evaluates the traffic.
3. The Main Router forwards permitted traffic toward the ISP Router.
4. The ISP Router forwards the traffic toward the external network.
5. The External Cloud can be used to verify connectivity.

---

## 13. Architectural Design Principles

The network architecture follows several important networking principles:

### Segmentation

Different service areas are separated into individual VLANs.

### Centralised Routing

The Main Router provides a single controlled Layer 3 point for VLAN routing.

### Controlled Connectivity

ACLs prevent unnecessary communication between internal networks.

### Structured Addressing

Each VLAN uses a dedicated IPv4 network following a consistent addressing scheme.

### Controlled VLAN Propagation

Trunk links carry only the VLANs required by the connected network segments.

### Infrastructure Separation

Infrastructure and management traffic is separated using VLAN 99.

### Verification

Each major part of the architecture can be independently tested using Cisco IOS verification commands.

---

## 14. Architecture Summary

The completed architecture provides a centralised and segmented network for the rural digital access environment.

The design combines:

- VLAN segmentation
- Access switching
- Core switching
- Router-on-a-Stick
- IPv4 addressing
- DHCP
- ACL-based traffic control
- External routing
- Infrastructure management

The resulting architecture provides a structured foundation that can be expanded with additional users, devices, or services without redesigning the entire network.

---

## 15. Key Architecture Outcome

The final architecture achieves the following:

- Separate logical networks for each service area
- Centralised switching through the Core Switch
- Centralised Layer 3 routing through the Main Router
- Automatic IP addressing through DHCP
- Controlled inter-VLAN communication
- External connectivity through the ISP Router
- Dedicated Infrastructure / Management VLAN
- A structured foundation for future expansion

This architecture forms the foundation for the detailed VLAN, addressing, routing, DHCP, security, troubleshooting, and testing documentation that follows.
