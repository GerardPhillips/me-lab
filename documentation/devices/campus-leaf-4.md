# campus-leaf-4
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [Name Servers](#name-servers)
  - [NTP](#ntp)
  - [PTP](#ptp)
  - [Management API GNMI](#management-api-gnmi)
  - [Management CVX Summary](#management-cvx-summary)
  - [Management API HTTP](#management-api-http)
  - [Management API Models](#management-api-models)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Roles](#roles)
  - [AAA Authorization](#aaa-authorization)
- [Aliases](#aliases)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [MCS client Summary](#mcs-client-summary)
  - [SNMP](#snmp)
  - [SFlow](#sflow)
- [Monitor Connectivity](#monitor-connectivity)
  - [Global Configuration](#global-configuration)
  - [Vrf Configuration](#vrf-configuration)
  - [Monitor Connectivity Device Configuration](#monitor-connectivity-device-configuration)
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
| Management1 | oob_management | oob | MGMT | - | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.90.227.31/24
```

## DNS Domain

### DNS domain: MnE.lab

### DNS Domain Device Configuration

```eos
dns domain MnE.lab
!
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 10.90.227.155 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 10.90.227.155
```

## NTP

### NTP Summary

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 172.22.22.50 | MGMT | - | - | True | - | - | - | - | - |
| 198.55.111.50 | MGMT | - | - | True | - | - | - | - | - |

### NTP Device Configuration

```eos
!
ntp server vrf MGMT 172.22.22.50 iburst
ntp server vrf MGMT 198.55.111.50 iburst
```

## PTP

### PTP Summary

| Clock ID | Source IP | Priority 1 | Priority 2 | TTL | Domain | Mode | Forward Unicast |
| -------- | --------- | ---------- | ---------- | --- | ------ | ---- | --------------- |
| 00:1C:73:1e:00:0b | - | 30 | 11 | - | 127 | boundary | - |

### PTP Device Configuration

```eos
!
ptp clock-identity 00:1C:73:1e:00:0b
ptp priority1 30
ptp priority2 11
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

## Management API GNMI

### Management API GNMI Summary

| Transport | SSL Profile | VRF | Notification Timestamp | ACL |
| --------- | ----------- | --- | ---------------------- | --- |
| grpc | - | MGMT | last-change-time | - |

Provider eos-native is configured.

### Management API gnmi configuration

```eos
!
management api gnmi
   transport grpc grpc
      vrf MGMT
   provider eos-native
```

## Management CVX Summary

| Shutdown | CVX Servers |
| -------- | ----------- |
| False | 10.90.224.188 |

### Management CVX configuration

```eos
!
management cvx
   no shutdown
   server host 10.90.224.188
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

## Management API Models

### Management API Models Summary

| Provider | Path | Disabled |
| -------- | ---- | ------- |
| smash | bridging | False |
| smash | ptp | False |
| smash | routing | False |

### Management API Models Configuration

```eos
!
management api models
   !
   provider smash
      path bridging
      path ptp
      path routing
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role | Disabled |
| ---- | --------- | ---- | -------- |
| admin | 15 | network-admin | False |
| cvpadmin | 15 | network-admin | False |
| dataminer | 1 | view-only | False |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 $6$ttzoDdVO8Uz2SiBF$Ge.hQWy9wPGWyGc9.Q/mMyjycNO.9ylh40Vc.0iiqU/MtR7MKKIuOgbKkOeD3EpPMtN2SBForxog2ZCqv0kbu0
username cvpadmin privilege 15 role network-admin secret sha512 $6$n7NC6yuQmsXgOMKP$UZu7iLcCZgydY8NMdGAz244RVKdWqEikN.9ByPifgwQ90xS3fT6.46j5WcfUxkmVYWhWSgbIpODxVw67VkbkQ0
username dataminer privilege 1 role view-only secret sha512 $6$tDLfGUdh6qFbD7XV$YOgbgeXei40thdIzlH1/Nz38K2IBkBcA6hYe.ihQnyq3qC92qP/ezmFZcrdOSmbF7xo10VegijeznJrn9X9lc.
```

## Roles

### Roles Summary

#### Role view-only

| Sequence | Action | Mode | Command |
| -------- | ------ | ---- | ------- |
| 10 | permit | - | enable |
| 20 | deny | exec | reload |
| 30 | deny | config-all | .* |
| 40 | deny | exec | clear .* |
| 50 | permit | - | show .* |
| 60 | permit | - | bash timeout 1 df -h |

### Roles Device Configuration

```eos
!
role view-only
   10 permit command enable
   20 deny mode exec command reload
   30 deny mode config-all command .*
   40 deny mode exec command clear .*
   50 permit command show .*
   60 permit command bash timeout 1 df -h
```

## AAA Authorization

### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| 0,15 | local |

### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
aaa authorization commands 0,15 default local
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

!
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 10.90.227.161:9910,cvaas.addr=apiserver.arista.io:443:443 | MGMT | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.90.227.161:9910,cvaas.addr=apiserver.arista.io:443:443 -cvauth=token-secure,/tmp/cv-onboarding-token -cvvrf=MGMT -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## MCS client Summary

MCS client is enabled

### MCS client configuration

```eos
!
mcs client
   no shutdown
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | - | All | Disabled |

### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| ro | ro | - | - | - |
| rw | rw | - | - | - |

### SNMP Device Configuration

```eos
!
snmp-server community ro ro
snmp-server community rw rw
```

## SFlow

### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
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

# Monitor Connectivity

## Global Configuration

### Probing Configuration

| Enabled | Interval | Default Interface Set |
| ------- | -------- | --------------------- |
| True | - | - |

### Host Parameters

| Host Name | Description | IPv4 Address | Probing Interface Set | URL |
| --------- | ----------- | ------------ | --------------------- | --- |
| GM1 | GM1 | 172.24.121.65 | - | - |

## Vrf Configuration

| Name | Description | Default Interface Set |
| ---- | ----------- | --------------------- |
| MGMT | - | - |

### Vrf MGMT Configuration

#### Host Parameters

| Host Name | Description | IPv4 Address | Probing Interface Set | URL |
| --------- | ----------- | ------------ | --------------------- | --- |
| aws-us-east-1 | aws-us-east-1 | 52.216.227.10 | - | http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com |
| aws-us-west-2 | aws-us-west-2 | 52.218.182.251 | - | http://fredwebsitebuckettest.s3-website-us-west-2.amazonaws.com |
| azure-eastus | aws-us-west-2 | 52.216.227.10 | - | http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com |
| Google | Google | 8.8.8.8 | - | - |

## Monitor Connectivity Device Configuration

```eos
!
monitor connectivity
   no shutdown
   !
   host GM1
      description
      GM1
      ip 172.24.121.65
   vrf MGMT
      !
      host aws-us-east-1
         description
         aws-us-east-1
         ip 52.216.227.10
         url http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com
      !
      host aws-us-west-2
         description
         aws-us-west-2
         ip 52.218.182.251
         url http://fredwebsitebuckettest.s3-website-us-west-2.amazonaws.com
      !
      host azure-eastus
         description
         aws-us-west-2
         ip 52.216.227.10
         url http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com
      !
      host Google
         description
         Google
         ip 8.8.8.8
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
| Ethernet1 |  Lawo Ruby 1 | access | 2231 | - | - | - |
| Ethernet2 |  Lawo Ruby 2 | access | 2231 | - | - | - |
| Ethernet3 |  Lawo Link 4 | access | 2231 | - | - | - |

*Inherited from Port-Channel Interface

#### Multicast Routing

| Interface | IP Version | Static Routes Allowed | Multicast Boundaries |
| --------- | ---------- | --------------------- | -------------------- |
| Ethernet53/1 | IPv4 | True | - |
| Ethernet54/1 | IPv4 | True | - |

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet11 | mcs senders/receivers | routed | - | 172.24.231.49/30 | default | - | False | - | - |
| Ethernet53/1 | P2P_LINK_TO_MEDIA-SPINE-1_Ethernet25/1 | routed | - | 192.168.102.161/31 | default | 9000 | False | - | - |
| Ethernet54/1 | P2P_LINK_TO_MEDIA-SPINE-2_Ethernet25/1 | routed | - | 192.168.102.163/31 | default | 9000 | False | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description Lawo Ruby 1
   no shutdown
   speed forced 1000full
   switchport access vlan 2231
   switchport mode access
   switchport
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp role master
!
interface Ethernet2
   description Lawo Ruby 2
   no shutdown
   speed forced 1000full
   switchport access vlan 2231
   switchport mode access
   switchport
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp role master
!
interface Ethernet3
   description Lawo Link 4
   no shutdown
   speed forced 1000full
   switchport access vlan 2231
   switchport mode access
   switchport
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp role master
!
interface Ethernet11
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
   ip address 172.24.231.49/30
!
interface Ethernet12
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet13
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet14
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet15
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet16
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet17
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet18
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet19
   description mcs senders/receivers
   no shutdown
   speed forced 1000full
   no switchport
!
interface Ethernet53/1
   description P2P_LINK_TO_MEDIA-SPINE-1_Ethernet25/1
   no shutdown
   mtu 9000
   no switchport
   ip address 192.168.102.161/31
   multicast ipv4 static
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
   ip address 192.168.102.163/31
   multicast ipv4 static
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
| Loopback0 | Router_ID | default | 172.24.0.31/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | Router_ID | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description Router_ID
   no shutdown
   ip address 172.24.0.31/32
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
| default | True |
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
| default | False |
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
| 65431|  172.24.0.31 |

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

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- |
| 192.168.102.160 | 65411 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 192.168.102.162 | 65412 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |

### Router BGP Device Configuration

```eos
!
router bgp 65431
   router-id 172.24.0.31
   maximum-paths 4 ecmp 4
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 192.168.102.160 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.102.160 remote-as 65411
   neighbor 192.168.102.160 description media-spine-1_Ethernet25/1
   neighbor 192.168.102.162 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.102.162 remote-as 65412
   neighbor 192.168.102.162 description media-spine-2_Ethernet25/1
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

BFD enabled: False

##### IP Rendezvous Information

| Rendezvous Point Address | Group Address | Access Lists | Priority | Hashmask | Override |
| ------------------------ | ------------- | ------------ | -------- | -------- | -------- |
| 172.24.0.10 | - | - | - | - | - |

#### Router Multicast Device Configuration

```eos
!
router pim sparse-mode
   ipv4
      rp address 172.24.0.10
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
