# 02 - Network Requirements

## 1. Purpose

The Rural Digital Access Network requires a structured network infrastructure capable of supporting multiple service areas within a rural environment.

The network must provide connectivity while maintaining logical separation between different areas of the organisation.

The requirements defined in this document form the basis for the network design and implementation.

---

## 2. Functional Requirements

The network must provide the following functionality:

### 2.1 Network Connectivity

All required service areas must have network connectivity through the central network infrastructure.

The network must support communication between end devices and their configured default gateways.

---

### 2.2 Network Segmentation

Each major service area must operate within its own VLAN.

The required VLANs are:

| VLAN | Service Area |
|------|--------------|
| 10 | School |
| 20 | Clinic |
| 30 | Business |
| 40 | Community |
| 50 | Additional Network |
| 99 | Infrastructure / Management |

---

### 2.3 IPv4 Addressing

Each VLAN must use a dedicated IPv4 network.

The addressing structure must provide:

- A unique network for each VLAN
- A default gateway for each VLAN
- Sufficient host addresses for the intended devices
- A consistent addressing scheme

---

### 2.4 Automatic Address Allocation

End devices should be able to obtain IPv4 configuration automatically through DHCP.

DHCP should provide the required network parameters, including:

- IPv4 address
- Subnet mask
- Default gateway
- DNS information where configured

---

### 2.5 Inter-VLAN Routing

The network must provide Layer 3 connectivity through the Main Router.

Router-on-a-Stick is used to provide the default gateway for the VLANs and route traffic between network segments where permitted.

---

### 2.6 External Connectivity

The internal network must have a path toward the external network through the ISP Router.

The External Cloud is used as a test destination for verifying external connectivity.

The documented External Cloud address is:

**172.16.0.1**

---

## 3. Security Requirements

Network connectivity must be controlled rather than allowing unrestricted communication between all internal networks.

### 3.1 VLAN Isolation

Different service areas should remain logically separated.

For example, devices in the School network should not automatically have unrestricted access to devices in the Clinic, Business, Community, or Additional Network segments.

---

### 3.2 Access Control

Extended ACLs must be used to control traffic between internal VLANs.

The security policy must restrict unnecessary inter-VLAN communication while allowing required connectivity such as:

- Access to the local default gateway
- Required external connectivity
- Other explicitly permitted traffic

---

### 3.3 Infrastructure Management

Infrastructure and management traffic must be logically separated from normal user networks through VLAN 99.

VLAN 99 is used as the Infrastructure / Management VLAN and is not configured as a custom native VLAN.

---

## 4. Switching Requirements

The switching infrastructure must support VLAN-based segmentation.

The network must provide:

- Access ports for end devices
- Trunk connections where multiple VLANs are required
- Correct VLAN assignment
- Central connectivity through the Core Switch

Unused switch ports should be disabled where applicable.

---

## 5. Routing Requirements

The routing infrastructure must provide:

- Default gateways for internal VLANs
- Inter-VLAN routing
- A path toward the ISP Router
- Connectivity toward the External Cloud
- Appropriate routing information for return traffic

Routing must be verified using Cisco IOS commands and connectivity tests.

---

## 6. DHCP Requirements

The DHCP implementation must provide address allocation for the required internal VLANs.

The configuration should ensure that:

- Devices receive addresses from the correct network.
- Devices receive the correct default gateway.
- DHCP scopes correspond to the correct VLAN.
- Reserved gateway addresses are not accidentally assigned to clients.
- DHCP operation can be verified using Cisco IOS commands.

---

## 7. Verification Requirements

The completed network must be tested before being considered operational.

Verification must include:

### VLAN Verification

Confirm that the required VLANs exist and that access ports are assigned correctly.

### Trunk Verification

Confirm that required trunk links are operational and carrying the appropriate VLAN traffic.

### DHCP Verification

Confirm that end devices receive valid addresses from the correct DHCP network.

### Routing Verification

Confirm that the Main Router has the required connected and static routes.

### Security Verification

Confirm that ACLs enforce the intended communication restrictions.

### Connectivity Verification

Confirm that permitted destinations are reachable and restricted destinations are blocked.

---

## 8. Troubleshooting Requirements

The project must include a documented troubleshooting process.

When connectivity fails, the investigation should follow a logical sequence:

1. Check the end device.
2. Check the access port.
3. Verify VLAN assignment.
4. Verify the trunk path.
5. Check the Core Switch.
6. Check the router interface or subinterface.
7. Check IP addressing.
8. Check routing.
9. Check DHCP where applicable.
10. Check ACLs.
11. Test external connectivity.

This prevents configuration changes from being made without first identifying the likely source of the problem.

---

## 9. Documentation Requirements

The final project must document:

- Network requirements
- Network architecture
- IPv4 addressing
- VLAN configuration
- Routing configuration
- DHCP configuration
- Security configuration
- Troubleshooting procedures
- Testing results
- Device configurations
- Supporting screenshots
- Packet Tracer topology

The documentation should allow another person to understand how the network was designed, configured, tested, and secured.

---

## 10. Success Criteria

The project is considered successfully implemented when:

- All required VLANs are configured.
- Service areas are logically separated.
- End devices receive appropriate IP configuration.
- Default gateways are reachable.
- Routing operates correctly.
- External connectivity is available where permitted.
- ACLs enforce the intended security policy.
- Unauthorised inter-VLAN communication is restricted.
- Network problems can be isolated using a structured troubleshooting process.
- The final configuration and testing evidence are documented.

---

## 11. Requirements Summary

The project requirements can be summarised as:

| Requirement | Objective |
|-------------|-----------|
| Connectivity | Provide network access to required service areas |
| Segmentation | Separate service areas using VLANs |
| Addressing | Use structured IPv4 networks |
| DHCP | Automatically configure end devices |
| Routing | Provide Layer 3 connectivity |
| External Access | Provide controlled connectivity toward the external network |
| Security | Restrict unnecessary inter-VLAN communication |
| Switching | Provide correct access and trunk connectivity |
| Verification | Test the implemented network |
| Troubleshooting | Identify and resolve network failures |
| Documentation | Record the final implementation |

These requirements define the technical foundation used for the network design and implementation documented in the following sections.
