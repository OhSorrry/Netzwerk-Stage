



| Bauteil:           | Name:      | IP-Adresse:                                            | Virtuelle IP:   | Description: |
| ------------------ | ---------- | ------------------------------------------------------ | --------------- | ------------ |
| Windows PC         | *TLABPC01* | *172.21.30.10*                                         | -               | -            |
| Linux PC           | *TLABPC02* | *172.21.20.10*                                         | -               | -            |
| Switch 1 (Layer 2) | *TLABs21*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | -<br>-<br>-     | TLABPC01<br> |
| Switch 2 (Layer 2) | *TLABs022* | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -     | -<br>-<br>-     | TLABPC02     |
| Switch 3 (Layer 3) | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | -<br>-<br>-<br> |              |
| Switch 4 (Layer 3) | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -     | -<br>-<br>-<br> |              |

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
Grundkonfig:
```cisco
conf t
no ip domain-lookup
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
interface gi1/0/1
spanning-tree portfast
```

