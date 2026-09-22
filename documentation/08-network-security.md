# 08 - Network Security

## 1. Purpose

Security is implemented through network segmentation, controlled routing, and Extended Access Control Lists (ACLs).

The primary security objective is to prevent unnecessary communication between separate service-area networks while still allowing required connectivity such as access to the local gateway and permitted external resources.

The security design uses:

- VLAN segmentation
- Layer 3 traffic control
- Extended ACLs
- Infrastructure / Management separation
- Controlled external connectivity
- Disabled unused switch ports where applicable
- Security verification testing

---

## 2. Security Architecture

The network uses a layered approach:

**VLANs → Separate Networks**

**Routing → Controls the Layer 3 path**

**ACLs → Control permitted traffic**

**Port Security Practice → Reduces unnecessary physical access**

The combination provides stronger control than relying on VLAN segmentation alone.

---

## 3. VLAN Segmentation

Each service area is assigned to a separate VLAN.

| VLAN | Service Area | Network |
|------|--------------|---------|
| 10 | School | 192.172.10.0/24 |
| 20 | Clinic | 192.172.20.0/24 |
| 30 | Business | 192.172.30.0/24 |
| 40 | Community | 192.172.40.0/24 |
| 50 | Additional Network | 192.172.50.0/24 |
| 99 | Infrastructure / Management | 192.172.99.0/24 |

This creates separate broadcast domains and provides the foundation for applying network security policies.

---

## 4. Inter-VLAN Security Policy

The network is designed to restrict unnecessary communication between service areas.

The intended policy is:

| Source VLAN | Destination VLAN | Policy |
|-------------|------------------|--------|
| 10 | 20 | Deny |
| 10 | 30 | Deny |
| 10 | 40 | Deny |
| 10 | 50 | Deny |
| 20 | 10 | Deny |
| 20 | 30 | Deny |
| 20 | 40 | Deny |
| 20 | 50 | Deny |
| 30 | 10 | Deny |
| 30 | 20 | Deny |
| 30 | 40 | Deny |
| 30 | 50 | Deny |
| 40 | 10 | Deny |
| 40 | 20 | Deny |
| 40 | 30 | Deny |
| 40 | 50 | Deny |
| 50 | 10 | Deny |
| 50 | 20 | Deny |
| 50 | 30 | Deny |
| 50 | 40 | Deny |

This prevents unrestricted lateral communication between the main service-area networks.

---

## 5. Extended ACLs

Extended ACLs are used because the project needs to control traffic based on both source and destination IP networks.

The ACLs are applied to the relevant VLAN routing interfaces on the Main Router.

The general policy is:

**Deny restricted internal destinations**

followed by:

**Permit other required traffic**

This allows the router to block specific inter-VLAN communication while permitting traffic that is required for external connectivity.

---

## 6. VLAN 10 Security Policy

VLAN 10 represents the School network.

**Network:** 192.172.10.0/24

The School network is prevented from initiating traffic toward:

- 192.172.20.0/24
- 192.172.30.0/24
- 192.172.40.0/24
- 192.172.50.0/24

Traffic that is not explicitly blocked by the policy can continue toward permitted destinations.

---

## 7. VLAN 20 Security Policy

VLAN 20 represents the Clinic network.

**Network:** 192.172.20.0/24

The Clinic network is prevented from initiating traffic toward:

- 192.172.10.0/24
- 192.172.30.0/24
- 192.172.40.0/24
- 192.172.50.0/24

Traffic toward permitted destinations remains available.

---

## 8. VLAN 30 Security Policy

VLAN 30 represents the Business network.

**Network:** 192.172.30.0/24

The Business network is prevented from initiating traffic toward:

- 192.172.10.0/24
- 192.172.20.0/24
- 192.172.40.0/24
- 192.172.50.0/24

Traffic toward permitted destinations remains available.

---

## 9. VLAN 40 Security Policy

VLAN 40 represents the Community network.

**Network:** 192.172.40.0/24

The Community network is prevented from initiating traffic toward:

- 192.172.10.0/24
- 192.172.20.0/24
- 192.172.30.0/24
- 192.172.50.0/24

Traffic toward permitted destinations remains available.

---

## 10. VLAN 50 Security Policy

VLAN 50 represents the Additional Network.

**Network:** 192.172.50.0/24

The Additional Network is prevented from initiating traffic toward:

- 192.172.10.0/24
- 192.172.20.0/24
- 192.172.30.0/24
- 192.172.40.0/24

Traffic toward permitted destinations remains available.

---

## 11. External Connectivity

The security policy is designed to restrict unnecessary internal lateral movement without preventing required external connectivity.

The External Cloud is:

**172.16.0.1**

The intended traffic model is:

**Internal VLAN → Main Router → ACL Evaluation → ISP Router → External Network**

If the traffic is permitted, it can continue toward the external network.

This demonstrates that network segmentation does not have to prevent useful external connectivity.

---

## 12. Gateway Access

Each VLAN requires access to its own default gateway.

| VLAN | Gateway |
|------|---------|
| 10 | 192.172.10.1 |
| 20 | 192.172.20.1 |
| 30 | 192.172.30.1 |
| 40 | 192.172.40.1 |
| 50 | 192.172.50.1 |
| 99 | 192.172.99.1 |

Gateway connectivity is required for normal Layer 3 communication.

A successful gateway ping confirms that the device can reach the first Layer 3 hop.

---

## 13. Infrastructure / Management VLAN

VLAN 99 is reserved for infrastructure and management traffic.

**Network:** 192.172.99.0/24

**Gateway:** 192.172.99.1

Separating infrastructure traffic from normal user networks provides a dedicated logical area for network management.

VLAN 99 is not configured as a custom native VLAN.

---

## 14. Unused Switch Ports

Unused switch ports are disabled where applicable.

This reduces unnecessary physical access to the switching infrastructure.

The status of switch ports can be checked using:

**show interfaces status**

Disabled interfaces should be distinguishable from active interfaces during an infrastructure audit.

---

## 15. Controlled Trunking

Trunk links are configured to carry only the VLANs required by the relevant network connection.

This reduces unnecessary VLAN propagation.

For example:

**School infrastructure → VLAN 10**

**Clinic infrastructure → VLAN 20**

**Business infrastructure → VLAN 30**

**Community infrastructure → VLAN 40**

**Additional Network infrastructure → VLAN 50**

This approach reduces the number of networks unnecessarily exposed across individual trunk paths.

---

## 16. Security Verification

Security controls must be tested rather than assumed to be working.

The following tests are used.

### Gateway Test

From each VLAN, test the local gateway.

Expected result:

**Permitted**

### Inter-VLAN Test

Test communication from one service VLAN toward another service VLAN.

Example:

**VLAN 10 → VLAN 20**

Expected result:

**Blocked**

### External Test

Test communication toward the External Cloud.

**172.16.0.1**

Expected result:

**Permitted where allowed by the security policy**

---

## 17. ACL Verification Commands

The following command can be used to inspect configured ACLs:

**show ip access-lists**

This allows the administrator to verify:

- ACL names
- Permit statements
- Deny statements
- Matching traffic counters

Traffic counters are particularly useful because they provide evidence that ACL entries are being matched.

---

## 18. Router Security Verification

The following commands can also be used during a security audit.

### View Interface Configuration

**show running-config | section interface**

This helps identify ACLs applied to router interfaces.

### View ACL Configuration

**show running-config | section access-list**

This displays configured access-list statements.

### View Routing Information

**show ip route**

This confirms the available routing paths.

### View Interface Status

**show ip interface brief**

This confirms the operational state of router interfaces and subinterfaces.

---

## 19. Security Testing Matrix

| Test | Expected Result |
|------|-----------------|
| VLAN 10 → VLAN 10 gateway | Permitted |
| VLAN 20 → VLAN 20 gateway | Permitted |
| VLAN 30 → VLAN 30 gateway | Permitted |
| VLAN 40 → VLAN 40 gateway | Permitted |
| VLAN 50 → VLAN 50 gateway | Permitted |
| VLAN 10 → VLAN 20 | Blocked |
| VLAN 10 → VLAN 30 | Blocked |
| VLAN 20 → VLAN 30 | Blocked |
| VLAN 20 → VLAN 40 | Blocked |
| VLAN 30 → VLAN 40 | Blocked |
| VLAN 40 → VLAN 50 | Blocked |
| VLAN 50 → VLAN 10 | Blocked |
| Internal VLAN → External Cloud | Permitted where allowed |

The exact testing evidence is recorded in the project's testing documentation.

---

## 20. Troubleshooting Security Issues

If a permitted connection fails, ACLs should be checked after verifying basic connectivity.

The troubleshooting sequence should be:

1. Verify the device IP address.
2. Verify the subnet mask.
3. Verify the default gateway.
4. Verify VLAN assignment.
5. Verify the trunk path.
6. Verify the router interface.
7. Verify the routing table.
8. Check the ACL configuration.
9. Check ACL match counters.
10. Repeat the connectivity test.

This prevents ACL changes from being made before the underlying network path has been verified.

---

## 21. Security Design Principles

The project follows several security principles.

### Least Required Access

Traffic between service areas is restricted unless communication is required.

### Segmentation

Separate service areas operate in separate VLANs and IPv4 networks.

### Centralised Control

The Main Router provides a central point for Layer 3 traffic control.

### Controlled External Access

External connectivity is permitted through the routing and ACL policy rather than allowing unrestricted internal communication.

### Infrastructure Separation

Management traffic is separated using VLAN 99.

### Reduced Attack Surface

Unused switch ports are disabled where applicable and unnecessary VLAN propagation is avoided.

---

## 22. Security Limitations

This project is a Packet Tracer network engineering implementation and focuses primarily on network-layer segmentation and traffic control.

The implemented security controls do not represent a complete production enterprise security architecture.

Additional production controls could include:

- Dedicated firewalls
- Network monitoring
- Centralised logging
- Authentication services
- Endpoint security
- Intrusion detection and prevention
- Secure management protocols
- Backup and recovery controls

These are outside the primary scope of this project.

---

## 23. Security Outcome

The implemented security design provides controlled separation between the main service areas.

The combination of VLAN segmentation and Extended ACLs prevents unnecessary inter-VLAN communication while maintaining required gateway and external connectivity.

The security implementation therefore demonstrates practical understanding of:

- VLAN segmentation
- Layer 3 security
- Extended ACLs
- Traffic filtering
- Network isolation
- Security verification
- Network troubleshooting

This security architecture forms the basis for the testing and validation documented in the next sections.
