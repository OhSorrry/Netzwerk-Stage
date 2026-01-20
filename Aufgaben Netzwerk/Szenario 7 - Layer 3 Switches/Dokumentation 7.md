



| Bauteil:           | Name:      | IP-Adresse:                                            | Virtuelle IP:   | Description:                   |
| ------------------ | ---------- | ------------------------------------------------------ | --------------- | ------------------------------ |
| Windows PC         | *TLABPC01* | *172.21.30.10*                                         | -               | -                              |
| Linux PC           | *TLABPC02* | *172.21.20.10*                                         | -               | -                              |
| Switch 1 (Layer 2) | *TLABs21*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | -<br>-<br>-     | TLABPC01<br>TLABs05<br>TLABs06 |
| Switch 2 (Layer 2) | *TLABs022* | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -     | -<br>-<br>-     | TLABPC02<br>TLABs06<br>TLABs05 |
| Switch 3 (Layer 3) | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | -<br>-<br>-<br> | TLABs06<br>TLABs21<br>TLABs22  |
| Switch 4 (Layer 3) | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -     | -<br>-<br>-<br> | TLABs05<br>TLABs22<br>TLABs21  |

---
---

Grundkonfig:
```cisco
conf t
no ip domain-lookup
```
---
```cisco
hostname TLABrXX
```
---
```cisco
enable password cisco
line console 0
password cisco
login
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
---

## Switch 1

```cisco
hostname TLABs21
```

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
no shutdown
exit

interface Gi1/0/2
description TLABs05
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface Gi1/0/3
description TLABs06
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface vlan 10
ip address 172.21.10.101 255.255.255.0
no shutdown
exit
ip default-gateway 172.21.10.103
line vty 0 15 
password cisco
transport input telnet
end
```
```cisco
conf t
spanning-tree mode rapid-pvst
interface gi1/0/1
spanning-tree portfast
end
```


---
---
## Switch 2

```cisco
enable
conf t
hostname TLABs22
end
```

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
description TLABPC02
switchport mode access
switchport access vlan 20
no shutdown
exit

interface Gi1/0/2
description TLABs06
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface Gi1/0/3
description TLABs05
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface vlan 10
ip address 172.21.10.102 255.255.255.0
no shutdown
exit
ip default-gateway 172.21.10.103
line vty 0 15 
password cisco
transport input telnet
end
```
```cisco
conf t
spanning-tree mode rapid-pvst
interface gi1/0/1
spanning-tree portfast
end
```

---
---

## Switch 3

```cisco
enable
conf t
hostname TLABs05
end
```

```cisco
conf t
interface range fa1/0/4 - 48
description Dummy-VLAN
switchport mode access
switchport access vlan 999
shutdown
end
```

```cisco
conf t
interface range Gi1/0/1 - 4
description Dummy-VLAN
switchport mode access
switchport access vlan 999
shutdown
end
```

```cisco
conf t

ip routing

interface vlan 10
ip address 172.21.10.103 255.255.255.0
no shutdown
interface vlan 20
ip address 172.21.20.1 255.255.255.0
no shutdown
interface vlan 30
ip address 172.21.30.1 255.255.255.0
no shutdown



interface fa1/0/1
description TLABs06
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface fa1/0/2
description TLABs21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface fa1/0/3
description TLABs22
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface vlan 10
ip address 172.21.10.103 255.255.255.0
no shutdown
exit

end
```


---
---

## Switch 4