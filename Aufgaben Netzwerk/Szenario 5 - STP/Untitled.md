outer

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
encapsulation dot1Q 10
ip address 172.21.10.1 255.255.255.0
description MGMT
no shutdown
interface fa 0/0.20
encapsulation dot1Q 20
ip address 172.21.20.1 255.255.255.0
description LIN
no shutdown
interface fa 0/0.30
encapsulation dot1Q 30
ip address 172.21.30.1 255.255.255.0
description WIN
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
ip default-gateway 172.21.10.1
line vty 0 4 
password cisco
end
```
```cisco
spanning-tree mode rapid-pvst
interface fa1/0/1
spanning-tree portfast trunk

spanning-tree vlan 1 root primary
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
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
ip address 172.21.10.103 255.255.255.0
no shutdown
exit
ip default-gateway 172.21.10.1
line vty 0 4 
password cisco
end
```
```cisco
spanning-tree mode rapid-pvst
interface gi1/0/1
spanning-tree portfast
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
ip default-gateway 172.21.10.1
line vty 0 4 
password cisco
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
