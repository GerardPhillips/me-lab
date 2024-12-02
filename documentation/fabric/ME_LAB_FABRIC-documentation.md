# ME_LAB_FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| DC1 | media_leaf | media-leaf-1 | 10.90.227.25/24 | 7280R2 | Provisioned | JPE20302333 |
| DC1 | media_leaf | media-leaf-2 | 10.90.227.27/24 | 7280R2 | Provisioned | JPE20302208 |
| DC1 | media_leaf | media-leaf-3 | 10.90.227.29/24 | 7050X3 | Provisioned | JPE20282733 |
| DC1 | media_leaf | media-leaf-4 | 10.90.227.31/24 | 720XP | Provisioned | JPE19363421 |
| DC1 | ptp_leaf | media-PTP-1 | 10.90.227.21/24 | 720XP | Provisioned | JPE20284615 |
| DC1 | ptp_leaf | media-PTP-2 | 10.90.227.23/24 | 720XP | Provisioned | JPE20284734 |
| DC1 | media_spine | media-spine-1 | 10.90.227.11/24 | 7280R3 | Provisioned | JPE20244151 |
| DC1 | media_spine | media-spine-2 | 10.90.227.12/24 | 7280R3 | Provisioned | JPE21090526 |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| media_leaf | media-leaf-1 | Ethernet49/1 | media_spine | media-spine-1 | Ethernet1/1 |
| media_leaf | media-leaf-1 | Ethernet50/1 | media_spine | media-spine-1 | Ethernet2/1 |
| media_leaf | media-leaf-1 | Ethernet51/1 | media_spine | media-spine-2 | Ethernet1/1 |
| media_leaf | media-leaf-1 | Ethernet52/1 | media_spine | media-spine-2 | Ethernet2/1 |
| media_leaf | media-leaf-2 | Ethernet49/1 | media_spine | media-spine-1 | Ethernet3/1 |
| media_leaf | media-leaf-2 | Ethernet50/1 | media_spine | media-spine-1 | Ethernet4/1 |
| media_leaf | media-leaf-2 | Ethernet51/1 | media_spine | media-spine-2 | Ethernet3/1 |
| media_leaf | media-leaf-2 | Ethernet52/1 | media_spine | media-spine-2 | Ethernet4/1 |
| media_leaf | media-leaf-3 | Ethernet49/1 | media_spine | media-spine-1 | Ethernet17/1 |
| media_leaf | media-leaf-3 | Ethernet50/1 | media_spine | media-spine-1 | Ethernet18/1 |
| media_leaf | media-leaf-3 | Ethernet51/1 | media_spine | media-spine-2 | Ethernet17/1 |
| media_leaf | media-leaf-3 | Ethernet52/1 | media_spine | media-spine-2 | Ethernet18/1 |
| media_leaf | media-leaf-4 | Ethernet53/1 | media_spine | media-spine-1 | Ethernet25/1 |
| media_leaf | media-leaf-4 | Ethernet54/1 | media_spine | media-spine-2 | Ethernet25/1 |
| ptp_leaf | media-PTP-1 | Ethernet1 | ptp_leaf | media-PTP-2 | Ethernet1 |
| ptp_leaf | media-PTP-1 | Ethernet2 | ptp_leaf | media-PTP-2 | Ethernet2 |
| ptp_leaf | media-PTP-1 | Ethernet51 | media_spine | media-spine-1 | Ethernet27/1 |
| ptp_leaf | media-PTP-1 | Ethernet52 | media_spine | media-spine-2 | Ethernet27/1 |
| ptp_leaf | media-PTP-2 | Ethernet51 | media_spine | media-spine-1 | Ethernet28/1 |
| ptp_leaf | media-PTP-2 | Ethernet52 | media_spine | media-spine-2 | Ethernet28/1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 192.168.100.0/24 | 256 | 8 | 3.13 % |
| 192.168.101.0/24 | 256 | 0 | 0.0 % |
| 192.168.102.0/24 | 256 | 28 | 10.94 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| media-leaf-1 | Ethernet49/1 | 192.168.102.65/31 | media-spine-1 | Ethernet1/1 | 192.168.102.64/31 |
| media-leaf-1 | Ethernet50/1 | 192.168.102.69/31 | media-spine-1 | Ethernet2/1 | 192.168.102.68/31 |
| media-leaf-1 | Ethernet51/1 | 192.168.102.67/31 | media-spine-2 | Ethernet1/1 | 192.168.102.66/31 |
| media-leaf-1 | Ethernet52/1 | 192.168.102.71/31 | media-spine-2 | Ethernet2/1 | 192.168.102.70/31 |
| media-leaf-2 | Ethernet49/1 | 192.168.102.97/31 | media-spine-1 | Ethernet3/1 | 192.168.102.96/31 |
| media-leaf-2 | Ethernet50/1 | 192.168.102.101/31 | media-spine-1 | Ethernet4/1 | 192.168.102.100/31 |
| media-leaf-2 | Ethernet51/1 | 192.168.102.99/31 | media-spine-2 | Ethernet3/1 | 192.168.102.98/31 |
| media-leaf-2 | Ethernet52/1 | 192.168.102.103/31 | media-spine-2 | Ethernet4/1 | 192.168.102.102/31 |
| media-leaf-3 | Ethernet49/1 | 192.168.102.129/31 | media-spine-1 | Ethernet17/1 | 192.168.102.128/31 |
| media-leaf-3 | Ethernet50/1 | 192.168.102.133/31 | media-spine-1 | Ethernet18/1 | 192.168.102.132/31 |
| media-leaf-3 | Ethernet51/1 | 192.168.102.131/31 | media-spine-2 | Ethernet17/1 | 192.168.102.130/31 |
| media-leaf-3 | Ethernet52/1 | 192.168.102.135/31 | media-spine-2 | Ethernet18/1 | 192.168.102.134/31 |
| media-leaf-4 | Ethernet53/1 | 192.168.102.161/31 | media-spine-1 | Ethernet25/1 | 192.168.102.160/31 |
| media-leaf-4 | Ethernet54/1 | 192.168.102.163/31 | media-spine-2 | Ethernet25/1 | 192.168.102.162/31 |
| media-PTP-1 | Ethernet51 | 192.168.100.1/31 | media-spine-1 | Ethernet27/1 | 192.168.100.0/31 |
| media-PTP-1 | Ethernet52 | 192.168.100.3/31 | media-spine-2 | Ethernet27/1 | 192.168.100.2/31 |
| media-PTP-2 | Ethernet51 | 192.168.100.65/31 | media-spine-1 | Ethernet28/1 | 192.168.100.64/31 |
| media-PTP-2 | Ethernet52 | 192.168.100.67/31 | media-spine-2 | Ethernet28/1 | 192.168.100.66/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.24.0.0/24 | 256 | 8 | 3.13 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC1 | media-leaf-1 | 172.24.0.25/32 |
| DC1 | media-leaf-2 | 172.24.0.27/32 |
| DC1 | media-leaf-3 | 172.24.0.29/32 |
| DC1 | media-leaf-4 | 172.24.0.31/32 |
| DC1 | media-PTP-1 | 172.24.0.21/32 |
| DC1 | media-PTP-2 | 172.24.0.23/32 |
| DC1 | media-spine-1 | 172.24.0.11/32 |
| DC1 | media-spine-2 | 172.24.0.12/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC1 | media-spine-1 | 172.24.0.10/32 |
| DC1 | media-spine-2 | 172.24.0.10/32 |
