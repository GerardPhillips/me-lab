# ME_LAB

# Table of Contents

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

# Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision |
| --- | ---- | ---- | ------------- | -------- | -------------------------- |
| ME_LAB_FABRIC | media_leaf | campus-leaf-4 | 10.90.227.31/24 | 720XP | Provisioned |
| ME_LAB_FABRIC | media_leaf | media-leaf-1 | 10.90.227.25/24 | 7280R2 | Provisioned |
| ME_LAB_FABRIC | media_leaf | media-leaf-2 | 10.90.227.27/24 | 7280R2 | Provisioned |
| ME_LAB_FABRIC | media_leaf | media-leaf-3 | 10.90.227.29/24 | 7050X2 | Provisioned |
| ME_LAB_FABRIC | media_leaf | media-PTP-1 | 10.90.227.21/24 | 720XP | Provisioned |
| ME_LAB_FABRIC | media_leaf | media-PTP-2 | 10.90.227.23/24 | 720XP | Provisioned |
| ME_LAB_FABRIC | media_spine | media-spine-1 | 10.90.227.11/24 | 7280R3 | Provisioned |
| ME_LAB_FABRIC | media_spine | media-spine-2 | 10.90.227.12/24 | 7280R3 | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| media_leaf | campus-leaf-4 | Ethernet53/1 | media_spine | media-spine-1 | Ethernet25/1 |
| media_leaf | campus-leaf-4 | Ethernet54/1 | media_spine | media-spine-2 | Ethernet25/1 |
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
| media_leaf | media-PTP-1 | Ethernet51/1 | media_spine | media-spine-1 | Ethernet27/1 |
| media_leaf | media-PTP-1 | Ethernet52/1 | media_spine | media-spine-2 | Ethernet27/1 |
| media_leaf | media-PTP-2 | Ethernet51/1 | media_spine | media-spine-1 | Ethernet28/1 |
| media_leaf | media-PTP-2 | Ethernet52/1 | media_spine | media-spine-2 | Ethernet28/1 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 192.168.99.64/28 | 16 | 4 | 25.0 % |
| 192.168.99.128/28 | 16 | 8 | 50.0 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| campus-leaf-4 | Ethernet53/1 | 192.168.99.225/31 | media-spine-1 | Ethernet25/1 | 192.168.99.224/31 |
| campus-leaf-4 | Ethernet54/1 | 192.168.99.227/31 | media-spine-2 | Ethernet25/1 | 192.168.99.226/31 |
| media-leaf-1 | Ethernet49/1 | 192.168.99.129/31 | media-spine-1 | Ethernet1/1 | 192.168.99.128/31 |
| media-leaf-1 | Ethernet50/1 | 192.168.99.133/31 | media-spine-1 | Ethernet2/1 | 192.168.99.132/31 |
| media-leaf-1 | Ethernet51/1 | 192.168.99.131/31 | media-spine-2 | Ethernet1/1 | 192.168.99.130/31 |
| media-leaf-1 | Ethernet52/1 | 192.168.99.135/31 | media-spine-2 | Ethernet2/1 | 192.168.99.134/31 |
| media-leaf-2 | Ethernet49/1 | 192.168.99.161/31 | media-spine-1 | Ethernet3/1 | 192.168.99.160/31 |
| media-leaf-2 | Ethernet50/1 | 192.168.99.165/31 | media-spine-1 | Ethernet4/1 | 192.168.99.164/31 |
| media-leaf-2 | Ethernet51/1 | 192.168.99.163/31 | media-spine-2 | Ethernet3/1 | 192.168.99.162/31 |
| media-leaf-2 | Ethernet52/1 | 192.168.99.167/31 | media-spine-2 | Ethernet4/1 | 192.168.99.166/31 |
| media-leaf-3 | Ethernet49/1 | 192.168.99.193/31 | media-spine-1 | Ethernet17/1 | 192.168.99.192/31 |
| media-leaf-3 | Ethernet50/1 | 192.168.99.197/31 | media-spine-1 | Ethernet18/1 | 192.168.99.196/31 |
| media-leaf-3 | Ethernet51/1 | 192.168.99.195/31 | media-spine-2 | Ethernet17/1 | 192.168.99.194/31 |
| media-leaf-3 | Ethernet52/1 | 192.168.99.199/31 | media-spine-2 | Ethernet18/1 | 192.168.99.198/31 |
| media-PTP-1 | Ethernet51/1 | 192.168.99.65/31 | media-spine-1 | Ethernet27/1 | 192.168.99.64/31 |
| media-PTP-1 | Ethernet52/1 | 192.168.99.67/31 | media-spine-2 | Ethernet27/1 | 192.168.99.66/31 |
| media-PTP-2 | Ethernet51/1 | 192.168.99.97/31 | media-spine-1 | Ethernet28/1 | 192.168.99.96/31 |
| media-PTP-2 | Ethernet52/1 | 192.168.99.99/31 | media-spine-2 | Ethernet28/1 | 192.168.99.98/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.24.0.0/24 | 256 | 8 | 3.13 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| ME_LAB_FABRIC | campus-leaf-4 | 172.24.0.26/32 |
| ME_LAB_FABRIC | media-leaf-1 | 172.24.0.23/32 |
| ME_LAB_FABRIC | media-leaf-2 | 172.24.0.24/32 |
| ME_LAB_FABRIC | media-leaf-3 | 172.24.0.25/32 |
| ME_LAB_FABRIC | media-PTP-1 | 172.24.0.21/32 |
| ME_LAB_FABRIC | media-PTP-2 | 172.24.0.22/32 |
| ME_LAB_FABRIC | media-spine-1 | 172.24.0.11/32 |
| ME_LAB_FABRIC | media-spine-2 | 172.24.0.12/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
