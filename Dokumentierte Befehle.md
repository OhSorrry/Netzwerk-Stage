# Grundkonfiguration Router
```cisco
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABr07
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
banner motd #Finger weg!#
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
interface fa 0/0
ip address 172.21.1.1 255.255.255.0
no shutdown
end
```

# Grundkonfiguration Switch
```cisco
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABs06
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
banner motd #Finger weg!#
```

# Router on a Stick (Szenario 4)
## Switch
```cisco
conf t
vlan 30
name WIN-VLAN
exit
vlan 40
name LIN-VLAN
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
exit
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
---
Zwischenkontrolle:
```cisco
show vlan brief
```
oder
```cisco
show interface status
```
---
```cisco
conf t
interface fa1/0/1
description TLABr07 TRUNK
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 30,40
end
```
---
Zwischenkontrolle:
```cisco
show interface trunk
```
---
```cisco
conf t
interface fa1/0/2
description TLABPC04 (VLAN 30)
switchport mode access
switchport access vlan 30
exit
interface fa1/0/3
description TLABPC03 (VLAN 40)
switchport mode access
switchport access vlan 40
end
```
---
Zwischenkontrolle:
```cisco
show vlan brief
```
oder
```cisco
show interface status
```
---
```cisco
conf t
interface vlan30
ip address 172.21.4.100 255.255.255.0
no shutdown
exit
```
Zwischenkontrolle:
```cisco
show ip interface brief
```
---
## Router
```cisco
conf t
interface fa 0/0
no shutdown
exit
interface fa 0/0.10
no shutdown
description TLABs06 (VLAN 30) 
encapsulation dot1Q 30
ip address 172.21.4.1 255.255.255.0
exit
interface fa 0/0.20
no shutdown
description TLABs06 (VLAN 40) 
encapsulation dot1Q 40
ip address 172.21.6.1 255.255.255.0
end
```
Zwischenkontrolle:
```cisco
show ip interface brief
```
