# clab-aoscx-labs
[Aruba AOS-CX Switch Simulator labs](https://community.arubanetworks.com/discussion/using-the-aos-cx-switch-simulator-lab-guides) for containerlab.

>[!NOTE]
>This is work in progress. Many labs are still missing and so far only a few lab guides for using the labs with containerlab are available (use original Aruba lab guides instead).

## Available Labs

### Layer 2
| Lab | Source | Clab ready | Doc ready |
|-----|--------|------------|-----------|
| [L2 Access Management and Connectivity](./01_layer_2/01_l2_access_mgmt/README.md) | [Source](https://community.arubanetworks.com/community-home/digestviewer/viewthread?GroupId=565&MessageKey=4a6a8fde-947c-4910-b36f-7172d0f096ea&CommunityKey=aa40c287-728e-4827-b062-5eff4ed6410b&tab=digestviewer&ReturnUrl=%2fcommunity-home%2fdigestviewer%3fcommunitykey%3daa40c287-728e-4827-b062-5eff4ed6410b%26tab%3ddigestviewer) | :white_check_mark: |  :white_check_mark: |
| [Spanning Tree Basics](./01_layer_2/02_spanning_tree_basics/README.md) | [Source](https://community.arubanetworks.com/community-home/digestviewer/viewthread?GroupId=565&MessageKey=348d033c-c04c-40f6-9709-35324390ef25&CommunityKey=aa40c287-728e-4827-b062-5eff4ed6410b&tab=digestviewer&ReturnUrl=%2fcommunity-home%2fdigestviewer%3fcommunitykey%3daa40c287-728e-4827-b062-5eff4ed6410b%26tab%3ddigestviewer) |:white_check_mark: |  :white_check_mark: |
| [Deploying MSTP](./01_layer_2/03_deploying_mstp/README.md) | [Source](https://community.arubanetworks.com/community-home/digestviewer/viewthread?GroupId=565&MessageKey=93cd4620-5597-4f35-81e2-e6a32b6b4c98&CommunityKey=aa40c287-728e-4827-b062-5eff4ed6410b&tab=digestviewer&ReturnUrl=%2fcommunity-home%2fdigestviewer%3fcommunitykey%3daa40c287-728e-4827-b062-5eff4ed6410b%26tab%3ddigestviewer) | :white_check_mark: |  :white_check_mark: |
| [Loop Protect](./01_layer_2/04_loop_protect/README.md) | [Source](https://community.arubanetworks.com/community-home/digestviewer/viewthread?GroupId=565&MessageKey=8bdba8f9-180d-46b9-8551-ac9e1c17ada6&CommunityKey=aa40c287-728e-4827-b062-5eff4ed6410b&tab=digestviewer&ReturnUrl=%2fcommunity-home%2fdigestviewer%3fcommunitykey%3daa40c287-728e-4827-b062-5eff4ed6410b%26tab%3ddigestviewer) | :white_check_mark: |  :white_check_mark: |
| [RPVST+](./01_layer_2/05_rpvst+/README.md) | [Source](https://community.arubanetworks.com/community-home/digestviewer/viewthread?GroupId=565&MessageKey=bca5bc66-ab2d-46e9-b2f5-675af866d83b&CommunityKey=aa40c287-728e-4827-b062-5eff4ed6410b&tab=digestviewer&ReturnUrl=%2fcommunity-home%2fdigestviewer%3fcommunitykey%3daa40c287-728e-4827-b062-5eff4ed6410b%26tab%3ddigestviewer) | :white_check_mark: | :white_check_mark: |
| [MVRP](./01_layer_2/06_mvrp/) | [Source]() | :white_check_mark: | :x: |

### High Availability
- AOS-CX Simulator Lab - VSX - Virtual Switching Extension (Part 1)
- AOS-CX Simulator Lab - VSX (Part 2)

### Layer 3 Routing
- AOS-CX Simulator Lab - Deploying OSPFv2
- AOS-CX Simulator Lab - Basic BGP
- AOS-CX Simulator Lab - VRF Part 1

### Campus Series
- AOS-CX Simulator Lab - Campus 3-Tier with RIPv2
- AOS-CX Simulator Lab - Campus 2-Tier IPv4 with OSPF Routed Access
- AOS-CX Simulator Lab - Campus 2-Tier IPv4 L2 Access with VSX
- AOS-CX Simulator Lab - Campus 3-Tier IPv4 L2 Access with VSX and OSPF
- AOS-CX Simulator Lab - Campus 2-Tier IPv4 Routed Access iBGP
- AOS-CX Simulator Lab - Campus 2-Tier with IPv4 RIPv2 Routed Access
- AOS-CX Simulator Lab - Campus 2-Tier IPv6 Routed Access with RIPng and DHCPv6
- AOS-CX Simulator Lab - Campus 2-Tier IPv6 Routed Access with OSPFv3 and SLAAC
- AOS-CX Simulator Lab - Campus 2-Tier IPv6 L2 Access with VSX and OSPFv3
- AOS-CX Simulator Lab - Campus 3-Tier IPv6 L3 Access with OSPFv3 MultiArea and SLAAC
- AOS-CX Simulator Lab - Campus 3-Tier Routed Access with iBGP
- AOS-CX Simulator Lab - Campus 2 Tier L3 Access with OSPF and Link Security

## Missing Labs

### Layer 3 Routing
- AOS-CX Simulator Lab - Deploying OSPFv2
- AOS-CX Simulator Lab - Basic BGP
- AOS-CX Simulator Lab - VRF Part 1
- AOS-CX Simulator Lab - VRF - Dynamic IVRL
- AOS-CX Simulator Lab - OSPFv3 Fundamentals
- AOS-CX Simulator Lab - OSPF-BGP Route Redistribution
- AOS-CX Simulator Lab - OSPFv2 Areas - Basics
- AOS-CX Simulator Lab - OSPF v2 Features
- AOS-CX Simulator Lab - OSPF Troubleshooting
- AOS-CX Simulator Lab - OSPFv3 Troubleshooting
- AOS-CX Simulator Lab - IPv6 Basics
- AOS-CX Simulator Lab - RIPng
- AOS-CX Simulator Lab - OSPFv3 Areas
- AOS-CX Simulator Lab - BGP IPv6
- AOS-CX Simulator Lab - BFD
- AOS-CX Simulator Lab - BGP Internet
- AOS-CX Simulator Lab - Dual-Homed Internet Access with a Single Border Gateway

### Layer 3 Services
- AOS-CX Simulator Lab - IPv4 DHCP
- AOS-CX Simulator Lab - IPv6 SLAAC and DHCPv6

### Multicast
- AOS-CX Simulator Lab - Multicast PIM Dense Mode
- AOS-CX Simulator Lab - Multicast PIM Sparse Mode
- AOS-CX Simulator Lab - PIM-SM Troubleshooting

### Overlay
- AOS-CX Simulator Lab - Static VXLAN
- AOS-CX Simulator Lab - VXLAN EVPN
- AOS-CX Simulator Lab - VXLAN Centralized L3 Gateway
- AOS-CX Simulator Lab - VXLAN EVPN Troubleshooting
- AOS-CX Simulator Lab - VNBT - Virtual Network-Based Tunneling

### Security
- AOS-CX Switch Simulator Lab - ACLs
- AOS-CX Simulator Lab - Local MAC Match Authentication
- AOS-CX Simulator Lab - RADIUS MAC Authentication
- AOS-CX Simulator Lab - RADIUS 802.1X Authentication
- AOS-CX Simulator Lab - RadSec
- AOS-CX Simulator Lab - Local User Roles
- AOS-CX Simulator Lab - Device Profiles

### Management and Orchestration
- AOS-CX Simulator Lab - L2 Access Management and Connectivity
- AOS-CX Simulator Lab - L2 Access Management and Connectivity - Part 2
- AOS-CX Simulator Lab - Web User Interface
- AOS-CX Simulator Lab - Managing CX Checkpoints
- AOS-CX Simulator Lab - SNMP
- AOS-CX Simulator Lab - SNMPv3
- AOS-CX Simulator Lab - NAE IP SLA
- AOS-CX Simulator Lab - sFlow Part 1
- AOS-CX Simulator Lab - Monitoring Aruba CX Switches

### NetEdit Series
- AOS-CX Simulator Lab - NetEdit 2.1 Part 1
- AOS-CX Simulator Lab - NetEdit 2.1 Part 2
- AOS-CX Simulator Lab - NetEdit 2.1 Part 3
- AOS-CX Simulator Lab - NetEdit 2.1 Part 4

### AOS-CX Switch Simulator Lab Tools
- AOS-CX Simulator Lab - Saving Lab Configurations

### Datacenter Series
- AOS-CX Simulator Lab - Data Center Bridging
- AOS-CX Simulator Lab - Datacenter Collapsed Core
- AOS-CX Simulator Lab - Datacenter 2-Tier L2 Fabric
- AOS-CX Simulator Lab - DCN 2 Tier-L3 Fabric iBGP
- AOS-CX Simulator Lab - DCN Layer 3 Spine and Leaf Fabric with OSPF and VXLAN