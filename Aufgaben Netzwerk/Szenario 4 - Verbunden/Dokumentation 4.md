# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. 


## Adresstabelle

| Bauteil:           | Name:      | IP-Adresse:                                                                       | Description:                                              |
| ------------------ | ---------- | --------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Windows PC (Noé)   | *TLABPC01* | *172.21.1.10*                                                                     | -                                                         |
| Linux PC (Noé)     | *TLABPC02* | *172.21.2.10*                                                                     | -                                                         |
| Router (Noé)       | *TLABr08*  | **FA0/0.1:** *172.21.3.1*<br>**FA0/0.2:** *172.21.5.1*<br>**FA0/1:** *172.21.1.1* | TLABs05 (VLAN 10)<br>TLABs05 (VLAN 20)<br>TLABr07         |
| Switch (Noé)       | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr08 TRUNK<br>TLABPC01 (VLAN 10)<br>TLABPC02 (VLAN 20) |
| Windows PC (Kevin) | *TLABPC04* | *172.21.3.10*                                                                     | -                                                         |
| Linux PC (Kevin)   | *TLABPC03* | *172.21.4.10*                                                                     | -                                                         |
| Router (Kevin)     | *TLABr07*  | **FA0/0.1:** *172.21.4.1*<br>**FA0/0.2:** *172.21.6.1*<br>**FA0/1:** *172.21.2.1* | TLABs06 (VLAN 30)<br>TLABs06 (VLAN 40)<br>TLABr08         |
| Switch (Kevin)     | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr07 TRUNK<br>TLABPC04 (VLAN 30)<br>TLABPC03 (VLAN 40) |



---
---
# Switch Konfig


## Namen ändern
![](Screenshots/Pasted%20image%2020251215145537.png)


---
## Banner erstellen
![](Screenshots/Pasted%20image%2020251215145623.png)

**Banner Vorlage:**
```cisco
+-------------------------+
| Finger weg!             |
+-------------------------+
```


---
## VTY-Access
![](Screenshots/Pasted%20image%2020251215145655.png)


---
## VLAN erstellen
![](Screenshots/Pasted%20image%2020251215150857.png)


---
## Interface

![](Screenshots/Pasted%20image%2020251215150301.png)
![](Screenshots/Pasted%20image%2020251215150313.png)
![](Screenshots/Pasted%20image%2020251215150330.png)
![](Screenshots/Pasted%20image%2020251215150344.png)
![](Screenshots/Pasted%20image%2020251215150356.png)






---
## Dummy VLAN erstellen

![](Screenshots/Pasted%20image%2020251215151038.png)





---
