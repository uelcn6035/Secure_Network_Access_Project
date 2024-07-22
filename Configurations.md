## Access Switches
vlan 10
name production
ex
vlan 20
name Admin
ex
vlan 30
name Finance
ex
vlan 40
name WLAN
ex
vlan 50
name Voice
ex

int range f0/1-2
switchport mode trunk

int range f0/3-10
switchport mode access
switchport access vlan 10
switchport voice vlan 50
spanning-tree portfast
 
int range f0/11-15
switchport mode access
switchport access vlan 20
switchport voice vlan 50
spanning-tree portfast

int range f0/16-20
switchport mode access
switchport access vlan 30
switchport voice vlan 50
spanning-tree portfast

int range f0/21-24
switchport mode access
switchport access vlan 40
```
## Distributed Switch 1
int g1/0/1
no switchport
ip add 172.30.1.137 255.255.255.252
no shut
ex

int range g1/0/2-7
switchport mode trunk
ex

vlan 10
name production
ex
vlan 20
name Admin
ex
vlan 30
name Finance
ex
vlan 40
name WLAN
ex
vlan 50
name Voice
ex

Etherchannel:
int range g1/0/4-6
channel-group 1 mode passive
interface port-channel 1
switchport mode trunk
ex
```
## Distributed Switch 2
int g1/0/1
no switchport
ip add 172.30.1.141 255.255.255.252
no shut
ex

int range g1/0/2-6
switchport mode trunk
ex

vlan 10
name production
ex
vlan 20
name Admin
ex
vlan 30
name Finance
ex
vlan 40
name WLAN
ex
vlan 50
name Voice
ex

Etherchannel:
int range g1/0/4-6
channel-group 1 mode active
interface port-channel 1
switchport mode trunk
ex
```
## WLC
int g1/0/7
switchport mode access
switchport access vlan 40
ex

int g1/0/8
switchport mode access
switchport access vlan 40
spanning-tree portfast
ex

int range g1/0/2-3, g1/0/6-7
switchport mode trunk
```
## DHCP Config of distribution switch 1&2
```bash
conf t
ip dhcp pool vlan_10
network 172.30.1.0 255.255.255.224
default-router 172.30.1.2
ip dhcp excluded-address 172.30.1.1 172.30.1.4
ex

conf t
ip dhcp pool vlan_20
network 172.30.1.32 255.255.255.224
default-router 172.30.1.34
ip dhcp excluded-address 172.30.1.33 172.30.1.36
ex

conf t
ip dhcp pool vlan_30
network 172.30.1.64 255.255.255.224
default-router 172.30.1.66
ip dhcp excluded-address 172.30.1.65 172.30.1.68
ex

conf t
ip dhcp pool vlan_40
network 172.30.1.96 255.255.255.224
default-router 172.30.1.98
ip dhcp excluded-address 172.30.1.97 172.30.1.100
ex
```
## Layer 3 Configuration - Routing
### Distributed Switch 1
HSRP & INTER-VLAN Routing:
int vlan 10
ip add 172.30.1.1 255.255.255.224
standby 10 priority 255
No standby ip 172.30.1.2
ex

int vlan 20
ip add 172.30.1.33 255.255.255.224
standby 20 priority 255
standby 20 ip 172.30.1.34
ex

int vlan 30
ip add 172.30.1.65 255.255.255.224
standby 30 priority 255
standby 30 ip 172.30.1.66
ex

int vlan 40
ip add 172.30.1.97 255.255.255.224
standby 40 priority 255
standby 40 ip 172.30.1.98
ex
```
### Distributed Switch 2
HSRP & INTER-VLAN Routing:

int vlan 10
ip add 172.30.1.3 255.255.255.224
standby 10 priority 100
standby 10 ip 172.30.1.2
ex

int vlan 20
ip add 172.30.1.35 255.255.255.224
standby 20 priority 100
standby 20 ip 172.30.1.34
ex

int vlan 30
ip add 172.30.1.67 255.255.255.224
standby 30 priority 100
standby 30 ip 172.30.1.66
ex

int vlan 40
ip add 172.30.1.99 255.255.255.224
standby 40 priority 100
standby 40 ip 172.30.1.98
ex
```
## Layer 3 Router Links
### ISP Gate(Router)
int gig0/0
ip add 221.30.1.146 255.255.255.252
no shut
ex

int se0/3/0
ip add 30.30.1.149 255.255.255.252
no shut
ex
```
### AWS CLOUD
int se0/3/0
ip add 30.30.1.150 255.255.255.252
no shut
ex

int gig0/1
ip add 172.20.172.17 255.255.255.240
no shut
ex
```
## Dynamic Routing Configuration
### Distribution switch 1
int g1/0/1
ip routing
router ospf 2
router-id 6.6.6.6
network 172.30.1.136 0.0.0.3 area 0
network 172.30.1.0 0.0.0.31 area 0
network 172.30.1.32 0.0.0.31 area 0
network 172.30.1.64 0.0.0.31 area 0
network 172.30.1.96 0.0.0.31 area 0
```
### Distribution switch 2
int g1/0/1
ip routing
router ospf 2
router-id 2.2.2.2
network 172.30.1.140 0.0.0.3 area 0
network 172.30.1.0 0.0.0.31 area 0
network 172.30.1.32 0.0.0.31 area 0
network 172.30.1.64 0.0.0.31 area 0
network 172.30.1.96 0.0.0.31 area 0
```
### FIREWALL OSPF
router ospf 2
router-id 1.1.1.1
network 172.30.1.136 255.255.255.252 area 0
network 172.30.1.140 255.255.255.252 area 0
network 221.30.1.144 255.255.255.252 area 0
ex
```
### ISP CORE
router ospf 2
router-id 5.5.5.5
network 221.30.1.144 0.0.0.3 area 0
network 172.30.1.148 0.0.0.3 area 0
ex
```
### IBM Cloud Instance
router ospf 2
router-id 4.4.4.4
network 30.20.172.16 0.0.0.7 area 0
network 172.30.1.148 0.0.0.3 area 0
ex
```
### FIREWALL OSPF
router ospf 2
router-id 1.1.1.1
network 172.30.1.136 255.255.255.248 area 0
network 172.30.1.140 255.255.255.252 area 0
network 172.30.1.144
