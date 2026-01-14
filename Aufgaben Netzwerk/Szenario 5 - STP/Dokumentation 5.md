

Management IP auf Switches in anderem Subnetz als Pcs
3 Subnetze- Linux, Windows, Switch
Spanning Tree Protocol anschauen
Spanning Tree Route über S1 ()  




| Bauteil:   | Name:      | IP-Adresse:                                                                            | Description:                   |
| ---------- | ---------- | -------------------------------------------------------------------------------------- | ------------------------------ |
| Windows PC | *TLABPC01* | *172.21.30.10*                                                                         | -                              |
| Linux PC   | *TLABPC02* | *172.21.20.10*                                                                         | -                              |
| Router     | *TLABr08*  | **FA0/0.1:** *172.21.10.1*<br>**FA0/0.2:** *172.21.20.1*<br>**FA0/0.3:** *172.21.30.1* | MGMT<br>LIN<br>WIN             |
| Switch 1   | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                                 | TLABr08<br>TLABs06<br>TLABs21  |
| Switch 2   | *TLABs21*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -                                     | TLABPC01<br>TLABs05<br>TLABs06 |
| Switch 3   | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -                                     | TLABPC02<br>TLABs21<br>TLABs05 |
|            |            |                                                                                        |                                |


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

## Switches



```cisco
enable
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABsXX
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
conf t
vlan 10
name Management-VLAN
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
description Dummy-VLAN
switchport mode access
switchport access vlan 999
shutdown
end
```
---
```cisco
conf t
interface range Gi1/0/1 - 4
description Dummy-VLAN
switchport mode access
switchport access vlan 999
shutdown
end
```

### Switch 1
```cisco
conf t
interface fa1/0/1
description TLABr08
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
interface fa1/0/2
description TLABs06
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
interface fa1/0/3
description TLABs21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
interface vlan 10
ip address 172.21.10.101 255.255.255.0
no shutdown
exit
ip default-gateway 172.21.110.1
end
```
---

### Switch 2
```cisco
conf t
interface range Gi1/0/4 - 28
description Dummy-VLAN
switchport mode access
switchport access vlan 999
shutdown
end
```

```cisco
conf t
interface Gi1/0/1
description TLABPC01
switchport mode access
switchport access vlan 30
exit
interface Gi1/0/2
description TLABs05
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
exit
interface Gi1/0/3
description TLABs06
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
exit
interface vlan 10
ip address 172.21.10.102
no shutdown
exit
ip default-gateway 172.21.110.1 255.255.255.0
end
```

### Switch 3
```cisco
conf t
interface fa1/0/1
description TLABPC02
switchport mode access
switchport access vlan 20
exit
interface fa1/0/2
description TLABs21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
interface fa1/0/3
description TLABs05
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
interface vlan 10
ip address 172.21.10.103 255.255.255.0
no shutdown
exit
ip default-gateway 172.21.110.1
end
```
---
Zwischenkontrolle:
```cisco
show interface trunk
```
```cisco
show ip interface brief
```
