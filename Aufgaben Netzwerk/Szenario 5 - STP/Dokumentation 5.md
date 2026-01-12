

Management IP auf Switches in anderem Subnetz als Pcs
3 Subnetze- Linux, Windows, Switch
Spanning Tree Protocol anschauen
Spanning Tree Route über S1 ()  




---
---
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


