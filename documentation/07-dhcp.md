# 07 - DHCP Configuration

## 1. Purpose

Dynamic Host Configuration Protocol (DHCP) is used to automatically provide IPv4 configuration to end devices within the Rural Digital Access Network.

DHCP reduces the need to manually configure every client and ensures that devices receive addressing information from the correct network.

The DHCP implementation provides:

- IPv4 addresses
- Subnet masks
- Default gateways
- DNS information where configured
- Automatic address allocation
- Centralised address management

---

## 2. DHCP Architecture

DHCP is provided by the Main Router.

The Main Router maintains DHCP pools for the required internal VLANs.

The general process is:

**End Device → DHCP Request → Main Router → DHCP Pool → IP Configuration**

The assigned address must correspond to the VLAN in which the device is connected.

---

## 3. DHCP Networks

The DHCP addressing structure follows the VLAN addressing plan.

| VLAN | Service Area | DHCP Network | Default Gateway |
|------|--------------|--------------|-----------------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure / Management | 192.172.99.0/24 | 192.172.99.1 |

---

## 4. Address Reservation

The first addresses in each network are reserved from DHCP allocation.

This prevents important infrastructure addresses from being automatically assigned to end devices.

The project reserves the first ten addresses of the relevant networks.

For example, the School network reserves:

**192.172.10.1 - 192.172.10.10**

The DHCP pool then begins allocating addresses after the reserved range.

The same addressing principle is applied to the other DHCP networks.

---

## 5. Default Gateway Allocation

The default gateway is the first usable address in each VLAN.

| VLAN | Default Gateway |
|------|-----------------|
| 10 | 192.172.10.1 |
| 20 | 192.172.20.1 |
| 30 | 192.172.30.1 |
| 40 | 192.172.40.1 |
| 50 | 192.172.50.1 |
| 99 | 192.172.99.1 |

The DHCP configuration must provide the correct gateway for the corresponding VLAN.

For example:

A device in VLAN 10 must receive:

**Default Gateway: 192.172.10.1**

A device in VLAN 20 must receive:

**Default Gateway: 192.172.20.1**

---

## 6. DHCP and VLAN Relationship

DHCP depends on the VLAN configuration being correct.

The expected relationship is:

**VLAN 10 → 192.172.10.0/24 DHCP network**

**VLAN 20 → 192.172.20.0/24 DHCP network**

**VLAN 30 → 192.172.30.0/24 DHCP network**

**VLAN 40 → 192.172.40.0/24 DHCP network**

**VLAN 50 → 192.172.50.0/24 DHCP network**

A device receiving an address from the wrong subnet is an indication that the VLAN, DHCP configuration, or Layer 2 path should be investigated.

---

## 7. DHCP Allocation Process

When an end device requires an IP address, DHCP follows a basic exchange.

### Step 1 - Discover

The client broadcasts a DHCP discovery message to locate a DHCP server.

### Step 2 - Offer

The DHCP server offers an available address from the appropriate DHCP pool.

### Step 3 - Request

The client requests the offered address.

### Step 4 - Acknowledge

The DHCP server confirms the allocation.

The client can then use the assigned network configuration.

---

## 8. DHCP Information

A DHCP pool can provide several pieces of information to a client.

These may include:

- IP address
- Subnet mask
- Default gateway
- DNS server information

The exact values depend on the configured DHCP pool.

The most important requirement is that the client receives addressing information belonging to its VLAN.

---

## 9. DHCP Verification

The Main Router provides several commands for checking DHCP operation.

### View DHCP Pools

Use:

**show ip dhcp pool**

This displays information about configured DHCP pools and their allocation status.

### View DHCP Bindings

Use:

**show ip dhcp binding**

This displays addresses that have been dynamically assigned to clients.

### Check Router Interfaces

Use:

**show ip interface brief**

This confirms that the VLAN gateway interfaces are operational.

### Check Routing

Use:

**show ip route**

This confirms that the VLAN networks are present in the routing table.

---

## 10. End-Device Verification

An end device should be checked after connecting it to the network.

The device should receive:

- An IPv4 address from the correct subnet
- A `/24` subnet mask
- The correct VLAN gateway
- DNS information where configured

For example, a School client should receive an address from:

**192.172.10.0/24**

and use:

**192.172.10.1**

as its default gateway.

---

## 11. DHCP Testing

DHCP testing should be performed from devices connected to different VLANs.

For each VLAN:

1. Connect the end device to the correct access port.
2. Confirm the port belongs to the expected VLAN.
3. Set the device to obtain its address automatically.
4. Check the assigned IPv4 address.
5. Check the subnet mask.
6. Check the default gateway.
7. Test connectivity to the gateway.
8. Test permitted external connectivity.

This confirms that DHCP is operating together with the VLAN and routing configuration.

---

## 12. DHCP Troubleshooting

If a client does not receive an IP address, troubleshoot the problem from Layer 2 upward.

### Step 1 - Check the Physical Connection

Confirm that the end device is connected and the switch interface is operational.

### Step 2 - Check the Access Port

Confirm that the switch port is assigned to the correct VLAN.

### Step 3 - Check the VLAN

Use:

**show vlan brief**

Confirm that the required VLAN exists and that the client port is assigned correctly.

### Step 4 - Check the Uplink

If the client connects through another switch, verify that the required VLAN is permitted across the uplink.

Use:

**show interfaces trunk**

### Step 5 - Check the Router Gateway

Use:

**show ip interface brief**

Confirm that the corresponding router subinterface is operational.

### Step 6 - Check the DHCP Pool

Use:

**show ip dhcp pool**

Confirm that the correct DHCP pool exists.

### Step 7 - Check DHCP Bindings

Use:

**show ip dhcp binding**

Determine whether the router has assigned an address to the client.

### Step 8 - Check Address Availability

Confirm that the DHCP pool still has usable addresses available.

### Step 9 - Check the Client

Confirm that the end device is configured to obtain its IP address automatically.

---

## 13. Common DHCP Problems

### Wrong VLAN

A client connected to the wrong VLAN may receive an address from an unexpected subnet.

### Incorrect DHCP Network

A DHCP pool that uses the wrong network will provide incorrect addressing.

### Incorrect Default Gateway

A client may receive an IP address but still fail to communicate outside its subnet if the gateway is incorrect.

### DHCP Pool Exhaustion

If all available addresses have been allocated, new clients cannot obtain an address.

### VLAN Not Reaching the Router

If the required VLAN is not correctly transported through the switching infrastructure, DHCP requests may not reach the appropriate router interface.

### Interface Down

If the VLAN gateway interface is not operational, clients cannot use that gateway.

---

## 14. DHCP and Network Security

DHCP provides addressing but does not determine whether a client can communicate with other networks.

Traffic control is handled separately through routing and ACL policies.

Therefore:

**DHCP = Address Configuration**

**Routing = Path Selection**

**ACL = Traffic Control**

These functions work together but perform different roles.

---

## 15. DHCP and Troubleshooting

DHCP is also useful as a diagnostic indicator.

If a device receives:

**192.172.10.x**

it is likely receiving configuration from the School network.

If a School device unexpectedly receives:

**192.172.20.x**

the VLAN or Layer 2 path should be investigated.

If a device receives a valid address but cannot reach its gateway, the switching path and router interface should be checked.

If the gateway works but external connectivity fails, routing and ACL configuration should be investigated.

---

## 16. DHCP Design Benefits

The DHCP design provides several benefits.

### Automation

Clients do not need to be manually configured.

### Consistency

Devices receive addressing information from the appropriate network.

### Centralised Management

Address allocation can be monitored from the Main Router.

### Reduced Configuration Errors

Automatic configuration reduces the chance of manually entering an incorrect subnet mask or gateway.

### Scalability

Additional clients can be connected without manually assigning an address to every device.

---

## 17. DHCP Validation Checklist

The following checklist can be used to validate DHCP:

- DHCP pools exist for the required networks.
- Reserved addresses are excluded from automatic allocation.
- The correct network is configured for each pool.
- The correct subnet mask is provided.
- The correct default gateway is provided.
- End devices receive addresses from the expected VLAN subnet.
- DHCP bindings appear on the router.
- VLANs are correctly assigned.
- Trunk links carry required VLAN traffic.
- Router subinterfaces are operational.
- Gateway connectivity works.
- Permitted external connectivity works.

---

## 18. DHCP Summary

The DHCP implementation provides automatic IPv4 configuration for the network's service areas.

The design follows the VLAN structure:

| VLAN | Network | Gateway |
|------|---------|---------|
| 10 | 192.172.10.0/24 | 192.172.10.1 |
| 20 | 192.172.20.0/24 | 192.172.20.1 |
| 30 | 192.172.30.0/24 | 192.172.30.1 |
| 40 | 192.172.40.0/24 | 192.172.40.1 |
| 50 | 192.172.50.0/24 | 192.172.50.1 |
| 99 | 192.172.99.0/24 | 192.172.99.1 |

The DHCP service works together with VLAN segmentation, routing, and ACL security to provide structured and controlled network connectivity.
