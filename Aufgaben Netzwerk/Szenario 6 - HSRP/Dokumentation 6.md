










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



