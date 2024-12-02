# media-PTP-1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [NTP](#ntp)
  - [PTP](#ptp)
  - [Management API gNMI](#management-api-gnmi)
  - [Management CVX Summary](#management-cvx-summary)
  - [Management API HTTP](#management-api-http)
  - [Management API Models](#management-api-models)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Roles](#roles)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [Aliases Device Configuration](#aliases-device-configuration)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SNMP](#snmp)
  - [SFlow](#sflow)
- [Monitor Connectivity](#monitor-connectivity)
  - [Global Configuration](#global-configuration)
  - [VRF Configuration](#vrf-configuration)
  - [Monitor Connectivity Device Configuration](#monitor-connectivity-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [Queue Monitor](#queue-monitor)
  - [Queue Monitor Streaming](#queue-monitor-streaming)
  - [Queue Monitor Configuration](#queue-monitor-configuration)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
  - [Router Multicast](#router-multicast)
  - [PIM Sparse Mode](#pim-sparse-mode)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management_interface | oob | MGMT | 10.90.227.21/24 | 10.90.227.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management_interface | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management_interface
   no shutdown
   vrf MGMT
   ip address 10.90.227.21/24
```

### DNS Domain

DNS domain: MnE.lab

#### DNS Domain Device Configuration

```eos
dns domain MnE.lab
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 10.90.227.155 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 10.90.227.155
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| ntp.aristanetworks.com | MGMT | - | - | True | - | - | - | Ma1 | - |

#### NTP Device Configuration

```eos
!
ntp server vrf MGMT ntp.aristanetworks.com iburst local-interface Ma1
```

### PTP
#### PTP Summary

| Clock ID | Source IP | Priority 1 | Priority 2 | TTL | Domain | Mode | Forward Unicast |
| -------- | --------- | ---------- | ---------- | --- | ------ | ---- | --------------- |
| 00:1C:73:0a:00:01 | - | 10 | 1 | - | 127 | boundary | - |

#### PTP Device Configuration

```eos
!
ptp clock-identity 00:1C:73:0a:00:01
ptp domain 127
ptp mode boundary
ptp priority1 10
ptp priority2 1
ptp monitor threshold offset-from-master 250
ptp monitor threshold mean-path-delay 1500
ptp monitor sequence-id
ptp monitor threshold missing-message sync 3 sequence-ids
ptp monitor threshold missing-message follow-up 3 sequence-ids
ptp monitor threshold missing-message delay-resp 3 sequence-ids
ptp monitor threshold missing-message announce 3 sequence-ids
```

### Management API gNMI

#### Management API gNMI Summary

| Transport | SSL Profile | VRF | Notification Timestamp | ACL | Port |
| --------- | ----------- | --- | ---------------------- | --- | ---- |
| grpc | - | MGMT | last-change-time | - | 6030 |

Provider eos-native is configured.

#### Management API gNMI Device Configuration

```eos
!
management api gnmi
   transport grpc grpc
      vrf MGMT
   provider eos-native
```

### Management CVX Summary

| Shutdown | CVX Servers |
| -------- | ----------- |
| False | 10.90.228.50 |

#### Management CVX Device Configuration

```eos
!
management cvx
   no shutdown
   server host 10.90.228.50
   vrf MGMT
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

### Management API Models

#### Management API Models Summary

| Provider | Path | Disabled |
| -------- | ---- | ------- |
| smash | bridging | False |
| smash | ptp | False |
| smash | routing | False |

#### Management API Models Device Configuration

```eos
!
management api models
   !
   provider smash
      path bridging
      path ptp
      path routing
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |
| dataminer | 1 | view-only | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
username dataminer privilege 1 role view-only secret sha512 <removed>
```

### Roles

#### Roles Summary

##### Role view-only

| Sequence | Action | Mode | Command |
| -------- | ------ | ---- | ------- |
| 10 | permit | - | enable |
| 20 | deny | exec | reload |
| 30 | deny | config-all | .* |
| 40 | deny | exec | clear .* |
| 50 | permit | - | show .* |
| 60 | permit | - | bash timeout 1 df -h |

#### Roles Device Configuration

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

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| 0,15 | local |

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
aaa authorization commands 0,15 default local
!
```

## Aliases Device Configuration

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

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
+
| gzip | apiserver.arista.io:443 | MGMT | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |
+
| gzip | apiserver.cv-dev.corp.arista.io:443 | MGMT | token-secure,/tmp/cvdev-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |
+
| gzip | apiserver.cv-staging.corp.arista.io:443 | MGMT | token-secure,/tmp/cvstage-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |
| gzip | 10.244.132.193:9910 | MGMT | token,/tmp/devtoken | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |
| gzip | 10.90.227.161:9910 | MGMT | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvopt cvaas.addr=apiserver.arista.io:443 -cvopt cvaas.auth=token-secure,/tmp/cv-onboarding-token -cvopt cvaas.vrf=MGMT -cvopt cvaasdev.addr=apiserver.cv-dev.corp.arista.io:443 -cvopt cvaasdev.auth=token-secure,/tmp/cvdev-onboarding-token -cvopt cvaasdev.vrf=MGMT -cvopt cvaasstage.addr=apiserver.cv-staging.corp.arista.io:443 -cvopt cvaasstage.auth=token-secure,/tmp/cvstage-onboarding-token -cvopt cvaasstage.vrf=MGMT -cvopt dev.addr=10.244.132.193:9910 -cvopt dev.auth=token,/tmp/devtoken -cvopt dev.vrf=MGMT -cvopt lab.addr=10.90.227.161:9910 -cvopt lab.auth=token,/tmp/token -cvopt lab.vrf=MGMT -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | - | All | Disabled |

#### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| <removed> | ro | - | - | - |
| <removed> | rw | - | - | - |

#### SNMP Device Configuration

```eos
!
snmp-server community <removed> ro
snmp-server community <removed> rw
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | loopback0 | - | - |

sFlow is enabled.

#### SFlow Device Configuration

```eos
!
sflow destination 127.0.0.1
sflow source-interface loopback0
sflow run
```

## Monitor Connectivity

### Global Configuration

#### Probing Configuration

| Enabled | Interval | Default Interface Set | Address Only |
| ------- | -------- | --------------------- | ------------ |
| True | - | - | True |

#### Host Parameters

| Host Name | Description | IPv4 Address | Probing Interface Set | Address Only | URL |
| --------- | ----------- | ------------ | --------------------- | ------------ | --- |
| GM1 | GM1 | 172.24.121.65 | - | True | - |

### VRF Configuration

| Name | Description | Default Interface Set | Address Only |
| ---- | ----------- | --------------------- | ------------ |
| MGMT | - | - | True |

#### Vrf MGMT Configuration

##### Host Parameters

| Host Name | Description | IPv4 Address | Probing Interface Set | Address Only | URL |
| --------- | ----------- | ------------ | --------------------- | ------------ | --- |
| aws-us-east-1 | aws-us-east-1 | 52.216.227.10 | - | True | http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com |
| aws-us-west-2 | aws-us-west-2 | 52.218.182.251 | - | True | http://fredwebsitebuckettest.s3-website-us-west-2.amazonaws.com |
| azure-eastus | aws-us-west-2 | 52.216.227.10 | - | True | http://fredcloudtracereast1.s3-website-us-east-1.amazonaws.com |
| DNS | DNS | 10.90.227.155 | - | True | - |
| Google | Google | 8.8.8.8 | - | True | - |
| OOB-GW | OOB GW | 10.90.227.1 | - | True | - |

### Monitor Connectivity Device Configuration

```eos
!
monitor connectivity
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
      host DNS
         description
         DNS
         ip 10.90.227.155
      !
      host Google
         description
         Google
         ip 8.8.8.8
      !
      host OOB-GW
         description
         OOB GW
         ip 10.90.227.1
   no shutdown
   !
   host GM1
      description
      GM1
      ip 172.24.121.65
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 2222 | vlan_PTP_1 | - |

### VLANs Device Configuration

```eos
!
vlan 2222
   name vlan_PTP_1
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet48 | Meinberg Interface(cu-cu) | access | 2222 | - | - | - |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet51 | P2P_media-spine-1_Ethernet27/1 | - | 192.168.100.1/31 | default | 9214 | False | - | - |
| Ethernet52 | P2P_media-spine-2_Ethernet27/1 | - | 192.168.100.3/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_media-PTP-2_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ptp enable
   ptp announce interval 0
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp sync-message interval -3
   ptp transport ipv4
!
interface Ethernet2
   description P2P_media-PTP-2_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ptp enable
   ptp announce interval 0
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp sync-message interval -3
   ptp transport ipv4
!
interface Ethernet48
   description Meinberg Interface(cu-cu)
   no shutdown
   speed forced 1000full
   switchport access vlan 2222
   switchport mode access
   switchport
   ptp enable
   ptp announce interval 0
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp sync-message interval -3
   ptp transport ipv4
!
interface Ethernet51
   description P2P_media-spine-1_Ethernet27/1
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.100.1/31
   pim ipv4 sparse-mode
   ptp enable
   ptp announce interval 0
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp sync-message interval -3
   ptp transport ipv4
!
interface Ethernet52
   description P2P_media-spine-2_Ethernet27/1
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.100.3/31
   pim ipv4 sparse-mode
   ptp enable
   ptp announce interval 0
   ptp announce timeout 3
   ptp delay-req interval -3
   ptp sync-message interval -3
   ptp transport ipv4
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 172.24.0.21/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 172.24.0.21/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| vlan2222 | - | default | - | - |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| vlan2222 |  default  |  172.24.121.65/30  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface vlan2222
   no autostate
   ip address 172.24.121.65/30
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:dc:01

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:01
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 10.90.227.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 10.90.227.1
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65421 | 172.24.0.21 |

| BGP Tuning |
| ---------- |
| update wait-install |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| BFD | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 192.168.100.0 | 65411 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 192.168.100.2 | 65412 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65421
   router-id 172.24.0.21
   update wait-install
   no bgp default ipv4-unicast
   maximum-paths 4 ecmp 4
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS bfd
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 192.168.100.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.100.0 remote-as 65411
   neighbor 192.168.100.0 description media-spine-1_Ethernet27/1
   neighbor 192.168.100.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.100.2 remote-as 65412
   neighbor 192.168.100.2 description media-spine-2_Ethernet27/1
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```

## Queue Monitor

### Queue Monitor Streaming

| Enabled | IP Access Group | IPv6 Access Group | Max Connections | VRF |
| ------- | --------------- | ----------------- | --------------- | --- |
| True | - | - | - | - |

### Queue Monitor Configuration

```eos
!
queue-monitor streaming
   no shutdown
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

### Router Multicast

#### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.

#### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
```

### PIM Sparse Mode

#### Router PIM Sparse Mode

##### IP Sparse Mode Information

BFD enabled: False

##### IP Rendezvous Information

| Rendezvous Point Address | Group Address | Access Lists | Priority | Hashmask | Override |
| ------------------------ | ------------- | ------------ | -------- | -------- | -------- |
| 172.24.0.10 | - | - | - | - | - |

##### IP Anycast Information

| IP Anycast Address | Other Rendezvous Point Address | Register Count |
| ------------------ | ------------------------------ | -------------- |
| 172.24.0.10 | 172.24.0.11 | - |
| 172.24.0.10 | 172.24.0.12 | - |

##### Router Multicast Device Configuration

```eos
!
router pim sparse-mode
   ipv4
      rp address 172.24.0.10
      anycast-rp 172.24.0.10 172.24.0.11
      anycast-rp 172.24.0.10 172.24.0.12
```

#### PIM Sparse Mode Enabled Interfaces

| Interface Name | VRF Name | IP Version | Border Router | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ------------- | ----------- | --------------- |
| Ethernet51 | - | IPv4 | - | - | - |
| Ethernet52 | - | IPv4 | - | - | - |

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 172.24.0.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 172.24.0.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```
