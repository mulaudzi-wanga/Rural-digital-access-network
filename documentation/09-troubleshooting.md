# 09 - Troubleshooting

## 1. Purpose

Troubleshooting is used to identify, isolate, and resolve network connectivity problems within the Rural Digital Access Network.

The troubleshooting approach follows a structured process rather than changing configurations randomly.

The main areas investigated include:

- Physical connectivity
- VLAN assignment
- Trunk links
- IP addressing
- DHCP
- Default gateways
- Routing
- ACLs
- External connectivity
- End-device configuration

---

## 2. Troubleshooting Methodology

Network problems are investigated from the lower layers upward.

The general troubleshooting sequence is:

**Physical → Layer 2 → Layer 3 → Routing → Security → Application**

This prevents higher-level configuration changes from being made before the underlying connectivity has been verified.

---

## 3. Step 1 - Physical Connectivity

The first step is to verify that devices are physically connected.

Check:

- Ethernet cables
- Switch ports
- Router interfaces
- Access points
- End-device connections
- Interface status

On a switch, use:

**show interfaces status**

On a router, use:

**show ip interface brief**

An interface that is administratively down or physically down must be investigated before moving to higher layers.

---

## 4. Step 2 - Verify VLAN Assignment

If an end device is receiving an unexpected IP address or cannot communicate, verify its VLAN.

Use:

**show vlan brief**

The command can confirm:

- VLAN existence
- Access-port assignments
- Active VLAN membership

For example:

**FastEthernet0/5 → VLAN 20**

If the port is assigned to the wrong VLAN, the device may receive addressing from the wrong network or fail to communicate correctly.

---

## 5. Step 3 - Verify Trunk Links

When an end device connects through an additional access switch, the required VLAN must be transported across the uplink.

Use:

**show interfaces trunk**

Verify:

- The uplink is operating as a trunk.
- The required VLAN exists.
- The required VLAN is allowed across the trunk.
- The expected native VLAN is configured where applicable.

A missing VLAN on a trunk can prevent clients from reaching the correct router subinterface.

---

## 6. Step 4 - Verify IP Addressing

After confirming the Layer 2 path, verify the client's IP configuration.

The client should receive an address belonging to its VLAN.

Example:

**VLAN 10 → 192.172.10.x**

**VLAN 20 → 192.172.20.x**

**VLAN 30 → 192.172.30.x**

**VLAN 40 → 192.172.40.x**

**VLAN 50 → 192.172.50.x**

The client should also have:

- Correct subnet mask
- Correct default gateway
- Correct DNS information where configured

An address from the wrong subnet is an important troubleshooting indicator.

---

## 7. Step 5 - Verify DHCP

If a client does not receive an IP address automatically, check the DHCP service on the Main Router.

Use:

**show ip dhcp pool**

This verifies the configured DHCP pools.

Use:

**show ip dhcp binding**

This displays addresses currently assigned to clients.

The DHCP pool must match the VLAN network.

For example:

**VLAN 20 → 192.172.20.0/24**

with:

**Gateway → 192.172.20.1**

---

## 8. Step 6 - Test the Default Gateway

Once the client has a valid IP address, test the local gateway.

For VLAN 10:

**ping 192.172.10.1**

For VLAN 20:

**ping 192.172.20.1**

For VLAN 30:

**ping 192.172.30.1**

For VLAN 40:

**ping 192.172.40.1**

For VLAN 50:

**ping 192.172.50.1**

A successful gateway ping confirms that the client can reach the first Layer 3 hop.

If the gateway cannot be reached, investigate VLAN configuration, trunking, IP addressing, or the router interface before checking external connectivity.

---

## 9. Step 7 - Verify Routing

If the local gateway is reachable but another network cannot be reached, inspect the routing table.

Use:

**show ip route**

The routing table should contain the required internal networks and appropriate external routes.

The administrator should verify:

- Connected VLAN networks
- Static routes where configured
- Default routes where configured
- Next-hop addresses
- External network paths

---

## 10. Step 8 - Verify ACLs

If routing is correct but traffic is still blocked, inspect the ACL configuration.

Use:

**show ip access-lists**

This displays ACL entries and traffic match counters.

Also use:

**show running-config | section access-list**

to inspect the configured access-list statements.

ACLs should be checked carefully because a deny statement can intentionally block traffic between VLANs.

---

## 11. Step 9 - Test External Connectivity

After internal connectivity has been verified, test the external network.

The project's External Cloud is:

**172.16.0.1**

The expected path is:

**Client → VLAN Gateway → Main Router → ISP Router → External Network**

If the client can reach its gateway but cannot reach the External Cloud, investigate:

- Routing
- Default route
- ISP connectivity
- ACL policies
- External network configuration

---

## 12. Layer-by-Layer Troubleshooting

The troubleshooting process can be mapped to the network layers.

| Layer | What to Check | Example Command / Test |
|------|---------------|-------------------------|
| Physical | Cables and interfaces | `show interfaces status` |
| Layer 2 | VLAN membership | `show vlan brief` |
| Layer 2 | Trunking | `show interfaces trunk` |
| Layer 3 | IP configuration | Client IP configuration |
| Layer 3 | Gateway | `ping <gateway>` |
| Layer 3 | Routing | `show ip route` |
| Security | ACLs | `show ip access-lists` |
| External | End-to-end connectivity | `ping 172.16.0.1` |

This provides a repeatable troubleshooting process.

---

## 13. Common Problem - Wrong IP Address

### Symptom

A device receives an IP address from an unexpected network.

Example:

A School device receives:

**192.172.20.x**

instead of:

**192.172.10.x**

### Possible Causes

- Incorrect access VLAN
- Incorrect trunk configuration
- Incorrect DHCP configuration
- Incorrect router subinterface
- Incorrect Layer 2 path

### Troubleshooting

Check:

**show vlan brief**

Then:

**show interfaces trunk**

Then verify the router's VLAN subinterfaces and DHCP pools.

---

## 14. Common Problem - No DHCP Address

### Symptom

A client fails to obtain an IPv4 address.

### Possible Causes

- Access port assigned to the wrong VLAN
- VLAN missing
- VLAN not permitted on the trunk
- Router subinterface down
- DHCP pool missing
- DHCP pool exhausted
- Client not configured for DHCP

### Troubleshooting

Check in this order:

1. Physical connection
2. Access VLAN
3. Trunk
4. Router interface
5. DHCP pool
6. DHCP bindings
7. Client configuration

---

## 15. Common Problem - Gateway Unreachable

### Symptom

The client has a valid IP address but cannot ping its default gateway.

### Possible Causes

- Incorrect VLAN
- Incorrect subnet mask
- Incorrect gateway address
- Trunk problem
- Router subinterface problem
- Interface down

### Troubleshooting

Verify the client's IP configuration first.

Then check:

**show vlan brief**

**show interfaces trunk**

**show ip interface brief**

The gateway should be reachable before testing other networks.

---

## 16. Common Problem - Inter-VLAN Communication Fails

### Symptom

A client can reach its gateway but cannot reach another VLAN.

### Possible Causes

- ACL blocking the traffic
- Incorrect routing
- Incorrect destination address
- VLAN or Layer 3 configuration issue

### Troubleshooting

First verify:

**show ip route**

Then inspect:

**show ip access-lists**

If the traffic is intentionally restricted by the security policy, the failed ping may be the expected result.

---

## 17. Common Problem - External Connectivity Fails

### Symptom

A client can reach its local gateway but cannot reach the External Cloud.

### Possible Causes

- Missing default route
- Incorrect next-hop address
- ISP routing problem
- ACL blocking external traffic
- External network configuration problem

### Troubleshooting

Check:

**show ip route**

Verify the external path and default route.

Then inspect:

**show ip access-lists**

Finally test the connection again.

---

## 18. Common Problem - Access Switch Client Gets Wrong Network

### Symptom

A client connected directly to the Core Switch receives the expected VLAN address, but a client connected through an Access Switch receives an address from another network.

### Investigation

This indicates that the problem may exist on the path between the Core Switch and Access Switch.

Check:

1. Client access-port VLAN
2. Access Switch VLAN database
3. Access Switch uplink
4. Uplink trunk configuration
5. Allowed VLANs
6. Core Switch trunk configuration

Use:

**show vlan brief**

and:

**show interfaces trunk**

on the relevant switches.

The VLAN must remain consistent across the Layer 2 path.

---

## 19. Common Problem - ACL Blocks Required Traffic

### Symptom

A connection that should be permitted is unsuccessful.

### Investigation

Check:

**show ip access-lists**

Look for the ACL entry that matches the traffic.

ACL counters can help identify whether a particular permit or deny statement is being used.

Also verify:

- Source IP
- Destination IP
- Protocol
- Port
- ACL direction
- Interface where the ACL is applied

---

## 20. Troubleshooting Decision Process

The following decision process can be used during network faults.

### Client has no IP address

Check:

**Physical → VLAN → Trunk → Router Interface → DHCP**

### Client has an IP but cannot reach gateway

Check:

**IP Configuration → VLAN → Trunk → Router Interface**

### Client reaches gateway but not another VLAN

Check:

**Routing → ACL → Destination**

### Client reaches internal networks but not external network

Check:

**Default Route → ISP → ACL → External Network**

This provides a simple fault-isolation model.

---

## 21. Verification Commands

The following commands are useful during troubleshooting.

### Switch

**show vlan brief**

**show interfaces status**

**show interfaces trunk**

**show running-config**

### Router

**show ip interface brief**

**show ip route**

**show ip dhcp pool**

**show ip dhcp binding**

**show ip access-lists**

**show running-config**

### End Device

Use the device's network configuration tools to verify:

- IPv4 address
- Subnet mask
- Default gateway
- DNS server

Connectivity can then be tested with:

**ping**

---

## 22. Troubleshooting Best Practices

The project follows these troubleshooting practices:

### Change One Thing at a Time

Avoid changing several configurations simultaneously because this makes it difficult to identify the cause of a problem.

### Verify Before Changing

Use show commands and connectivity tests before modifying configuration.

### Start Local

Always verify local connectivity before testing remote connectivity.

### Follow the Network Path

Trace the expected path from:

**End Device → Access Switch → Core Switch → Main Router → ISP → External Network**

### Use Evidence

Use command output, IP addresses, ping results, and ACL counters as evidence.

### Re-test After Changes

After making a configuration change, repeat the original test to confirm whether the issue was resolved.

---

## 23. Troubleshooting Documentation

When a fault is identified, record:

- Problem description
- Affected device
- Affected VLAN
- Observed behaviour
- Tests performed
- Commands used
- Root cause
- Configuration change
- Verification result

This creates a useful technical record and makes future troubleshooting easier.

---

## 24. Troubleshooting Outcome

The troubleshooting methodology provides a structured way to diagnose problems within the Rural Digital Access Network.

The process avoids random configuration changes and instead isolates faults by network layer.

The project demonstrates practical troubleshooting of:

- VLANs
- Trunks
- DHCP
- IP addressing
- Default gateways
- Routing
- ACLs
- External connectivity

The troubleshooting process also supports the testing and validation of the network documented in the following sections.
