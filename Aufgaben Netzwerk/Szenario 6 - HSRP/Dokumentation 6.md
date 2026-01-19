










## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                                                            | Description:                               |
| ---------- | ---------- | -------------------------------------------------------------------------------------- | ------------------------------------------ |
| Windows PC | *TLABPC01* | *172.21.30.10*                                                                         | -                                          |
| Linux PC   | *TLABPC02* | *172.21.20.10*                                                                         | -                                          |
| Router 1   | *TLABr08*  | **FA0/0.1:** *172.21.10.1*<br>**FA0/0.2:** *172.21.20.1*<br>**FA0/0.3:** *172.21.30.1* | MGMT<br>LIN<br>WIN                         |
| Router 2   | *TLABr01*  |                                                                                        |                                            |
| Switch 1   | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                                 | TLABr08<br>TLABs06<br>TLABs21              |
| Switch 2   | *TLABs21*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -                                     | TLABr01 <br>TLABs05<br>TLABs06             |
| Switch 3   | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>**FA1/0/4:** -                   | TLABPC02<br>TLABs21<br>TLABs05<br>TLABPC01 |
|            |            |                                                                                        |                                            |

---
---

## Router Grundconfig

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
```cisco
line vty 0 4
password cisco
login
transport input telnet
end
```
---
---

## Router 1
Interface IP-Adressen
```cisco
conf t
interface fa0/0
no shutdown

interface fa 0/0.10
encapsulation dot1Q 10
ip address 172.21.10.2 255.255.255.0
standby 10 ip 172.21.10.1 
standby 10 priority 110 
standby 10 preempt
description MGMT
no shutdown

interface fa 0/0.20
encapsulation dot1Q 20
ip address 172.21.20.2 255.255.255.0
standby 20 ip 172.21.20.1 
standby 20 priority 110 
standby 20 preempt
description LIN

no shutdown
interface fa 0/0.30
encapsulation dot1Q 30
ip address 172.21.30.2 255.255.255.0
standby 30 ip 172.21.30.1 
standby 30 priority 110 
standby 30 preempt
description WIN
no shutdown
end
```

```cisco
conf t
interface fa 0/0.10
standby 10 timers msec 250 msec 750
exit

interface fa 0/0.20
standby 20 timers msec 250 msec 750
exit

interface fa 0/0.30
standby 30 timers msec 250 msec 750
end
```

---
---
## Router 2
Interface IP-Adressen
```cisco
conf t
interface fa 0/0.10
encapsulation dot1Q 10
ip address 172.21.10.3 255.255.255.0
standby 10 ip 172.21.10.1 
standby 10 priority 90
standby 10 preempt
description MGMT
no shutdown

interface fa 0/0.20
encapsulation dot1Q 20
ip address 172.21.20.3 255.255.255.0
standby 20 ip 172.21.20.1 
standby 20 priority 90
standby 20 preempt
description LIN
no shutdown

interface fa 0/0.30
encapsulation dot1Q 30
ip address 172.21.30.3 255.255.255.0
standby 30 ip 172.21.30.1 
standby 30 priority 90
standby 30 preempt
description WIN
no shutdown
end
```
---
---


## Switch
```cisco
interface fa1/0/4
description TLABPC01
switchport mode access
switchport access vlan 30
```
