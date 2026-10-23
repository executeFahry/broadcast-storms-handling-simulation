# Network Design Decisions

## 1. Architecture
This network uses a **collapsed core** model to simplify the topology and reduce costs for a small-to-medium-sized network:
- **Core/Distribution Layer**: Two multilayer switches (Cisco 3650) provide inter-VLAN routing and network redundancy.
- **Access Layer**: Three Layer 2 switches (Cisco 2960) distribute connections to the teachers' room, student lab, and staff room.

## 2. VLAN & IP Subnet Allocation
IP addresses are organized into `/24` subnets:
- **VLAN 10 (Teacher)**: 192.168.10.0/24 (Gateway: .1)
- **VLAN 20 (StudentLab)**: 192.168.20.0/24 (Gateway: .1)
- **VLAN 30 (Staff)**: 192.168.30.0/24 (Gateway: .1)
- **VLAN 99 (Native & Management)**: 192.168.99.0/24 (Gateway: .1)
- **VLAN 100 (DHCP Server)**: 192.168.100.0/24 (Gateway: .1)

## 3. Redundancy & Load Balancing Strategy
Each *access switch* has dual uplinks to `SW-CORE-1` and `SW-CORE-2`. **STP load balancing** is implemented with PVST+ to make use of the available link capacity:
- Traffic from VLAN 10 (Teacher) and VLAN 20 (StudentLab) uses **SW-CORE-1** as its primary path.
- Traffic from VLAN 30 (Staff) and VLAN 99 uses **SW-CORE-2** as its primary path.

## 4. Security & Optimization
- **Strict VLAN Pruning**: Inter-switch *trunk* links allow only VLANs that are in use. The default VLAN 1 is removed from the *trunk* allowed list.
- **PortFast & BPDU Guard**: Enabled on all ports connected to end devices (PCs/laptops).
