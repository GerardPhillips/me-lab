# media-leaf-4
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Name Servers](#name-servers)
  - [NTP](#ntp)
  - [PTP](#ptp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [AAA Authorization](#aaa-authorization)
- [Aliases](#aliases)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SFlow](#sflow)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
  - [Router Multicast](#router-multicast)
  - [PIM Sparse Mode](#pim-sparse-mode)
- [Filters](#filters)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.90.227.31/24 | 10.90.227.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.90.227.31/24
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 192.168.0.20 | MGMT |
| 8.8.8.8 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 192.168.0.20
```

## NTP

### NTP Summary

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.50.2 | - | True | - | - | - | - | - | - | - |
| 192.168.150.2 | - | - | - | - | - | - | - | - | - |
| pool.ntp.org | - | - | - | - | - | - | - | - | - |

### NTP Device Configuration

```eos
!
ntp server 192.168.50.2 prefer
ntp server 192.168.150.2
ntp server pool.ntp.org
```

## PTP

### PTP Summary

| Clock ID | Source IP | Priority 1 | Priority 2 | TTL | Domain | Mode | Forward Unicast |
| -------- | --------- | ---------- | ---------- | --- | ------ | ---- | --------------- |
| 00:00:00:1e:00:06 | - | 30 | 6 | - | 127 | boundary | - |

### PTP Device Configuration

```eos
!
ptp clock-identity 00:00:00:1e:00:06
ptp priority1 30
ptp priority2 6
ptp domain 127
ptp mode boundary
ptp monitor threshold offset-from-master 250
ptp monitor threshold mean-path-delay 1500
ptp monitor sequence-id
ptp monitor threshold missing-message announce 3 sequence-ids
ptp monitor threshold missing-message delay-resp 3 sequence-ids
ptp monitor threshold missing-message follow-up 3 sequence-ids
ptp monitor threshold missing-message sync 3 sequence-ids
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| admin | 15 | network-admin |
| cvpadmin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 $6$ttzoDdVO8Uz2SiBF$Ge.hQWy9wPGWyGc9.Q/mMyjycNO.9ylh40Vc.0iiqU/MtR7MKKIuOgbKkOeD3EpPMtN2SBForxog2ZCqv0kbu0
username cvpadmin privilege 15 role network-admin secret sha512 $6$n7NC6yuQmsXgOMKP$UZu7iLcCZgydY8NMdGAz244RVKdWqEikN.9ByPifgwQ90xS3fT6.46j5WcfUxkmVYWhWSgbIpODxVw67VkbkQ0
```

## AAA Authorization

### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

# Aliases

```eos
alias sln show lldp neighbors
alias conint sh interface | I connected
alias connect show interfaces status connected
alias descr show int descr
alias drops watch 1 diff show int coun dis | nz
alias dump bash tcpdump -i %1
alias ptpcount watch 1 diff sh ptp interface counters | egrep 'Ethernet|Manage'| grep -v "received: 0" | grep -v sent
alias ptpmgmt watch 1 diff sh ptp int count | egrep "Management messages received: | Ethernet" | nz
alias rates watch 1 diff show int count rates | nz
alias routeage bash echo show ip route | cliribd
alias senz show interface counter error | nz
alias senzwatch watch 1 diff show interface counter error | nz
alias shmc show int | awk '/^[A-Z]/ { intf = $1 } /, address is/ { print intf, $6 }'
alias shptp show ptp int counters drop
alias snoopcount watch 1 diff sh ip igmp snooping counters | nz
alias snoopgroup watch 1 diff sh ip igmp snooping groups | nz
alias snz show interface counter | nz
alias spd show port-channel %1 detail all
alias sqnz show interface counter queue | nz
alias srnz show interface counter rate | nz
alias watchptp watch 1 diff sh ip igmp snooping group 224.0.1.129 | nz
alias igmpgroups show ip igmp snooping groups detail
alias ptpwatch watch 1 diff show ptp
alias sdnz show int count discard | nz
alias sie show ip igmp snoop counters err | nz
alias sqml show queue-monitor length
alias media2main
 1 conf
 2 interface ethernet %1
 3 ip helper 192.168.0.20
 4 no ip helper 192.168.204.20
 5 no ip helper 192.168.208.20
 6 no ip helper 192.168.212.20 
 7 end

alias media2nmos1
 1 conf
 2 interface ethernet %1
 3 no ip helper 192.168.0.20
 4 ip helper 192.168.204.20
 5 no ip helper 192.168.208.20
 6 no ip helper 192.168.212.20 
 7 end

alias media2nmos2
 1 conf
 2 interface ethernet %1
 3 no ip helper 192.168.0.20
 4 no ip helper 192.168.204.20
 5 ip helper 192.168.208.20
 6 no ip helper 192.168.212.20 
 7 end

alias media2nmos3
 1 conf
 2 interface ethernet %1
 3 no ip helper 192.168.0.20
 4 no ip helper 192.168.204.20
 5 no ip helper 192.168.208.20
 6 ip helper 192.168.212.20 
 7 end

alias mgt2main
 1 conf
 2 interface ethernet %1
 3 switchport access vlan 1001
 4 no switchport access vlan 2001
 5 no switchport access vlan 2002
 6 no switchport access vlan 2003
 7 end

alias mgt2nmos1
 1 conf
 2 interface ethernet %1
 3 no switchport access vlan 1001
 4 switchport access vlan 2001
 5 no switchport access vlan 2002
 6 no switchport access vlan 2003
 7 end

alias mgt2nmos2
 1 conf
 2 interface ethernet %1
 3 no switchport access vlan 1001
 4 no switchport access vlan 2001
 5 switchport access vlan 2002
 6 no switchport access vlan 2003
 7 end

alias mgt2nmos3
 1 conf
 2 interface ethernet %1
 3 no switchport access vlan 1001
 4 no switchport access vlan 2001
 5 no switchport access vlan 2002
 6 switchport access vlan 2003
 7 end

!
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | MGMT | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/tmp/cv-onboarding-token -cvvrf=MGMT -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## SFlow

### SFlow Summary

| VRF | SFlow Source Interface | SFlow Destination | Port |
| --- | ---------------------- | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | loopback0 | - | - |

sFlow is enabled.

### SFlow Device Configuration

```eos
!
sflow destination 127.0.0.1
sflow source-interface loopback0
sflow run
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet53/1 | P2P_LINK_TO_MEDIA-SPINE-1_Ethernet25/1 | routed | - | 192.168.99.225/31 | default | 9000 | false | - | - |
| Ethernet54/1 | P2P_LINK_TO_MEDIA-SPINE-2_Ethernet25/1 | routed | - | 192.168.99.227/31 | default | 9000 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet53/1
   description P2P_LINK_TO_MEDIA-SPINE-1_Ethernet25/1
   no shutdown
   mtu 9000
   no switchport
   ip address 192.168.99.225/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet54/1
   description P2P_LINK_TO_MEDIA-SPINE-2_Ethernet25/1
   no shutdown
   mtu 9000
   no switchport
   ip address 192.168.99.227/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 172.24.0.26/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 172.24.0.26/32
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:1c:73:00:dc:01

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:01
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| MGMT | false |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| MGMT | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 10.90.227.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 10.90.227.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65431|  172.24.0.26 |

| BGP Tuning |
| ---------- |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- |
| 192.168.99.224 | 65410 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 192.168.99.226 | 65410 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |

### Router BGP Device Configuration

```eos
!
router bgp 65431
   router-id 172.24.0.26
   maximum-paths 4 ecmp 4
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 192.168.99.224 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.99.224 remote-as 65410
   neighbor 192.168.99.224 description media-spine-1_Ethernet25/1
   neighbor 192.168.99.226 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.99.226 remote-as 65410
   neighbor 192.168.99.226 description media-spine-2_Ethernet25/1
   redistribute connected
   !
   address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
```

## Router Multicast

### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.

### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
```


## PIM Sparse Mode

### Router PIM Sparse Mode

#### IP Sparse Mode Information

##### IP Rendezvous Information

| Rendezvous Point Address | Group Address |
| ------------------------ | ------------- |
| 192.168.99.1 | - |

#### Router Multicast Device Configuration

```eos
!
router pim sparse-mode
   ipv4
      rp address 192.168.99.1
```

### PIM Sparse Mode enabled interfaces

| Interface Name | VRF Name | IP Version | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ----------- | --------------- |
| Ethernet53/1 | - | IPv4 | - | - |
| Ethernet54/1 | - | IPv4 | - | - |

# Filters

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```

# Quality Of Service
