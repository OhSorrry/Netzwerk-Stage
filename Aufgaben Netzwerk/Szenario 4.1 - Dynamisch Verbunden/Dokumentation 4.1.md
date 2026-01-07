# Einleitung

## Auftrag
In dieser Aufgabe mussten wir das gesamte Netzwerk vom Szenario 4 ins Cisco Packet Tracker übertragen. Da die beide Router mit einem statischen Route verbunden sind, und das nicht sehr praktisch ist, gab uns Marcel den Auftrag, dass wir ein dynamisches Routing Protokoll verwenden sollten. Nach einer kurzen Recherche haben wir herausgefunden welche 2 Protokolle wir verwenden. Noé verwendet das OSPF (Open Shortest Path First) und ich das EIGRP (Enhanced Interior Gateway Routing Protocol).
Zur Erinnerung nochmals:

![](Screenshots/Pasted%20image%2020251217120006.jpg)

![](Screenshots/Pasted%20image%2020260107133048.png)


## Adresstabelle

| Bauteil:           | Name:      | IP-Adresse:                                                                       | Description:                                              |
| ------------------ | ---------- | --------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Windows PC (Noé)   | *TLABPC01* | *172.21.3.10*                                                                     | -                                                         |
| Linux PC (Noé)     | *TLABPC02* | *172.21.5.10*                                                                     | -                                                         |
| Router (Noé)       | *TLABr08*  | **FA0/0.1:** *172.21.3.1*<br>**FA0/0.2:** *172.21.5.1*<br>**FA0/1:** *172.21.1.1* | TLABs05 (VLAN 10)<br>TLABs05 (VLAN 20)<br>TLABr07         |
| Switch (Noé)       | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr08 TRUNK<br>TLABPC01 (VLAN 10)<br>TLABPC02 (VLAN 20) |
| Windows PC (Kevin) | *TLABPC04* | *172.21.4.10*                                                                     | -                                                         |
| Linux PC (Kevin)   | *TLABPC03* | *172.21.6.10*                                                                     | -                                                         |
| Router (Kevin)     | *TLABr07*  | **FA0/0.1:** *172.21.4.1*<br>**FA0/0.2:** *172.21.6.1*<br>**FA0/1:** *172.21.1.2* | TLABs06 (VLAN 30)<br>TLABs06 (VLAN 40)<br>TLABr08         |
| Switch (Kevin)     | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr07 TRUNK<br>TLABPC04 (VLAN 30)<br>TLABPC03 (VLAN 40) |


---

