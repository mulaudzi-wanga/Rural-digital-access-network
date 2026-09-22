# Rural Digital Access Network

## Project Overview

The Rural Digital Access Network is a Cisco Packet Tracer network engineering project designed to provide structured, segmented and controlled network connectivity for a rural environment.

The network was designed to support different service areas while maintaining separation between their networks. The project focuses on practical network engineering principles including IP addressing, VLAN segmentation, switching, trunking, routing, DHCP, access control and network troubleshooting.

The project was developed as a practical demonstration of designing, configuring, securing, testing and troubleshooting a multi-VLAN network.

---

## Project Objectives

The main objectives of the project were to:

- Design a structured network topology for a rural environment.
- Separate different service areas using VLANs.
- Develop a structured IP addressing scheme.
- Configure access ports and trunk links.
- Implement inter-VLAN routing using Router-on-a-Stick.
- Configure DHCP for client address allocation.
- Configure routing between the internal network and the ISP network.
- Provide connectivity to an external simulated cloud network.
- Restrict unauthorized communication between internal VLANs.
- Implement basic switch security.
- Disable unused switch ports.
- Troubleshoot network connectivity and configuration problems.
- Verify the operation of the network using Cisco IOS commands and connectivity tests.

---

## Network Environment

The network represents a rural digital access environment containing multiple service areas.

The main service areas are:

- School
- Clinic
- Business
- Community
- Infrastructure/Management

Each major service area is separated into its own VLAN and IP network.

This design reduces the size of broadcast domains and provides greater control over communication between different parts of the network.

---

## Network Topology

The network consists of:

- Main router
- Core switch
- Access switches
- ISP router
- End devices
- Simulated external cloud network

The main router provides routing between the internal VLANs and connectivity toward the ISP.

The core switch provides the central switching point between the main router and the access networks.

Access switches provide connectivity for end devices belonging to their respective network areas.

The ISP router represents the connection between the internal network and the external network.

The simulated cloud represents an external network used to test connectivity beyond the internal network.

---

## VLAN Structure

The network uses VLANs to separate different logical network segments.

| VLAN | Network Area | Network Address | Default Gateway |
|------|--------------|-----------------|-----------------|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure/Management | 192.172.99.0/24 | 192.172.99.1 |

VLAN 99 is included as part of the network design and is documented as an infrastructure/management VLAN.

The exact devices and interfaces associated with VLAN 99 are documented in the device configuration files and verification evidence.

---

## IP Addressing

The internal network uses a structured IPv4 addressing scheme based on the VLAN numbers.

The internal networks are:

```text
VLAN 10
192.172.10.0/24
Gateway: 192.172.10.1

VLAN 20
192.172.20.0/24
Gateway: 192.172.20.1

VLAN 30
192.172.30.0/24
Gateway: 192.172.30.1

VLAN 40
192.172.40.0/24
Gateway: 192.172.40.1

VLAN 50
192.172.50.0/24
Gateway: 192.172.50.1

VLAN 99
192.172.99.0/24
Gateway: 192.172.99.1
