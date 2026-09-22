# 05 - VLAN Segmentation

## 1. Purpose

VLAN segmentation is a core part of the Rural Digital Access Network design.

The network is divided into separate logical networks so that different service areas do not operate within the same broadcast domain.

The VLAN design provides:

- Logical network separation
- Smaller broadcast domains
- Better traffic organisation
- Easier troubleshooting
- A foundation for access control
- Structured network expansion

---

## 2. VLAN Structure

The project uses the following VLANs:

| VLAN | Name | Service Area |
|------|------|--------------|
| 10 | School | School network |
| 20 | Clinic | Clinic network |
| 30 | Business | Business network |
| 40 | Community | Community network |
| 50 | Additional Network | Additional network |
| 99 | Infrastructure / Management | Network infrastructure |

Each VLAN has its own IPv4 subnet.

---

## 3. VLAN and IPv4 Relationship

The VLAN number is reflected in the IPv4 addressing scheme.

| VLAN | IPv4 Network | Default Gateway |
|------|--------------|-----------------|
| 10 | 192.172.10.0/24 | 192.172.10.1 |
| 20 | 192.172.20.0/24 | 192.172.20.1 |
| 30 | 192.172.30.0/24 | 192.172.30.1 |
| 40 | 192.172.40.0/24 | 192.172.40.1 |
| 50 | 192.172.50.0/24 | 192.172.50.1 |
| 99 | 192.172.99.0/24 | 192.172.99.1 |

This consistent relationship makes it easier to identify a device's expected network from its IP address.

---

## 4. VLAN 10 - School

VLAN 10 is assigned to the School service area.

**VLAN:** 10

**Network:** 192.172.10.0/24

**Gateway:** 192.172.10.1

Devices connected to School access ports are assigned to VLAN 10.

The School network is treated as a separate logical network from the Clinic, Business, Community, and Additional Network VLANs.

---

## 5. VLAN 20 - Clinic

VLAN 20 is assigned to the Clinic service area.

**VLAN:** 20

**Network:** 192.172.20.0/24

**Gateway:** 192.172.20.1

Clinic devices connected to the appropriate access ports operate within VLAN 20.

Traffic from the Clinic network is subject to the network's routing and ACL policies.

---

## 6. VLAN 30 - Business

VLAN 30 is assigned to the Business service area.

**VLAN:** 30

**Network:** 192.172.30.0/24

**Gateway:** 192.172.30.1

Business devices are placed into VLAN 30 through their switch access ports.

This prevents normal Layer 2 traffic from the Business network from becoming part of the other service-area broadcast domains.

---

## 7. VLAN 40 - Community

VLAN 40 is assigned to the Community service area.

**VLAN:** 40

**Network:** 192.172.40.0/24

**Gateway:** 192.172.40.1

Community devices connected to the appropriate access ports operate within VLAN 40.

Communication toward other VLANs is controlled at Layer 3 by the Main Router.

---

## 8. VLAN 50 - Additional Network

VLAN 50 provides a separate network for additional devices or services required by the project.

**VLAN:** 50

**Network:** 192.172.50.0/24

**Gateway:** 192.172.50.1

Keeping this network separate allows additional services to be introduced without placing them directly into another service area's broadcast domain.

---

## 9. VLAN 99 - Infrastructure / Management

VLAN 99 is dedicated to infrastructure and management purposes.

**VLAN:** 99

**Network:** 192.172.99.0/24

**Gateway:** 192.172.99.1

The purpose of VLAN 99 is to provide a dedicated logical network for infrastructure and management traffic.

VLAN 99 is not configured as a custom native VLAN.

---

## 10. Access Ports

Access ports are used for connections to end devices that belong to a single VLAN.

An access port assigns incoming untagged traffic to its configured VLAN.

For example:

**School end device → Access Port → VLAN 10**

**Clinic end device → Access Port → VLAN 20**

**Business end device → Access Port → VLAN 30**

**Community end device → Access Port → VLAN 40**

**Additional Network device → Access Port → VLAN 50**

This ensures that end devices are placed into the correct logical network.

---

## 11. Trunk Ports

Trunk ports are used between network devices when a link needs to transport VLAN traffic.

The project uses controlled trunking.

A trunk should carry only the VLANs required by the connected network segment.

Examples include:

| Network Connection | VLAN |
|--------------------|------|
| School infrastructure | 10 |
| Clinic infrastructure | 20 |
| Business infrastructure | 30 |
| Community infrastructure | 40 |
| Additional Network infrastructure | 50 |

This avoids unnecessarily extending VLANs across the entire switching infrastructure.

---

## 12. VLAN Propagation

A VLAN does not need to exist on every switch simply because it exists in the network.

VLANs should be created and permitted where they are actually required.

For example, a switch serving only the School network does not need to carry unrelated service-area VLANs across its uplink.

This approach reduces unnecessary broadcast-domain extension and keeps the network design easier to manage.

---

## 13. Broadcast Domain Separation

Each VLAN represents a separate broadcast domain.

For example:

**VLAN 10 → School broadcast domain**

**VLAN 20 → Clinic broadcast domain**

**VLAN 30 → Business broadcast domain**

**VLAN 40 → Community broadcast domain**

**VLAN 50 → Additional Network broadcast domain**

**VLAN 99 → Infrastructure / Management broadcast domain**

A broadcast generated inside one VLAN does not automatically become a broadcast in another VLAN.

---

## 14. Inter-VLAN Communication

VLAN segmentation separates networks at Layer 2.

Communication between different VLANs requires Layer 3 routing.

The Main Router provides this routing function.

The general traffic path is:

**Source Device → Access Switch → Core Switch → Main Router → Destination VLAN**

Before traffic is forwarded between VLANs, the applicable ACL policy can control whether that communication is permitted.

---

## 15. VLAN Segmentation and Security

VLANs provide logical separation, but VLANs alone are not a complete security control.

A device in one VLAN can still communicate with another VLAN if Layer 3 routing allows the traffic.

For this reason, the project combines VLAN segmentation with Extended ACLs.

The design therefore uses:

**VLANs = Network Segmentation**

**ACLs = Traffic Control**

This provides a stronger security architecture than relying on VLAN separation alone.

---

## 16. Intended Communication Policy

The network is designed to restrict unnecessary direct communication between service areas.

Examples include:

| Source | Destination | Intended Result |
|--------|-------------|-----------------|
| School | Clinic | Restricted |
| School | Business | Restricted |
| School | Community | Restricted |
| School | Additional Network | Restricted |
| Clinic | School | Restricted |
| Clinic | Business | Restricted |
| Clinic | Community | Restricted |
| Clinic | Additional Network | Restricted |
| Business | School | Restricted |
| Business | Clinic | Restricted |
| Business | Community | Restricted |
| Business | Additional Network | Restricted |
| Community | School | Restricted |
| Community | Clinic | Restricted |
| Community | Business | Restricted |
| Community | Additional Network | Restricted |
| Additional Network | School | Restricted |
| Additional Network | Clinic | Restricted |
| Additional Network | Business | Restricted |
| Additional Network | Community | Restricted |

The exact ACL implementation is documented in:

**08 - Network Security**

---

## 17. External Connectivity

VLAN segmentation does not prevent the service areas from accessing permitted external resources.

Traffic can be routed from the VLAN gateway toward the ISP Router when permitted by the security policy.

The External Cloud is used as a test destination.

**External Cloud:** 172.16.0.1

This demonstrates that network segmentation can coexist with controlled external connectivity.

---

## 18. Unused Ports

Unused switch ports should be disabled where applicable.

Disabling unused ports helps reduce unnecessary physical connectivity and prevents an unused interface from becoming an unintended entry point into the switching environment.

Port status can be verified using:

**show interfaces status**

---

## 19. VLAN Verification

The following Cisco IOS commands can be used to verify VLAN configuration:

**show vlan brief**

This confirms:

- VLAN existence
- VLAN names
- Access-port assignments

The following command can be used to verify trunk operation:

**show interfaces trunk**

This confirms:

- Active trunk interfaces
- VLANs allowed on trunks
- VLANs currently active on trunks

The MAC address table can also be inspected using:

**show mac address-table**

This helps confirm where devices are being learned within the switching infrastructure.

---

## 20. VLAN Troubleshooting Process

When an end device appears to be connected to the wrong network, troubleshoot the VLAN path systematically.

### Step 1 - Check the End Device

Confirm the device is physically connected to the expected switch port.

### Step 2 - Check the Access Port

Confirm that the port is configured for the expected VLAN.

### Step 3 - Check the VLAN

Confirm that the VLAN exists on the switch.

### Step 4 - Check the Uplink

Confirm that the required VLAN is permitted across the uplink or trunk.

### Step 5 - Check the Core Switch

Confirm that the VLAN reaches the Core Switch correctly.

### Step 6 - Check the Router

Confirm that the correct router subinterface and gateway exist for the VLAN.

### Step 7 - Check DHCP

Confirm that the device receives an address from the correct subnet.

### Step 8 - Check ACLs

If the IP configuration is correct but communication fails, verify whether an ACL is restricting the traffic.

---

## 21. Design Benefits

The VLAN architecture provides several benefits.

### Logical Separation

Each service area has its own logical network.

### Reduced Broadcast Scope

Broadcast traffic is contained within its VLAN.

### Security Foundation

VLANs provide the segmentation required for implementing traffic-control policies.

### Easier Troubleshooting

The VLAN and IPv4 structure makes it easier to identify where a device belongs.

### Scalability

Additional devices can be added to the appropriate VLAN without redesigning the entire network.

### Better Network Organisation

Different services can be managed as separate logical networks even when they share the same physical switching infrastructure.

---

## 22. VLAN Architecture Summary

The final VLAN structure is:

| VLAN | Service Area | Network | Gateway |
|------|--------------|---------|---------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 192.172.99.1 |

The VLAN design provides the Layer 2 segmentation required by the Rural Digital Access Network.

Combined with Layer 3 routing and ACLs, it creates a structured and controlled network architecture for the different service areas.
