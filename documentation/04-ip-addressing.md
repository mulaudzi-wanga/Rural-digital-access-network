# 04 - IP Addressing Plan

## 1. Purpose

The Rural Digital Access Network uses a structured IPv4 addressing plan to provide predictable and scalable communication between network devices.

Each service area is assigned its own IPv4 subnet.

The addressing plan supports:

- VLAN segmentation
- Default gateways
- DHCP
- Inter-VLAN routing
- ACL security
- External connectivity
- Infrastructure management
- Future network expansion

---

## 2. Addressing Scheme

The internal network uses the following addressing structure:

**192.172.<VLAN-ID>.0/24**

This creates a simple relationship between the VLAN number and its IPv4 network.

For example:

**VLAN 10 → 192.172.10.0/24**

**VLAN 20 → 192.172.20.0/24**

This makes the network easier to understand, configure, troubleshoot, and maintain.

---

## 3. VLAN Addressing Table

| VLAN | Service Area | Network | Subnet Mask | Default Gateway |
|------|--------------|---------|-------------|-----------------|
| 10 | School | 192.172.10.0/24 | 255.255.255.0 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 255.255.255.0 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 255.255.255.0 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 255.255.255.0 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 255.255.255.0 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 255.255.255.0 | 192.172.99.1 |

---

## 4. Subnet Design

Each VLAN uses a `/24` subnet.

A `/24` network provides:

- 256 total IPv4 addresses
- 1 network address
- 1 broadcast address
- 254 usable host addresses

For example:

**Network:** 192.172.10.0

**Usable range:** 192.172.10.1 - 192.172.10.254

**Broadcast:** 192.172.10.255

The same structure applies to the other `/24` VLAN networks.

---

## 5. Default Gateway Design

The first usable address in each VLAN is reserved for the VLAN gateway.

| VLAN | Gateway |
|------|---------|
| 10 | 192.172.10.1 |
| 20 | 192.172.20.1 |
| 30 | 192.172.30.1 |
| 40 | 192.172.40.1 |
| 50 | 192.172.50.1 |
| 99 | 192.172.99.1 |

The gateway provides the Layer 3 entry point from each VLAN toward the Main Router.

For example, a School device configured with an address from the 192.172.10.0/24 network uses:

**Default Gateway: 192.172.10.1**

A Clinic device uses:

**Default Gateway: 192.172.20.1**

---

## 6. DHCP Addressing

DHCP is used to automatically provide IP configuration to end devices.

The DHCP service provides the required network information, including:

- IP address
- Subnet mask
- Default gateway
- DNS information where configured

The gateway address is reserved so that it is not accidentally assigned to an end device.

The DHCP configuration is documented separately in:

**07 - DHCP**

---

## 7. Host Addressing

The usable host range for each `/24` VLAN is:

**.1 - .254**

The network address is reserved for identifying the subnet.

The broadcast address is reserved for broadcast communication.

The remaining addresses can be used for network devices, servers, management interfaces, and end devices according to the network requirements.

---

## 8. Example Address Allocation

For VLAN 10:

| Address | Purpose |
|---------|---------|
| 192.172.10.0 | Network address |
| 192.172.10.1 | Default gateway |
| 192.172.10.2 - 192.172.10.254 | Usable host addresses |
| 192.172.10.255 | Broadcast address |

The same addressing principle applies to VLANs 20, 30, 40, 50, and 99.

---

## 9. Infrastructure / Management Addressing

VLAN 99 uses a dedicated IPv4 network:

**Network:** 192.172.99.0/24

**Default Gateway:** 192.172.99.1

This network is intended for infrastructure and management purposes.

Separating management traffic from normal user networks provides a more structured network design and allows management access policies to be controlled independently.

VLAN 99 is not configured as a custom native VLAN.

---

## 10. External Network Addressing

The internal VLAN networks use the 192.172.x.x addressing structure.

The external connectivity path uses a separate network between the internal routing infrastructure and the ISP Router.

The project also uses an External Cloud for connectivity testing.

**External Cloud:** 172.16.0.1

The external network is therefore kept separate from the internal VLAN addressing scheme.

---

## 11. Addressing and Routing Relationship

The addressing plan allows the Main Router to identify each VLAN as a separate directly connected network.

For example:

**192.172.10.0/24 → School**

**192.172.20.0/24 → Clinic**

**192.172.30.0/24 → Business**

**192.172.40.0/24 → Community**

**192.172.50.0/24 → Additional Network**

**192.172.99.0/24 → Infrastructure / Management**

This makes routing decisions predictable and simplifies troubleshooting.

---

## 12. Addressing and ACL Relationship

The addressing scheme also makes security policies easier to implement.

Because every VLAN has its own subnet, ACLs can identify traffic based on the source and destination networks.

For example:

**192.172.10.0/24 → School**

can be identified separately from:

**192.172.20.0/24 → Clinic**

This allows the Main Router to apply traffic policies between specific service areas.

The ACL implementation is documented in:

**08 - Network Security**

---

## 13. Addressing Validation

The addressing configuration should be verified using Cisco IOS commands and end-device testing.

Useful verification commands include:

- `show ip interface brief`
- `show ip route`
- `show ip dhcp pool`
- `show ip dhcp binding`

End devices should also be checked to confirm that they received:

- The correct subnet
- The correct subnet mask
- The correct default gateway
- A valid IPv4 address

---

## 14. Troubleshooting Using the Addressing Plan

The addressing structure provides a quick way to identify configuration problems.

For example:

If a School device receives an address from the 192.172.20.0/24 network, the device may have been placed in the wrong VLAN or DHCP scope.

If a device receives an address outside the expected VLAN subnet, DHCP or VLAN configuration should be investigated.

If the IP address is correct but the default gateway is incorrect, the DHCP configuration should be checked.

If the IP configuration is correct but external connectivity fails, routing and ACL configuration should be investigated.

---

## 15. Design Benefits

The addressing plan provides several operational benefits.

### Consistency

The VLAN number corresponds directly with the third octet of the IPv4 network.

### Simplicity

Network administrators can quickly identify which service area an address belongs to.

### Troubleshooting

Incorrect addresses can be identified quickly by comparing them with the expected VLAN subnet.

### Security

Separate subnets make it easier to create ACL policies for individual service areas.

### Scalability

Additional networks can be added using the same structured addressing approach.

---

## 16. Addressing Summary

The final internal addressing structure is:

| VLAN | Service Area | IPv4 Network | Gateway |
|------|--------------|--------------|---------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 192.172.99.1 |

The addressing plan provides the foundation for VLAN segmentation, DHCP, routing, ACL security, and network troubleshooting throughout the project.
