
# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. Diese Aufgabe war ähnlich wie das Szenario 2, jedoch gibt es zwischen Switch und Router nur noch ein Kabel. So muss man nun eine Konfiguration Namens "Router in a Stick" konfigurieren. hierbei wird das einzelne Kabel für zwei separate Vlans verwendet. Dies kann man über einen Trunk lösen, dieser muss auf dem Switch sowie auf dem Router Konfiguriert werden. Die zwei Clients werden in ein jeweils anderes Vlan integriert, das auch auf dem Switch ist und mittels des Trunk Ports konfiguriert wird. auf dem Router müssen in dem angeschlossenen Port zwei Sub-Interfaces erstellt werden über die die Verbindung läuft. 

Gestellte Anforderungen an das System:
- Access ohne Konsolenkabel (VTY)
- Alle ungenutzten Ports ausschalten (Dummy VLAN)
- Alle Ports haben Beschreibungen
- Alle Anforderungen der Vorherigen Aufgabe müssen ebenfalls umgesetzte werden


![](Screenshots/Pasted%20image%2020251215084742.jpg)



## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                            | Description:                                              |
| ---------- | ---------- | ------------------------------------------------------ | --------------------------------------------------------- |
| Windows PC | *TLABPC01* | *172.21.1.10*                                          | -                                                         |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                          | -                                                         |
| Router     | *TLABr08*  | **FA0/0.1:** *172.21.1.1*<br>**FA0/0.2:** *172.21.2.1* | TLABs05 (VLAN 10)<br>TLABs05 (VLAN 20)                    |
| Switch     | TLABs05    | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | TLABr08 TRUNK<br>TLABPC01 (VLAN 10)<br>TLABPC02 (VLAN 20) |



---
---
# Switch Konfig

## Interface


![](Screenshots/Pasted%20image%2020251211163530.png)

![](Screenshots/Pasted%20image%2020251211163404.png)



![](Screenshots/Pasted%20image%2020251211163940.png)

![](Screenshots/Pasted%20image%2020251211163950.png)

![](Screenshots/Pasted%20image%2020251211164001.png)


---
## VLAN's erstellen
Im Konfigurationsmodus mittels `vlan X` ein neues Vlan erstellen:
![](Screenshots/Pasted%20image%2020251211164125.png)

Mittels `name X` einen Namen setzen:
![](Screenshots/Pasted%20image%2020251211164136.png)



---
## Dummy VLAN erstellen
Mittels `vlan X` ein neues Vlan erstellen und den Namen auf "dummy" setzen:
![](Screenshots/Pasted%20image%2020251211140923.png)

Danach mittels `interface range faX/X/X - X` die nicht genutzten Ports auswählen:
![](Screenshots/Pasted%20image%2020251211141013.png)

den Port-mode auf "access" setzen:
![](Screenshots/Pasted%20image%2020251211141046.png)

die Ports mittels `switchport access vlan X` in das dummy-Vlan:
![](Screenshots/Pasted%20image%2020251211141113.png)

Die Ports mittels `shutdown` ausschalten:
![](Screenshots/Pasted%20image%2020251211141138.png)

die Gigabit-Ports müssen auch noch in das Dummy Vlan integriert werden:
![](Screenshots/Pasted%20image%2020251211141332.png)


---
## VTY-Access
mittels `interface vlan10` in das interface "vlan10" wechseln:
![](Screenshots/Pasted%20image%2020251211172445.png)

mittels `ip address x.x.x.x x.x.x.x` eine neue IP setzen:
![](Screenshots/Pasted%20image%2020251211172501.png)

Den default-gateway mittels `ip default gateway x.x.x.x` auf den Router setzen:
![](Screenshots/Pasted%20image%2020251211172636.png)


---
---
# Router Konfiguration

## Interface
via `interface faX/X` in das gewünschte interface wechseln, mittels`no ip address` die IP-löschen und `no shutdown aktivierren`. 
![](Screenshots/Pasted%20image%2020251211164933.png)

nun mittels via `interface faX/X.X0` das gewünschte sub-interface anlegen:
![](Screenshots/Pasted%20image%2020251211165049.png)

Das sub-interface dem gewünschten Vlan zuweisen:
![](Screenshots/Pasted%20image%2020251211165103.png)

mittels `ip address x.x.x.x x.x.x.x` eine neue IP setzen:
![](Screenshots/Pasted%20image%2020251211165152.png)

Das ganze noch für das zweite sub-interface erstellen:
![](Screenshots/Pasted%20image%2020251211165249.png)


---
![](Screenshots/Pasted%20image%2020251215095421.png)