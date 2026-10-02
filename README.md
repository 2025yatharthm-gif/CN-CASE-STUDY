Vellum Antiquarian Bookstore & Rare Books Chain

Computer Networking & Cyber Security – Case Study 117

A Cisco Packet Tracer networking project designed for Vellum Antiquarian Bookstore & Rare Books Chain, a three-site bookstore network requiring VLAN segmentation, trunking, redundancy, and network security.

Student Information

* Name: Yatharth Dharmesh Mishra
* Roll No.: 150096725055
* Program: B.Tech CSE 2025–29
* University: ITM Skills University
* Faculty: Dr. Dipanjan Biswas
* Case Study: 117

⸻

Project Overview

Vellum Antiquarian Bookstore operates three locations:

* Main Street
* Harborfront
* Auction Annex

The three locations are connected in a triangular topology to provide network redundancy and support shared business systems.

The original problem was a flat network where all devices operated on the same network. This could lead to excessive broadcast traffic and allow guest devices to potentially access internal systems.

The proposed solution uses VLANs, 802.1Q trunking, Spanning Tree Protocol, EtherChannel, router-on-a-stick routing, ACLs, and port security.

⸻

Network Topology

                    R0
                     |
                    SW0
                  /     \
                 /       \
               SW1-------SW2
                |           |
               R1           R2

Device Mapping

Location	Router	Switch
Main Street	R0	SW0
Harborfront	R1	SW1
Auction Annex	R2	SW2

The network contains 12 PCs, connected to their respective local switches.

⸻

VLAN Design

VLAN	Name	Purpose	Locations
13	Provenance-Systems	Cataloguing and authentication systems	Main + Auction
23	Staff-POS	POS terminals and staff devices	All sites
33	Admin	Finance and administration	Harbor
43	IT-Mgmt	Network management	All sites
915	Guest	Public Wi-Fi	Harbor
999	Native	Unused native VLAN	All trunk links

VLAN 999 is intentionally unused for normal user traffic and is configured as the native VLAN on trunk connections.

⸻

PC Port Assignment

SW0 – Main Street

Fa0/4 - Fa0/5 → VLAN 13
Fa0/6 - Fa0/7 → VLAN 23

SW1 – Harborfront

Fa0/4 - Fa0/5 → VLAN 23
Fa0/6 - Fa0/7 → VLAN 915

SW2 – Auction Annex

Fa0/4 - Fa0/5 → VLAN 13
Fa0/6 - Fa0/7 → VLAN 23

⸻

Trunk Connections

The three switches are connected using 802.1Q trunk links.

SW0 Fa0/1 ↔ SW1 Fa0/1
SW1 Fa0/2 ↔ SW2 Fa0/1
SW0 Fa0/2 ↔ SW2 Fa0/2
SW0 Fa0/3 ↔ SW2 Fa0/3

Allowed VLANs:

13, 23, 33, 43, 915, 999

Native VLAN:

999

Using an unused native VLAN helps reduce the risk associated with native-VLAN traffic and provides an additional Layer-2 security measure.

⸻

EtherChannel

The two parallel connections between SW0 and SW2 are intended to form an LACP EtherChannel.

SW0 Fa0/2 ───── SW2 Fa0/2
SW0 Fa0/3 ───── SW2 Fa0/3
        ↓
   Port-Channel 1

EtherChannel combines multiple physical links into one logical connection. From STP’s perspective, the bundled links appear as a single logical path.

Implementation Status: EtherChannel configuration was attempted using LACP but was not successfully verified as operational during the practical implementation.

⸻

Spanning Tree Protocol

The triangular switch topology creates a potential Layer-2 loop.

SW0 is planned as the root bridge.

Suggested priorities:

SW0 → 4096
SW1 → 8192
SW2 → 12288

STP is responsible for preventing switching loops while allowing redundant links to remain available as backup paths.

⸻

Inter-VLAN Routing

Router-on-a-stick is proposed for communication between VLANs.

Example gateway addressing:

VLAN 13  → 192.168.13.1/24
VLAN 23  → 192.168.23.1/24
VLAN 33  → 192.168.33.1/24
VLAN 43  → 192.168.43.1/24
VLAN 915 → 192.168.91.1/24

Inter-VLAN communication is controlled using routing and access-control policies.

⸻

Guest Network Security

The Harborfront Guest network uses:

VLAN 915

Guest devices must not be able to access:

VLAN 13 – Provenance-Systems

An ACL is proposed to block Guest-to-Provenance traffic while allowing other permitted communication.

⸻

Port Security

Port security is proposed for Harborfront Guest access ports.

Configuration:

Maximum MAC addresses: 2
Violation mode: restrict

The restrict mode allows violating traffic to be dropped without immediately shutting down the entire switch port, which is useful in a public environment where devices may frequently connect and disconnect.

⸻

VTP

VTP Transparent mode is recommended for this network.

The three-switch environment is small enough for VLANs to be managed locally. Transparent mode also avoids relying on a central VTP server to distribute VLAN database changes across the network.

⸻

Link Failure Scenario

The network contains redundant paths between the three switches.

For example, if the connection between:

SW0 ↔ SW1

fails, STP can reconverge and use the alternate path:

SW1 → SW2 → SW0

This demonstrates the purpose of the triangular topology and redundant switching paths.

⸻

Verification Commands

Useful Cisco IOS commands for verification include:

show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
show ip interface brief
show running-config
show port-security
show port-security interface

Connectivity can be tested using:

ping
traceroute

⸻

Implementation Status

Completed

* Three-site topology
* Router and switch connections
* Switch triangle
* Twelve PCs
* VLAN creation
* PC access-port assignment
* Inter-switch trunk configuration
* Native VLAN 999 configuration

Partially Implemented / Planned

* EtherChannel verification
* STP root bridge configuration and verification
* Router-on-a-stick inter-VLAN routing
* Guest-to-Provenance ACL
* Guest port security
* VTP configuration
* End-to-end connectivity testing
* STP link-failure simulation

⸻

Files

Vellum-Antiquarian-Bookstore/
│
├── Vellum_Antiquarian_Bookstore.pkt
├── Documentation/
│   └── Case_Study_117_Report.pdf
└── README.md

⸻

Technologies Used

* Cisco Packet Tracer
* VLAN
* IEEE 802.1Q Trunking
* Spanning Tree Protocol (STP)
* LACP EtherChannel
* Router-on-a-Stick
* Access Control Lists (ACL)
* Port Security
* VTP
* IPv4 Networking

⸻

Conclusion

This project demonstrates the design of a segmented three-site enterprise LAN for Vellum Antiquarian Bookstore. The design separates different categories of traffic using VLANs, provides redundant switch connectivity, and introduces security controls for the public Guest network.

The Packet Tracer file contains the implemented network topology and initial switching configuration, while the documentation describes the complete proposed network architecture and remaining configuration requirements.
