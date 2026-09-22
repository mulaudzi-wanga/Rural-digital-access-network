# 06 - Routing and Connectivity

## 1. Purpose

Routing provides communication between the different networks within the Rural Digital Access Network and provides a path toward the external network.

The Main Router performs the primary Layer 3 functions for the internal VLANs.

The routing design provides:

- Default gateways for VLANs
- Inter-VLAN routing
- Connectivity toward the ISP Router
- External connectivity
- Controlled traffic forwarding
- A structured path for troubleshooting

---

## 2. Routing Architecture

The routing path is structured as:

**End Device → Access Switch → Core Switch → Main Router → ISP Router → External Cloud**

The Main Router is the central Layer 3 device for the internal VLANs.

The Core Switch provides the Layer 2 path between the access switches and the Main Router.

---

## 3. Router-on-a-Stick

Router-on-a-Stick is used to provide Layer 3 connectivity for multiple VLANs through a single physical router interface.

The Main Router uses subinterfaces associated with the different VLANs.

Each subinterface provides the default gateway for its corresponding VLAN.

The logical relationship is:

| VLAN | Router Subinterface | Gateway |
|------|---------------------|---------|
| 10 | VLAN 10 subinterface | 192.172.10.1 |
| 20 | VLAN 20 subinterface | 192.172.20.1 |
| 30 | VLAN 30 subinterface | 192.172.30.1 |
| 40 | VLAN 40 subinterface | 192.172.40.1 |
| 50 | VLAN 50 subinterface | 192.172.50.1 |
| 99 | VLAN 99 subinterface | 192.172.99.1 |

The exact physical interface and subinterface configuration are stored in the device configuration files within the repository.

---

## 4. Default Gateways

Each VLAN has its own default gateway.

| VLAN | Network | Default Gateway |
|------|---------|-----------------|
| 10 | 192.172.10.0/24 | 192.172.10.1 |
| 20 | 192.172.20.0/24 | 192.172.20.1 |
| 30 | 192.172.30.0/24 | 192.172.30.1 |
| 40 | 192.172.40.0/24 | 192.172.40.1 |
| 50 | 192.172.50.0/24 | 192.172.50.1 |
| 99 | 192.172.99.0/24 | 192.172.99.1 |

The default gateway is the device a host uses when the destination is outside its local subnet.

For example, a School device in 192.172.10.0/24 sends traffic for another network toward 192.172.10.1.

---

## 5. How Inter-VLAN Routing Works

Consider a device in the School VLAN communicating with another network.

The traffic follows this process:

1. The School device determines that the destination is outside its local subnet.
2. The device forwards the packet to 192.172.10.1.
3. The Main Router receives the packet.
4. The router checks its routing table.
5. The applicable ACL policy is evaluated.
6. If the traffic is permitted, the router forwards the packet toward the destination network.

The same process applies to the other VLANs.

---

## 6. Directly Connected Networks

The Main Router directly connects to the VLAN networks through its VLAN subinterfaces.

The expected internal networks include:

- 192.172.10.0/24
- 192.172.20.0/24
- 192.172.30.0/24
- 192.172.40.0/24
- 192.172.50.0/24
- 192.172.99.0/24

These networks should appear as connected routes in the router's routing table when the corresponding interfaces are operational.

The routing table can be checked using:

**show ip route**

---

## 7. External Routing

Traffic destined for external networks must leave the internal VLAN environment through the Main Router.

The general path is:

**Internal VLAN → Main Router → ISP Router → External Network**

The ISP Router provides the next part of the path toward the External Cloud.

The routing configuration must therefore provide a valid path from the internal network toward the ISP Router.

Return traffic must also have a valid route back toward the internal networks.

---

## 8. External Cloud

The External Cloud is used as a connectivity test destination.

**External Cloud:** 172.16.0.1

Testing connectivity to the External Cloud helps verify that multiple parts of the network are working together.

A successful test can indicate that:

- The end device has valid IP configuration.
- The default gateway is reachable.
- The Main Router is forwarding traffic.
- The external route is available.
- The ISP path is operational.
- The security policy permits the traffic.

---

## 9. Routing and ACL Interaction

Routing determines where a packet should go.

ACLs determine whether the packet should be allowed.

These functions work together.

For example:

**School → Clinic**

The router knows where the Clinic network is located, but the ACL can prevent the traffic from being forwarded.

For permitted external traffic:

**School → External Cloud**

The router can forward the traffic toward the external network if the ACL permits it and the required route exists.

Therefore:

**Routing = Path Selection**

**ACL = Traffic Control**

---

## 10. Static and Default Routing

Routes toward external networks may be configured using static routing or a default route depending on the final topology.

A static route identifies a specific destination network and the next-hop path to reach it.

A default route provides a general path for destinations that are not already present in the routing table.

The exact routes configured on the project devices should be verified using:

**show ip route**

and:

**show running-config | include ip route**

This ensures that the documented routing information matches the implemented Packet Tracer topology.

---

## 11. Routing Verification

The following commands are useful for verifying the Main Router.

### Check Interface Status

**show ip interface brief**

This verifies:

- Interface status
- IP addresses
- VLAN subinterfaces
- Operational state

### Check Routing Table

**show ip route**

This verifies:

- Connected networks
- Static routes
- Default routes
- Routing information

### Check Interface Configuration

**show running-config | section interface**

This helps verify the configured router interfaces and subinterfaces.

### Check Static Routes

**show running-config | include ip route**

This displays configured static routes.

---

## 12. Gateway Testing

Each VLAN gateway should be tested from an appropriate end device.

Examples include:

**192.172.10.1**

**192.172.20.1**

**192.172.30.1**

**192.172.40.1**

**192.172.50.1**

Successful gateway testing confirms that the device can reach its Layer 3 entry point.

If a gateway cannot be reached, routing should not be investigated first.

The Layer 2 path, VLAN assignment, IP addressing, and interface status should be checked first.

---

## 13. External Connectivity Testing

External connectivity can be tested using ICMP.

The External Cloud address is:

**172.16.0.1**

A successful ping demonstrates connectivity through the routing path.

Testing should be performed from multiple VLANs where appropriate.

This helps confirm that the routing configuration is not working for only one network.

---

## 14. Routing Troubleshooting Process

When routing fails, use a structured troubleshooting process.

### Step 1 - Check the End Device

Confirm:

- IP address
- Subnet mask
- Default gateway

### Step 2 - Check the Access Port

Confirm that the device is connected to the correct VLAN.

### Step 3 - Check the VLAN

Verify the VLAN using:

**show vlan brief**

### Step 4 - Check the Trunk

Verify required VLANs on trunk links using:

**show interfaces trunk**

### Step 5 - Check the Router Interface

Use:

**show ip interface brief**

Confirm that the required router interface or subinterface is operational.

### Step 6 - Test the Gateway

Ping the VLAN gateway.

### Step 7 - Check the Routing Table

Use:

**show ip route**

Confirm that the destination network or default route exists.

### Step 8 - Check ACLs

Use:

**show ip access-lists**

Determine whether the traffic is being denied by the security policy.

### Step 9 - Test External Connectivity

Ping the External Cloud:

**172.16.0.1**

This determines whether the complete routing path is operational.

---

## 15. Common Routing Problems

### Incorrect Default Gateway

If an end device has the wrong gateway, traffic destined for other networks will not be forwarded correctly.

### Incorrect VLAN

If a device is placed in the wrong VLAN, it may receive an address from the wrong subnet.

### Inactive Router Interface

If the router subinterface or physical interface is not operational, the associated VLAN cannot use the gateway.

### Missing Route

If the router does not have a route toward an external network, the traffic cannot reach that destination.

### Missing Return Route

Traffic may leave the internal network successfully but fail to return if the remote router does not know how to reach the internal networks.

### ACL Restriction

Correct routing does not guarantee connectivity.

An ACL can intentionally block traffic even when the routing table contains the correct route.

---

## 16. Routing Design Principles

The routing design follows several principles.

### Centralised Layer 3 Routing

The Main Router provides the primary Layer 3 gateway functions.

### Logical Separation

Each VLAN is treated as a separate IPv4 network.

### Controlled Inter-VLAN Communication

ACLs determine which routed traffic is permitted.

### External Connectivity

The ISP Router provides the path toward external networks.

### Verification

Routing is validated through interface checks, routing-table inspection, gateway tests, and external connectivity tests.

---

## 17. Connectivity Model

The complete connectivity model can be summarised as:

**End Device**

↓  

**Access VLAN**

↓

**Access Switch**

↓

**Core Switch**

↓

**Main Router**

↓

**Routing and ACL Decision**

↓

**ISP Router**

↓

**External Cloud**

For internal destinations, the traffic is routed toward the appropriate internal VLAN when permitted.

For restricted internal destinations, the ACL prevents the communication.

---

## 18. Routing Summary

The routing architecture provides:

- Default gateways for all required VLANs
- Router-on-a-Stick inter-VLAN routing
- Connectivity between the switching and routing layers
- A path toward the ISP Router
- External connectivity testing
- ACL-controlled traffic forwarding
- Structured troubleshooting and verification

The Main Router therefore acts as the central Layer 3 control point for the Rural Digital Access Network.

The routing implementation works together with VLAN segmentation, DHCP, and ACL security to provide controlled network connectivity.
