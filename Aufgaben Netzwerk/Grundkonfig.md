# Grundkonfiguration Router

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
interface fa 0/0
ip address 172.21.1.1 255.255.255.0
no shutdown
end
```

---
---

# Grundkonfiguration Switch
```cisco
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABs05
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
---

# Router on a Stick (Szenario 4)
## Switch
```cisco
conf t
vlan 10
name WIN-VLAN
exit
vlan 20
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
description TLABr08 TRUNK
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20
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
description TLABPC01 (VLAN 10)
switchport mode access
switchport access vlan 10
exit
interface fa1/0/3
description TLABPC02 (VLAN 20)
switchport mode access
switchport access vlan 20
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
interface vlan10
ip address 172.21.1.100 255.255.255.0
no shutdown
exit
```
Zwischenkontrolle:
```cisco
show ip interface brief
```

---
---

## Router
```cisco
conf t
interface fa 0/0
no shutdown
exit
interface fa 0/0.10
no shutdown
description TLABs05 (VLAN 10) 
encapsulation dot1Q 10
ip address 172.21.3.1 255.255.255.0
exit
interface fa 0/0.20
no shutdown
description TLABs05 (VLAN 20) 
encapsulation dot1Q 20
ip address 172.21.5.1 255.255.255.0
end
```
Zwischenkontrolle:
```cisco
show ip interface brief
```


---
---

# STP (Szenario 5)
## Router

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

```cisco
conf t
interface fa 0/0.10
interface description MGMT
no shutdown
exit
interface fa 0/0.20
interface description LIN
no shutdown
exit
interface fa 0/0.30
Interface description WIN
no shutdown
end
```


