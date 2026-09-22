# 10 - Testing and Validation

## 1. Purpose

Testing and validation are used to confirm that the Rural Digital Access Network operates according to the intended design.

Testing verifies:

- VLAN segmentation
- IP addressing
- DHCP operation
- Default gateway connectivity
- Inter-VLAN security
- External connectivity
- Routing
- ACL behaviour
- Network path functionality

The tests provide evidence that the implemented configuration matches the documented network design.

---

## 2. Testing Methodology

Testing is performed progressively from local connectivity toward external connectivity.

The general testing sequence is:

**Physical Connectivity → VLAN → IP Addressing → Gateway → Routing → ACL → External Connectivity**

This approach helps isolate faults and prevents higher-level testing from being performed before basic connectivity has been confirmed.

---

## 3. VLAN Validation

The VLAN configuration is verified on the relevant switches.

Use:

**show vlan brief**

The command confirms that the required VLANs exist and that access ports are assigned correctly.

The expected VLAN structure is:

| VLAN | Service Area |
|------|--------------|
| 10 | School |
| 20 | Clinic |
| 30 | Business |
| 40 | Community |
| 50 | Additional Network |
| 99 | Infrastructure / Management |

Each end device should be connected to the appropriate access VLAN.

---

## 4. Trunk Validation

Trunk links are verified using:

**show interfaces trunk**

The test confirms that:

- Required trunk interfaces are operational.
- Required VLANs are permitted.
- VLAN traffic can travel between network devices.
- The Layer 2 path is correctly established.

A VLAN that is not carried across a required trunk can prevent devices from reaching their correct gateway and DHCP service.

---

## 5. IP Address Validation

End devices are checked to ensure that their addresses belong to the correct VLAN subnet.

The expected networks are:

| VLAN | Expected Network |
|------|------------------|
| 10 | 192.172.10.0/24 |
| 20 | 192.172.20.0/24 |
| 30 | 192.172.30.0/24 |
| 40 | 192.172.40.0/24 |
| 50 | 192.172.50.0/24 |
| 99 | 192.172.99.0/24 |

A device receiving an address from another VLAN indicates that the VLAN, DHCP, or Layer 2 path requires investigation.

---

## 6. DHCP Validation

DHCP operation is verified from end devices and the Main Router.

On the Main Router, use:

**show ip dhcp pool**

This confirms that the required DHCP pools exist.

Use:

**show ip dhcp binding**

This confirms dynamically assigned addresses.

Each client should receive:

- Correct IPv4 address
- `/24` subnet mask
- Correct default gateway
- DNS information where configured

For example, a School client should receive an address from:

**192.172.10.0/24**

with:

**192.172.10.1**

as its default gateway.

---

## 7. Gateway Connectivity Testing

Each VLAN should be able to reach its own default gateway.

### VLAN 10

**ping 192.172.10.1**

Expected result:

**Successful**

### VLAN 20

**ping 192.172.20.1**

Expected result:

**Successful**

### VLAN 30

**ping 192.172.30.1**

Expected result:

**Successful**

### VLAN 40

**ping 192.172.40.1**

Expected result:

**Successful**

### VLAN 50

**ping 192.172.50.1**

Expected result:

**Successful**

A successful gateway test confirms basic Layer 3 connectivity between the client and its router interface.

---

## 8. Inter-VLAN Security Testing

Inter-VLAN communication is tested to verify that ACL security policies are operating as intended.

Example test:

**VLAN 10 → VLAN 20**

Expected result:

**Blocked**

Additional tests include:

| Source | Destination | Expected Result |
|--------|-------------|-----------------|
| VLAN 10 | VLAN 20 | Blocked |
| VLAN 10 | VLAN 30 | Blocked |
| VLAN 20 | VLAN 30 | Blocked |
| VLAN 20 | VLAN 40 | Blocked |
| VLAN 30 | VLAN 40 | Blocked |
| VLAN 40 | VLAN 50 | Blocked |
| VLAN 50 | VLAN 10 | Blocked |

These tests verify that separate service areas cannot communicate freely with one another.

---

## 9. External Connectivity Testing

External connectivity is tested after internal connectivity has been verified.

The project's External Cloud is:

**172.16.0.1**

The expected path is:

**End Device → VLAN Gateway → Main Router → ISP Router → External Network**

The test command is:

**ping 172.16.0.1**

Expected result:

**Successful where permitted by the security policy**

Successful external connectivity confirms that internal VLAN clients can reach the external network through the configured routing path.

---

## 10. Routing Validation

The Main Router routing table is checked using:

**show ip route**

The routing table should contain the required internal VLAN networks.

The administrator should verify:

- VLAN 10 network
- VLAN 20 network
- VLAN 30 network
- VLAN 40 network
- VLAN 50 network
- VLAN 99 network
- External routes
- Default route where configured

Routing verification confirms that the Main Router has paths to the required destinations.

---

## 11. ACL Validation

ACL behaviour is verified using:

**show ip access-lists**

The output can be used to confirm:

- Configured ACL names
- Permit statements
- Deny statements
- Traffic match counters

A deny counter increasing during an inter-VLAN test provides evidence that the ACL is matching the restricted traffic.

ACL configuration can also be inspected using:

**show running-config | section access-list**

---

## 12. Router Interface Validation

Router interfaces and subinterfaces are checked using:

**show ip interface brief**

The expected VLAN gateway interfaces should be operational.

The administrator should verify that the corresponding interfaces have the expected gateway addresses.

For example:

| VLAN | Gateway |
|------|---------|
| 10 | 192.172.10.1 |
| 20 | 192.172.20.1 |
| 30 | 192.172.30.1 |
| 40 | 192.172.40.1 |
| 50 | 192.172.50.1 |
| 99 | 192.172.99.1 |

---

## 13. Access Switch Validation

Access switches are tested to ensure that end devices are placed into the correct VLAN.

Use:

**show vlan brief**

to verify access-port assignments.

Use:

**show interfaces trunk**

to verify the uplink to the Core Switch.

The expected path is:

**End Device → Access Port → Access Switch → Trunk → Core Switch → Main Router**

A failure at any point in this path can affect DHCP and connectivity.

---

## 14. End-to-End Testing

End-to-end testing verifies the complete network path.

A typical test follows:

**Client → Access Switch → Core Switch → Main Router → ISP Router → External Cloud**

The following should be confirmed:

1. Client receives an IP address.
2. Client has the correct subnet mask.
3. Client has the correct default gateway.
4. Client reaches its gateway.
5. Restricted internal destinations are blocked.
6. Permitted external destinations are reachable.

This verifies multiple network components together rather than testing each device in isolation.

---

## 15. Testing Matrix

| Test | Expected Result | Purpose |
|------|-----------------|---------|
| VLAN 10 client receives DHCP address | Successful | Verify DHCP |
| VLAN 20 client receives DHCP address | Successful | Verify DHCP |
| VLAN 30 client receives DHCP address | Successful | Verify DHCP |
| VLAN 40 client receives DHCP address | Successful | Verify DHCP |
| VLAN 50 client receives DHCP address | Successful | Verify DHCP |
| VLAN 10 → Gateway | Successful | Verify Layer 3 connectivity |
| VLAN 20 → Gateway | Successful | Verify Layer 3 connectivity |
| VLAN 30 → Gateway | Successful | Verify Layer 3 connectivity |
| VLAN 40 → Gateway | Successful | Verify Layer 3 connectivity |
| VLAN 50 → Gateway | Successful | Verify Layer 3 connectivity |
| VLAN 10 → VLAN 20 | Blocked | Verify ACL isolation |
| VLAN 10 → VLAN 30 | Blocked | Verify ACL isolation |
| VLAN 20 → VLAN 30 | Blocked | Verify ACL isolation |
| VLAN 20 → VLAN 40 | Blocked | Verify ACL isolation |
| VLAN 30 → VLAN 40 | Blocked | Verify ACL isolation |
| VLAN 40 → VLAN 50 | Blocked | Verify ACL isolation |
| VLAN 50 → VLAN 10 | Blocked | Verify ACL isolation |
| Internal VLAN → External Cloud | Successful where permitted | Verify external connectivity |

---

## 16. Troubleshooting During Testing

If a test fails, the network is investigated systematically.

The following order is used:

### 1. Physical

Check cables and interface status.

### 2. VLAN

Confirm that the device is connected to the correct VLAN.

### 3. Trunk

Confirm that the VLAN is carried across required uplinks.

### 4. IP Addressing

Verify the client's address, subnet mask, and gateway.

### 5. DHCP

Verify the DHCP pool and bindings.

### 6. Gateway

Ping the local VLAN gateway.

### 7. Routing

Inspect the routing table.

### 8. ACL

Check whether traffic is intentionally blocked.

### 9. External Path

Verify the ISP and external network path.

This process helps identify the location and cause of a failed test.

---

## 17. DHCP Failure Validation

A DHCP failure can be isolated by checking the following:

**show vlan brief**

**show interfaces trunk**

**show ip interface brief**

**show ip dhcp pool**

**show ip dhcp binding**

The expected result is that the client receives an address from the DHCP pool associated with its VLAN.

For example:

**VLAN 40 → 192.172.40.x**

If the client instead receives an address from another network, the Layer 2 path and DHCP configuration should be investigated.

---

## 18. Security Validation

Security validation confirms that segmentation is not only configured but also functioning.

The tests verify two important behaviours:

### Required Local Connectivity

Clients can reach their own default gateway.

### Restricted Internal Connectivity

Clients cannot freely communicate with other service-area VLANs.

### Required External Connectivity

Clients can reach permitted external resources.

This demonstrates that the network provides both connectivity and traffic control.

---

## 19. Evidence Collection

Testing evidence can be collected using:

- Packet Tracer screenshots
- Ping results
- IP configuration output
- Router command output
- Switch command output
- DHCP bindings
- ACL counters
- Routing table output

Evidence should show both successful and intentionally blocked tests where appropriate.

This allows the final project documentation to demonstrate that the network was actually tested.

---

## 20. Test Result Recording

When recording a test, use the following structure:

| Field | Description |
|------|-------------|
| Test ID | Unique test number |
| Source | Device or VLAN initiating the test |
| Destination | Target device, network, or service |
| Expected Result | Intended behaviour |
| Actual Result | Observed behaviour |
| Status | Pass / Fail |
| Evidence | Screenshot or command output |
| Notes | Additional observations |

Example:

| Test ID | Source | Destination | Expected | Actual | Status |
|---------|--------|-------------|----------|--------|--------|
| T01 | VLAN 10 Client | 192.172.10.1 | Reachable | Reachable | Pass |
| T02 | VLAN 10 Client | VLAN 20 Client | Blocked | Blocked | Pass |
| T03 | VLAN 10 Client | 172.16.0.1 | Reachable | Reachable | Pass |

Actual results should be updated with the results observed during final testing.

---

## 21. Validation Commands

The following commands form the primary verification toolkit.

### Switch Commands

**show vlan brief**

**show interfaces status**

**show interfaces trunk**

**show running-config**

### Router Commands

**show ip interface brief**

**show ip route**

**show ip dhcp pool**

**show ip dhcp binding**

**show ip access-lists**

**show running-config**

### Connectivity Commands

**ping**

These commands provide visibility into the state of the network during testing and troubleshooting.

---

## 22. Validation Criteria

The network is considered validated when:

- Required VLANs exist.
- Access ports are correctly assigned.
- Trunk links operate correctly.
- End devices receive appropriate IP addresses.
- DHCP pools operate correctly.
- Default gateways are reachable.
- Internal networks are correctly routed.
- Restricted inter-VLAN traffic is blocked.
- Required external connectivity works.
- ACLs match the intended security policy.
- Network faults can be isolated using the documented troubleshooting process.

---

## 23. Testing Outcome

Testing and validation provide evidence that the Rural Digital Access Network operates according to its intended design.

The testing process verifies the interaction between:

**VLANs + DHCP + Routing + ACLs + External Connectivity**

The project therefore demonstrates not only network configuration but also the ability to verify, troubleshoot, and document the behaviour of a multi-VLAN network.

---

## 24. Summary

The testing process follows a structured progression from basic connectivity to end-to-end validation.

The main validation areas are:

- VLAN configuration
- Trunking
- DHCP
- IP addressing
- Gateway connectivity
- Routing
- ACL security
- External connectivity

Testing results should be retained as project evidence and referenced alongside the final Packet Tracer topology and configuration documentation.
