



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
ip default-gateway 172.21.10.1
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
ip default-gateway 172.21.10.1
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

interface Vlan 10 
ip address 172.21.10.103 255.255.255.0 
standby 10 ip 172.21.10.1 
standby 10 priority 110 
standby 10 preempt 
standby 10 timers 1 3
standby 20 preempt delay minimum 30
no shutdown

interface Vlan 20 
ip address 172.21.20.103 255.255.255.0 
standby 20 ip 172.21.20.1 
standby 20 priority 110 
standby 20 preempt 
standby 20 timers 1 3
standby 20 preempt delay minimum 30
no shutdown

interface Vlan 30 
ip address 172.21.30.103 255.255.255.0 
standby 30 ip 172.21.30.1 
standby 30 priority 110 
standby 30 preempt 
standby 30 timers 1 3
standby 20 preempt delay minimum 30
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

spanning-tree vlan 1 root primary
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary

end
```


---
---

## Switch 4

```cisco
enable
conf t
hostname TLABs06
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

interface Vlan 10 
ip address 172.21.10.104 255.255.255.0 
standby 10 ip 172.21.10.1 
standby 10 priority 90 
standby 10 preempt 
standby 10 timers 1 3
interface Vlan 20 
ip address 172.21.20.104 255.255.255.0 
standby 20 ip 172.21.20.1 
standby 20 priority 90 
standby 20 preempt 
standby 20 timers 1 3
interface Vlan 30 
ip address 172.21.30.104 255.255.255.0 
standby 30 ip 172.21.30.1 
standby 30 priority 90 
standby 30 preempt 
standby 30 timers 1 3

interface fa1/0/1
description TLABs05
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface fa1/0/2
description TLABs22
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

interface fa1/0/3
description TLABs21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
exit

line vty 0 15 
password cisco
transport input telnet
end
```



---
---
# Szenario 9 - Routing/STP
## Switch 3 (s05)
```cisco
conf t
ip routing

router eigrp 100
no auto-summary
network 172.21.10.0 0.0.0.255
network 172.21.20.0 0.0.0.255
network 172.21.30.0 0.0.0.255
network 172.21.200.0 0.0.0.255
network 172.21.201.0 0.0.0.255
network 172.21.202.0 0.0.0.255
network 172.21.203.0 0.0.0.255
network 172.21.204.0 0.0.0.255 
passive-interface default
no passive-interface fa1/0/1
no passive-interface fa1/0/4
end
```

# Switch 4 (s06)
```cisco
conf t
ip routing

router eigrp 100
no auto-summary
network 172.21.10.0 0.0.0.255
network 172.21.20.0 0.0.0.255
network 172.21.30.0 0.0.0.255
network 172.21.200.0 0.0.0.255
network 172.21.201.0 0.0.0.255
network 172.21.202.0 0.0.0.255
network 172.21.203.0 0.0.0.255
network 172.21.204.0 0.0.0.255 

passive-interface default
no passive-interface fa1/0/1
no passive-interface fa1/0/4
end
```


# Switch 5 (s25)


```cisco
enable
conf t
hostname TLABs25
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
ip routing

interface gi1/0/1
no shutdown
no switchport
description TLABs38 Routed
ip address 172.21.204.2 255.255.255.0
exit

interface gi1/0/2
no shutdown
no switchport
description TLABs05 Routed
ip address 172.21.200.2 255.255.255.0
exit

interface gi1/0/3
no shutdown
no switchport
description TLABs12 Routed
ip address 172.21.201.2 255.255.255.0
exit

router eigrp 100
no auto-summary
network 172.21.200.0 0.0.0.255
network 172.21.201.0 0.0.0.255
network 172.21.202.0 0.0.0.255
network 172.21.203.0 0.0.0.255
network 172.21.204.0 0.0.0.255 

passive-interface default
no passive-interface gi1/0/1
no passive-interface gi1/0/2
no passive-interface gi1/0/3
end
```



