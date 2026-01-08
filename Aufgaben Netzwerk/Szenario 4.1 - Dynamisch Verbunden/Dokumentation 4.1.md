# Einleitung

## Auftrag
In dieser Aufgabe mussten wir das gesamte Netzwerk vom Szenario 4 ins Cisco Packet Tracker übertragen. Da die beide Router mit einem statischen Route verbunden sind, und das nicht sehr praktisch ist, gab uns Marcel den Auftrag, dass wir ein dynamisches Routing Protokoll verwenden sollten. Nach einer kurzen Recherche haben wir herausgefunden welche 2 Protokolle wir verwenden. Ich verwende das OSPF (Open Shortest Path First) und Kevin das EIGRP (Enhanced Interior Gateway Routing Protocol).

**Unsere Zeichnung aus der letzten Aufgabe:**
![](Screenshots/Pasted%20image%2020251217120006.jpg)


**Modell in Cisco:**
![](Screenshots/Pasted%20image%2020260107154126.png)


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
---

## Konfiguration Router

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

## Kofinguraion Switch

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
```cisco
conf t
interface vlan10
ip address 172.21.1.100 255.255.255.0
no shutdown
exit
```

---
---

## OSPF einrichten

Auf ospf:
![](Screenshots/Pasted%20image%2020260107150954.png)

Adressen der unterliegenden Vlans zufügen:
![](Screenshots/Pasted%20image%2020260107151007.png)

auf tlabr07:
![](Screenshots/Pasted%20image%2020260107151148.png)




tlabr07:
![](Screenshots/Pasted%20image%2020260108093913.png)

![](Screenshots/Pasted%20image%2020260108093926.png)

![](Screenshots/Pasted%20image%2020260108133540.png)



auf tlabr08:
![](Screenshots/Pasted%20image%2020260108094056.png)

![](Screenshots/Pasted%20image%2020260108094108.png)

![](Screenshots/Pasted%20image%2020260108133927.png)




config löschen:
![](Screenshots/Pasted%20image%2020260108094008.png)

---
---
# OSPF

##  Wie werden OSPF-Nachbarn gebildet?

**Grundvoraussetzungen**

- Gleiche **Area-ID** 
- Gleiche **Subnetzmaske** auf der Verbindung.
- Gleiche **Hello- und Dead-Timer**.
- Gleiche **Authentication** (habe ich nicht konfiguriert).
- Interfaces müssen beide **up** sein.

**Aufbau**

- OSPF sendet **Hello-Pakete** auf jedem Interface, das im network-Befehl enthalten ist.
- Wenn zwei Router die Bedingungen erfüllen, werden sie **Neighbors**.
- Danach tauschen sie **LSAs (Link-State Advertisements)** aus, um die Topologie zu kennenlernen.


**Wie werden Routing-Infos ausgetauscht?**

- OSPF baut eine **Link-State-Datenbank (LSDB)** auf.
- Jeder Router kennt die gesamte Topologie der Area.
- Mit dem **Dijkstra-Algorithmus** berechnet OSPF den kürzesten Pfad.
- Updates sind **inkrementell** (nur bei Änderungen).


**Kommunikation**

- Hello-Pakete - Nachbarschaft.
- **Database Description (DBD)** - Überblick über LSDB.
- **LSA Updates** - Details über Links.
- Multicast-Adressen:
    - 224.0.0.5 → Alle OSPF-Router.
    - 224.0.0.6 → Designated Router (DR) und Backup DR.

---
## Befehle zum Prüfen

`show ip ospf neighbor` --> zeigt Nachbarn und Status.
`show ip route` --> Routen mit **O** (OSPF).
`debug ip ospf adj` --> zeigt Nachbarschaftsbildung.




