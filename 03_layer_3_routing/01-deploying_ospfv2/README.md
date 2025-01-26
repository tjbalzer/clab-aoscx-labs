# Lab Guide: Configuring OSPF on Aruba CX Switches

## Lab Objective
At the end of this workshop, you will be able to implement the fundamentals of deploying an OSPFv2 network based on Aruba CX Switches. A successful deployment will show IPv4 HostA and IPv4 HostB have connectivity over the OSPF fabric.

## Lab Overview
OSPFv2 is the IPv4 implementation of Open Shortest Path First protocol (OSPFv3 is the IPv6 implementation of this protocol). It is a link-state based IGP (Interior Gateway Protocol) routing protocol. It is widely used with medium to large sized enterprise networks.

The characteristics of OSPFv2 are:

- Provides a loop-free topology using SPF algorithm.
- Allows hierarchical routing using area 0 (backbone area) as the top of the hierarchy.
- Supports load balancing with equal cost routes for the same destination.
- OSPFv2 is a classless protocol and allows for a hierarchical design with VLSM (Variable Length Subnet
- Masking) and route summarization.
- Provides authentication of routing messages.
- Scales easily using the concept of OSPF areas.
- Provides fast convergence with triggered, incremental updates via LSAs.

Some OSPFv2 configuration is done in the global configuration context, others in the router ospf context, or in the interface configuration context, or in the vlink context. OSPFv2 can be configured on L3 ports, VLAN interfaces, LAG interfaces, and loopback interfaces. All such configurations work in the mentioned interfaces context. OSPFv2 mandates the associated interface to be a routed interface.

The protocol uses Link State Advertisements (LSAs) transmitted by each router to update neighboring routers regarding its interfaces and the routes available through those interfaces. Each routing switch in an area also maintains a link -state database (LSDB) that describes the area topology. (All routers in a given OSPF area have identical LSDBs.) The routing switches used to connect areas to each other flood summary link LSAs and external link LSAs to neighboring OSPF areas to update them regarding available routes. Through this means, each OSPF router determines the shortest path between itself and a desired destination router in the same OSPF domain (Autonomous System (AS)).

## Lab Network Layout

## Lab Tasks
To complete the lab, you should follow the following steps:
1. Lab set-up
2. Configure HostA and HostB
3. Configure switch to switch interfaces, loopbacks, and VLANs
4. Configure OSPFv2
5. Verify OSPF peering is up
6. Verify HostA to HostB connectivity

Notes:
- Accessing the lab tasks panel are not available when using the community edition. The Lab Guide is fully available use.
- Many commands, except ‘show’ & ‘ping’, are configuration commands and need to be entered in the proper switch configuration mode. If a command does not work make sure you are in the right configuration context.

### Task 1 – Lab Setup

- Start all the devices, including host and client
- Open each switch console and log in with user “admin” and no password
- Change all hostnames as shown in the topology (Use ```SwitchA```, ```SwitchB```, etc. to replace ```<hostname>```:
```
hostname <hostname>
```
- On all devices, bring up required ports:
```
int 1/1/1-1/1/3
  no shutdown
```
- Validate LLDP neighbors appear as expected
```
show lldp neighbor
```

#### SwitchB
```
SwitchB(config)# show lldp neighbor-info
LLDP Neighbor Information
=========================
Total Neighbor Entries : 2
Total Neighbor Entries Deleted : 0
Total Neighbor Entries Dropped : 0
Total Neighbor Entries Aged-Out : 0

LOCAL-PORT CHASSIS-ID PORT-ID PORT-DESC TTL SYS-NAME
------------------------------------------------------------------------------------------
1/1/1 08:00:09:1d:a2:66 1/1/1 1/1/1 120 SwitchA
1/1/2 08:00:09:23:64:e7 1/1/2 1/1/2 120 SwitchD
```

### Task 2 - Configure HostA and HostB

Already done in topology file:

```
    HostA:
      kind: linux
      mgmt-ipv4: 172.10.103.51                              # Managemnt IP address, reachable via ssh (eth0)
      exec:
        - ip address add 8.1.1.1/24 dev eth1                # internal eth1 interface to reach network topology
        - ip route add 0.0.0.0/4 via 8.1.1.254 dev eth1     # routes to reach 1.1.1.x to 8.1.1.x addresses in network topology
        - ip route add 100.0.0.0/5 via 8.1.1.254 dev eth1   # routes to reach 100.1.1.x to 103.1.1.x addresses in network topology

    HostB:
      kind: linux
      mgmt-ipv4: 172.10.103.52
      exec:
        - ip address add 7.1.1.1/24 dev eth1
        - ip route add 0.0.0.0/4 via 7.1.1.254 dev eth1
        - ip route add 100.0.0.0/5 via 7.1.1.254 dev eth1
```

### Task 3 - Configure interfaces and verify direct connectivity
- Configure switch interfaces and ensure direct connectivity works
- Apply proper IPv4 Addresses to all interfaces, including loopback
- On Switch A and C:
-- Create Host facing VLAN/Interface
-- Apply proper VLAN to host facing access interface
- Ensure direct connectivity works between each link

#### SwitchA
```
vlan 8
  description HostA VLAN
interface 1/1/1
  no shutdown
  description To SwitchB
  ip address 100.1.1.0/31
interface 1/1/2
  no shutdown
  description To SwitchC
  ip address 101.1.1.0/31
interface 1/1/3
  no shutdown
  description Host Segment
  no routing
  vlan access 8
interface loopback 0
  ip address 1.1.1.1/32
  interface vlan 8
  description To Host VLAN
  ip address 8.1.1.254/24
```

#### SwitchB
```
interface 1/1/1
  no shutdown
  description To SwitchA
  ip address 100.1.1.1/31
interface 1/1/2
  no shutdown
  description To SwitchD
  ip address 102.1.1.1/31
interface loopback 0
  ip address 2.2.2.2/32
```

#### SwitchC
```
interface 1/1/1
  no shutdown
  description To SwitchD
  ip address 103.1.1.1/31
interface 1/1/2
  no shutdown
  description To SwitchA
  ip address 101.1.1.1/31
interface loopback 0
  ip address 3.3.3.3/32
```

#### SwitchD
```
vlan 7
  description HostB VLAN
interface 1/1/1
  no shutdown
  description To SwitchC
  ip address 103.1.1.0/31
interface 1/1/2
  no shutdown
  description To SwitchB
  ip address 102.1.1.0/31
interface 1/1/3
  no shutdown
  description HostB Segment
  no routing
  vlan access 7
interface loopback 0
  ip address 4.4.4.4/32
interface vlan 7
  description To Host segment
  ip address 7.1.1.254/24
```

##### SwitchA
```
SwitchA(config)# ping 2.2.2.2
connect: Network is unreachable

SwitchA(config)# ping 102.1.1.0
connect: Network is unreachable

SwitchA(config)# ping 100.1.1.1
PING 100.1.1.1 (100.1.1.1) 100(128) bytes of data.
108 bytes from 100.1.1.1: icmp_seq=1 ttl=64 time=5.80 ms
108 bytes from 100.1.1.1: icmp_seq=2 ttl=64 time=1.87 ms
108 bytes from 100.1.1.1: icmp_seq=3 ttl=64 time=1.90 ms
108 bytes from 100.1.1.1: icmp_seq=4 ttl=64 time=1.92 ms
108 bytes from 100.1.1.1: icmp_seq=5 ttl=64 time=1.78 ms

--- 100.1.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4004ms
rtt min/avg/max/mdev = 1.781/2.658/5.809/1.577 ms

SwitchA(config)# ping 101.1.1.1
PING 101.1.1.1 (101.1.1.1) 100(128) bytes of data.
108 bytes from 101.1.1.1: icmp_seq=1 ttl=64 time=2.21 ms
108 bytes from 101.1.1.1: icmp_seq=2 ttl=64 time=2.24 ms
108 bytes from 101.1.1.1: icmp_seq=3 ttl=64 time=1.63 ms
108 bytes from 101.1.1.1: icmp_seq=4 ttl=64 time=1.87 ms
108 bytes from 101.1.1.1: icmp_seq=5 ttl=64 time=2.14 ms

--- 101.1.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4004ms
rtt min/avg/max/mdev = 1.631/2.022/2.249/0.241 ms
```

#### Task 4 - Configure OSPF
- Create OSPF process 1 and area 0
- Create an OSPF IPv4 Router-ID (same as Loopback)
- Use OSPF point to point
- Ensure Loopback 0 and switch to switch interfaces are advertised into OSPF
- Ensure VLAN 7 interface on Switch D is advertised into OSPF
- Ensure VLAN 8 interface on Switch A is advertised into OSPF
- Verify OSPF peering is up
- Verify that HostA can reach HostB

#### SwitchA
```
router ospf 1
  router-id 1.1.1.1
  area 0.0.0.0
interface 1/1/1
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface 1/1/2
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface loopback 0
  ip ospf 1 area 0.0.0.0
interface vlan 8
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
```

####SwitchB
```
router ospf 1
  router-id 2.2.2.2
  area 0.0.0.0
interface 1/1/1
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface 1/1/2
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface loopback 0
  ip ospf 1 area 0.0.0.0
```

#### SwitchC
```
router ospf 1
  router-id 3.3.3.3
  area 0.0.0.0
interface 1/1/1
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface 1/1/2
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface loopback 0
  ip ospf 1 area 0.0.0.0
```

#### SwitchD
```
router ospf 1
  router-id 4.4.4.4
  area 0.0.0.0
interface 1/1/1
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface 1/1/2
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
interface loopback 0
  ip ospf 1 area 0.0.0.0
interface vlan 7
  ip ospf 1 area 0.0.0.0
  ip ospf network point-to-point
```

#### SwitchA
```
SwitchA(config)# show ip ospf neighbors
OSPF Process ID 1 VRF default
==============================
Total Number of Neighbors: 2

Neighbor ID Priority State Nbr Address Interface
-------------------------------------------------------------------------
2.2.2.2 n/a FULL 100.1.1.1 1/1/1
3.3.3.3 n/a FULL 101.1.1.1 1/1/2
```

#### SwitchB
```
SwitchB(config)# show ip ospf neighbors
OSPF Process ID 1 VRF default
==============================
Total Number of Neighbors: 2

Neighbor ID Priority State Nbr Address Interface
-------------------------------------------------------------------------
1.1.1.1 n/a FULL 100.1.1.0 1/1/1
4.4.4.4 n/a FULL 102.1.1.0 1/1/2
```

#### SwitchC
```
SwitchC(config)# show ip ospf neighbors
OSPF Process ID 1 VRF default
==============================
Total Number of Neighbors: 2

Neighbor ID Priority State Nbr Address Interface
-------------------------------------------------------------------------
4.4.4.4 n/a FULL 103.1.1.0 1/1/1
1.1.1.1 n/a FULL 101.1.1.0 1/1/2
```

#### SwitchD
```
SwitchD(config)# show ip ospf neighbors
OSPF Process ID 1 VRF default
==============================
Total Number of Neighbors: 2

Neighbor ID Priority State Nbr Address Interface
-------------------------------------------------------------------------
3.3.3.3 n/a FULL 103.1.1.1 1/1/1
2.2.2.2 n/a FULL 102.1.1.1 1/1/2
```

#### HostA
```
[*]─[HostA]─[/]
└──> ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
303: eth0@if304: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:0a:67:33 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.10.103.51/24 brd 172.10.103.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe0a:6733/64 scope link 
       valid_lft forever preferred_lft forever
316: eth1@if315: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9500 qdisc noqueue state UP group default 
    link/ether aa:c1:ab:df:a7:6b brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 8.1.1.1/24 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a8c1:abff:fedf:a76b/64 scope link 
       valid_lft forever preferred_lft forever

[*]─[HostA]─[/]
└──> ping -c 5 7.1.1.1
PING 7.1.1.1 (7.1.1.1) 56(84) bytes of data.
64 bytes from 7.1.1.1: icmp_seq=1 ttl=61 time=3.39 ms
64 bytes from 7.1.1.1: icmp_seq=2 ttl=61 time=3.00 ms
64 bytes from 7.1.1.1: icmp_seq=3 ttl=61 time=3.26 ms
64 bytes from 7.1.1.1: icmp_seq=4 ttl=61 time=2.96 ms
64 bytes from 7.1.1.1: icmp_seq=5 ttl=61 time=3.18 ms

--- 7.1.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4003ms
rtt min/avg/max/mdev = 2.961/3.155/3.390/0.160 ms
```

#### HostB
```
[*]─[HostB]─[/]
└──> ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
305: eth0@if306: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:0a:67:34 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.10.103.52/24 brd 172.10.103.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe0a:6734/64 scope link 
       valid_lft forever preferred_lft forever
324: eth1@if323: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9500 qdisc noqueue state UP group default 
    link/ether aa:c1:ab:98:cf:ff brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 7.1.1.1/24 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a8c1:abff:fe98:cfff/64 scope link 
       valid_lft forever preferred_lft forever

[*]─[HostB]─[/]
└──> ping -c 5 8.1.1.1
PING 8.1.1.1 (8.1.1.1) 56(84) bytes of data.
64 bytes from 8.1.1.1: icmp_seq=1 ttl=61 time=3.61 ms
64 bytes from 8.1.1.1: icmp_seq=2 ttl=61 time=3.06 ms
64 bytes from 8.1.1.1: icmp_seq=3 ttl=61 time=3.25 ms
64 bytes from 8.1.1.1: icmp_seq=4 ttl=61 time=3.27 ms
64 bytes from 8.1.1.1: icmp_seq=5 ttl=61 time=3.19 ms

--- 8.1.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4005ms
rtt min/avg/max/mdev = 3.056/3.274/3.605/0.181 ms
```

## Appendix - Complete Configurations

### SwitchA
```
SwitchA# sh run
Current configuration:
!
!Version ArubaOS-CX Virtual.10.14.1000
!export-password: default
hostname SwitchA
user admin group administrators password ciphertext AQBapbR6GqcnU7jI3orA12LkmGhvt7Zul5tHJWx7XOEeypZCYgAAAFV/8gTzoIKkc7d78vWwevDu2X7f39Tj3v6LvXYvqDlYYOMZxbe5oqIJ35DzAlOFNzznfJ3xeBilF58SedMFn9b5LR/HUqDOP8t/olx7jf0mrm7jAK18+ennhlaEy5e2TAJ9
ntp server pool.ntp.org minpoll 4 maxpoll 4 iburst
ntp enable
ntp vrf mgmt
!
!
!
!
!
!
ssh server vrf mgmt
vlan 1
vlan 8
    description HostA VLAN
interface mgmt
    no shutdown
    ip static 10.0.0.15/24
    default-gateway 10.0.0.2
interface 1/1/1
    description To SwitchB
    no shutdown
    ip address 100.1.1.0/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/2
    description To SwitchC
    no shutdown
    ip address 101.1.1.0/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/3
    description Host Segment
    no shutdown
    no routing
    vlan access 8
interface loopback 0
    ip address 1.1.1.1/32
    ip ospf 1 area 0.0.0.0
interface vlan 8
    description Host VLAN
    ip address 8.1.1.254/24
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
!
!
!
!
!
router ospf 1
    router-id 1.1.1.1
    area 0.0.0.0
https-server vrf mgmt
```

### SwitchB
```
SwitchB# sh run
Current configuration:
!
!Version ArubaOS-CX Virtual.10.14.1000
!export-password: default
hostname SwitchB
user admin group administrators password ciphertext AQBapR5nDzeKrA9qdLpAp0e1FSMCmID5ZFxT/vHriSqTascOYgAAAJA72vV8qfdivilOaqla2olg2BHt/BV/TbfivhBGMIWkZrv942SoXnyPyHTzb+Q5NOxGbs/A/bsFVCkSMd8seeLVO+mrmabeOFXirq/qHjo1za6g8RswB1KKpZ09Gt+eYyUe
ntp server pool.ntp.org minpoll 4 maxpoll 4 iburst
ntp enable
ntp vrf mgmt
!
!
!
!
!
!
ssh server vrf mgmt
vlan 1
interface mgmt
    no shutdown
    ip static 10.0.0.15/24
    default-gateway 10.0.0.2
interface 1/1/1
    description To SwitchA
    no shutdown
    ip address 100.1.1.1/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/2
    description To SwitchD
    no shutdown
    ip address 102.1.1.1/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface loopback 0
    ip address 2.2.2.2/32
    ip ospf 1 area 0.0.0.0
!
!
!
!
!
router ospf 1
    router-id 2.2.2.2
    area 0.0.0.0
https-server vrf mgmt
```

### SwitchC
```
SwitchC# sh run
Current configuration:
!
!Version ArubaOS-CX Virtual.10.14.1000
!export-password: default
hostname SwitchC
user admin group administrators password ciphertext AQBape1XWD07AmI1iLtlUu6291ur/jtR1fFUVuAqbVQVRecGYgAAAKWRp/1F3TtEudmWtO1Ar4WFTuJjU3isqMOB+w2P90WmasE5+F20iCNU4T4YaBY8LB38CxtZX3SAf4g7pu1dHcIaM65xS3OQhZHheqKL1xb3N7EeHde8f6/PzMgQzpRxi0GY
ntp server pool.ntp.org minpoll 4 maxpoll 4 iburst
ntp enable
ntp vrf mgmt
!
!
!
!
!
!
ssh server vrf mgmt
vlan 1
interface mgmt
    no shutdown
    ip static 10.0.0.15/24
    default-gateway 10.0.0.2
interface 1/1/1
    description To SwitchD
    no shutdown
    ip address 103.1.1.1/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/2
    description To SwitchA
    no shutdown
    ip address 101.1.1.1/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface loopback 0
    ip address 3.3.3.3/32
    ip ospf 1 area 0.0.0.0
!
!
!
!
!
router ospf 1
    router-id 3.3.3.3
    area 0.0.0.0
https-server vrf mgmt
```

### SwitchD
```
SwitchD# sh run
Current configuration:
!
!Version ArubaOS-CX Virtual.10.14.1000
!export-password: default
hostname SwitchD
user admin group administrators password ciphertext AQBapew9XRPZsYxERKcBML2ACVyGvKj6tr9bSDQdKGBl29jnYgAAABhN0frP4CaZJ1GUm+DWtHg087dbBCNGIbhY8OZ8JgjtSU+LOTiDOCHlD+V/CNkfFcPlxiU2DxH0xvOsBCixcPjS/bbKuC+JmagvocQrf1IAu/4oZsnNB/sGtkJJVKvsjuC1
ntp server pool.ntp.org minpoll 4 maxpoll 4 iburst
ntp enable
ntp vrf mgmt
!
!
!
!
!
!
ssh server vrf mgmt
vlan 1
vlan 7
    description HostB VLAN
interface mgmt
    no shutdown
    ip static 10.0.0.15/24
    default-gateway 10.0.0.2
interface 1/1/1
    description To SwitchC
    no shutdown
    ip address 103.1.1.0/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/2
    description To SwitchB
    no shutdown
    ip address 102.1.1.0/31
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
interface 1/1/3
    description HostB Segment
    no shutdown
    no routing
    vlan access 7
interface loopback 0
    ip address 4.4.4.4/32
    ip ospf 1 area 0.0.0.0
interface vlan 7
    description To Host VLAN
    ip address 7.1.1.254/24
    ip ospf 1 area 0.0.0.0
    ip ospf network point-to-point
!
!
!
!
!
router ospf 1
    router-id 4.4.4.4
    area 0.0.0.0
https-server vrf mgmt
```

### HostA
Refer to topology file:
```
    HostA:
      kind: linux
      mgmt-ipv4: 172.10.103.51
      exec:
        - ip address add 8.1.1.1/24 dev eth1
        - ip route add 0.0.0.0/4 via 8.1.1.254 dev eth1
        - ip route add 100.0.0.0/5 via 8.1.1.254 dev eth1
```

### HostB
Refer to topology file:
```
    HostB:
      kind: linux
      mgmt-ipv4: 172.10.103.52
      exec:
        - ip address add 7.1.1.1/24 dev eth1
        - ip route add 0.0.0.0/4 via 7.1.1.254 dev eth1
        - ip route add 100.0.0.0/5 via 7.1.1.254 dev eth1

```

