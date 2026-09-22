# Rural Digital Access Network

A Cisco Packet Tracer network engineering project designed to provide structured, segmented, secure, and controlled connectivity for a rural environment.

The project demonstrates practical network engineering skills including VLAN segmentation, IP addressing, switching, trunking, Router-on-a-Stick, DHCP, routing, ACLs, network security, troubleshooting, and connectivity testing.

---

## Project Overview

The Rural Digital Access Network represents a simulated rural environment containing multiple service areas that require reliable network connectivity while maintaining logical separation between different types of users and services.

The network was designed and implemented in Cisco Packet Tracer to demonstrate the complete network engineering process:

**Plan → Design → Configure → Secure → Test → Troubleshoot → Document**

---

## Project Objectives

The main objectives were to:

- Design a structured network topology for a rural environment.
- Create separate VLANs for different service areas.
- Develop a structured IPv4 addressing scheme.
- Configure access ports and trunk links.
- Implement inter-VLAN routing using Router-on-a-Stick.
- Configure DHCP for automatic client addressing.
- Configure routing between the internal network and ISP network.
- Provide connectivity to a simulated external network.
- Control communication between internal VLANs using ACLs.
- Separate infrastructure and management traffic.
- Disable unused switch ports where applicable.
- Troubleshoot network configuration and connectivity problems.
- Verify network operation using Cisco IOS commands and connectivity tests.

---

## Network Architecture

The network follows a hierarchical structure:

    End Devices
         |
    Access Switches
         |
    Core Switch
         |
    Main Router
         |
    ISP Router
         |
    External Network

### Main Components

- Main Router
- Core Switch
- Access Switches
- ISP Router
- End Devices
- Simulated External Network

The Core Switch provides the central Layer 2 switching point.

The Main Router provides:

- Inter-VLAN routing
- DHCP services
- Routing toward the external network
- ACL-based traffic control

The ISP Router represents the external network connection.

---

## VLAN Structure

The network uses VLANs to separate different logical network segments.

| VLAN | Network Area | Network Address | Default Gateway |
|---|---|---|---|
| 10 | School | 192.172.10.0/24 | 192.172.10.1 |
| 20 | Clinic | 192.172.20.0/24 | 192.172.20.1 |
| 30 | Business | 192.172.30.0/24 | 192.172.30.1 |
| 40 | Community | 192.172.40.0/24 | 192.172.40.1 |
| 50 | Additional Network | 192.172.50.0/24 | 192.172.50.1 |
| 99 | Infrastructure/Management | 192.172.99.0/24 | 192.172.99.1 |

VLAN 99 is used for infrastructure and management purposes.

---

## IP Addressing

The project uses a structured IPv4 addressing scheme based on the VLAN numbers.

| VLAN | Network | Gateway |
|---|---|---|
| 10 | 192.172.10.0/24 | 192.172.10.1 |
| 20 | 192.172.20.0/24 | 192.172.20.1 |
| 30 | 192.172.30.0/24 | 192.172.30.1 |
| 40 | 192.172.40.0/24 | 192.172.40.1 |
| 50 | 192.172.50.0/24 | 192.172.50.1 |
| 99 | 192.172.99.0/24 | 192.172.99.1 |

The ISP transit network uses:

- ISP Router: `10.0.0.1`
- Main Router: `10.0.0.2`

---

## Key Technologies

### Switching

- VLANs
- Access ports
- 802.1Q trunking
- Core and access switch architecture
- Unused-port shutdown

### Routing

- Router-on-a-Stick
- Inter-VLAN routing
- Static routing
- Default routing
- External network connectivity

### Network Services

- DHCP
- IPv4 addressing
- Default gateways

### Security

- VLAN-based segmentation
- Extended ACLs
- Infrastructure/management VLAN
- Controlled inter-VLAN communication
- Disabled unused ports

### Troubleshooting

The project involved troubleshooting issues related to:

- Incorrect VLAN assignments
- Access ports
- Trunk links
- Allowed VLANs
- Router subinterfaces
- DHCP
- Default gateways
- Routing
- ACLs
- End-to-end connectivity

---

## Network Security

Security was incorporated into the network design through segmentation and traffic control.

The project uses:

- VLAN segmentation
- Extended ACLs
- Infrastructure/management separation
- Controlled inter-VLAN communication
- Disabled unused switch ports

The purpose is to reduce unnecessary communication between different service areas while maintaining required network connectivity.

---

## Testing and Validation

The network was tested using Cisco IOS verification commands and end-device connectivity tests.

Examples of commands used include:

    show vlan brief
    show interfaces trunk
    show ip interface brief
    show ip route
    show running-config
    show interfaces status

Connectivity testing was also performed using tools such as:

    ping

Testing was used to verify VLAN assignment, trunk operation, routing, DHCP addressing, gateway reachability, ACL behaviour, and external connectivity.

---

## Project Evidence

The repository contains configuration screenshots showing the implementation of the network.

### Router Configuration

Router configuration evidence is available in:

[Router Configurations](./Rural-digital-access-network-screenshot/Router-Configs/)

### Switch Configuration

Switch configuration evidence is available in:

[Switch Configurations](./Rural-digital-access-network-screenshot/Switch-Configs/)

### Network Layout

The repository also contains screenshot showing the overall network and device layout.

[Network Devices Layout](./Rural-digital-access-network-screenshot/Network_devices_layout.png)

---

## Packet Tracer Project

The complete Cisco Packet Tracer project file is included in the repository.

[Download / Open the Packet Tracer Project](./MQHOBOKAZI%20COMMUNITY%20SETUP.pkt)

The file can be opened using Cisco Packet Tracer to inspect the topology, device configurations, VLANs, routing, DHCP, ACLs, and connectivity.

---

## Documentation

Detailed project documentation is available in the `documentation` directory.

It covers:

- Project overview
- Network design
- IP addressing
- VLAN configuration
- Routing
- Network topology
- DHCP
- Network security
- Troubleshooting
- Testing and validation
- Implementation and configuration
- Project conclusion and future improvements

[View Project Documentation](./documentation/)

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Cisco Packet Tracer
- Cisco IOS
- VLAN configuration
- Switching
- Trunking
- Router-on-a-Stick
- IPv4 addressing
- DHCP
- Inter-VLAN routing
- Static routing
- Access Control Lists
- Network segmentation
- Basic switch security
- Network troubleshooting
- Network testing
- Technical documentation

---

## Project Outcome

The completed project demonstrates how a multi-VLAN network can be designed, configured, secured, tested, and documented for a simulated rural environment.

The project focuses on practical network engineering rather than only theoretical configuration and provides evidence of hands-on work with Cisco networking concepts.

---

## Future Improvements

Possible future improvements include:

- Enterprise firewall integration
- Network redundancy
- Advanced routing protocols
- Network monitoring
- Centralised logging
- Advanced Layer 2 security
- VPN connectivity
- Network automation using Python
- Cloud networking integration
- IPv6 implementation

---

## Project Status

**Status: Completed**

The Cisco Packet Tracer network, configuration evidence, testing documentation, and project documentation have been completed and uploaded to this repository.
