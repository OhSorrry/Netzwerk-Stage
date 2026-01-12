

Management IP auf Switches in anderem Subnetz als Pcs
3 Subnetze- Linux, Windows, Switch
Spanning Tree Protocol anschauen
Spanning Tree Route über S1 ()  




| Bauteil:   | Name:      | IP-Adresse:                                                                         | Description:                   |
| ---------- | ---------- | ----------------------------------------------------------------------------------- | ------------------------------ |
| Windows PC | *TLABPC01* | *172.21.3.10*                                                                       | -                              |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                                                       | -                              |
| Router     | *TLABr08*  | **FA0/0.1:** *172.21.1.1*<br>**FA0/0.2:** *172.21.2.1*<br>**FA0/0.3:** *172.21.3.1* | MGMT<br>LIN<br>WIN             |
| Switch 1   | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                              | TLABr08<br>TLABs06<br>TLABs21  |
| Switch 2   | *TLABs21*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -                                  | TLABPC01<br>TLABs05<br>TLABs06 |
| Switch 3   | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -                                  | TLABPC02<br>TLABs21<br>TLABs05 |


---
---
## Router

Grundkonfig:
```cisco
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABr08
```
---
```cisco
enable password cisco
line console 0
password cisco
end
```
---
```cisco
conf t
banner #
+-------------------------+
|    Zugang verboten!     |
+-------------------------+
#
```
---
```cisco
line vty 0 4
password cisco
login
transport input telnet
end
```
---
VLANS:
```cisco
conf t
vlan 10
name MGMT-VLAN
exit
vlan 20
name LIN-VLAN
exit
vlan 30
name WIN-VLAN
exit
vlan 999
name Dummy-VLAN
end
```
---
```cisco
conf t
interface FA0/1 
description unbenutzt
switchport mode access
switchport access vlan 999
shutdown
exit
```
---
Interface IP-Adressen
```cisco
conf t
interface fa 0/0.10
ip address 172.21.10.1 255.255.255.0
interface description MGMT
interface fa 0/0.20
ip address 172.21.20.1 255.255.255.0
interface description LIN
interface fa 0/0.30
ip address 172.21.30.1 255.255.255.0
Interface description WIN
no shutdown
end
```
---
```cisco
conf t
interface fa 0/0.10
description MGMT
no shutdown
exit
interface fa 0/0.20
description LIN
no shutdown
exit
interface fa 0/0.30
description WIN
no shutdown
exit
exit
```





---
---
## Switch

```cisco
conf t
vlan 10
name MGMT-VLAN
exit
vlan 20
name LIN-VLAN
exit
vlan 30
name WIN-VLAN
exit
vlan 999
name Dummy-VLAN
end
```
---
```cisco
conf t
interface range FA1/0/4 - 48
description unbenutzt
switchport mode access
switchport access vlan 999
shutdown
exit
```
---
```cisco
conf t
interface range Gi1/0/1 - 4
description unbenutzt
switchport mode access
switchport access vlan 999
shutdown
end
```
---
