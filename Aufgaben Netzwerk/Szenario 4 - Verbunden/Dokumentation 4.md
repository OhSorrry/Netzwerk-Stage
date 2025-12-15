# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. 


## Adresstabelle

| Bauteil:           | Name:      | IP-Adresse:                                            | Description:                                              |
| ------------------ | ---------- | ------------------------------------------------------ | --------------------------------------------------------- |
| Windows PC (Noé)   | *TLABPC01* | *172.21.1.10*                                          | -                                                         |
| Linux PC (Noé)     | *TLABPC02* | *172.21.2.10*                                          | -                                                         |
| Router (Noé)       | *TLABr08*  | **FA0/0.1:** *172.21.1.1*<br>**FA0/0.2:** *172.21.3.1* | TLABs05 (VLAN 10)<br>TLABs05 (VLAN 20)                    |
| Switch (Noé)       | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> | TLABr08 TRUNK<br>TLABPC01 (VLAN 10)<br>TLABPC02 (VLAN 20) |
| Windows PC (Kevin) | *TLABPC03* |                                                        |                                                           |
| Linux PC (Kevin)   | *TLABPC04* |                                                        |                                                           |
| Router (Kevin)     | *TLABr07*  | **FA0/0.1:** *172.21.2.1*<br>**FA0/0.2:** *172.21.4.1* |                                                           |
| Switch (Kevin)     | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br> |                                                           |



---
---
# Switch Konfig
