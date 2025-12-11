
# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. 





## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                                          | Description:                                                                   |
| ---------- | ---------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Windows PC | *TLABPC01* | *172.21.1.10*                                                        | -                                                                              |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                                        | -                                                                              |
| Router     | *TLABr08*  | **FA0/0:** *172.21.1.1*<br>**FA0/1:** *172.21.2.1*                   | TLABs05 (vlan10)<br>TLABs05 (vlan20)                                           |
| Switch     | TLABs05    | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>**FA1/0/4:** - | TLABPC01 (vlan10)<br>TLABPC02 (vlan20)<br>TLABs05 (vlan10)<br>TLABs05 (vlan20) |


---
---
# Übernommenne Konfig

Den Router sowie die beiden Clients haben wir gemäss [[Dokumentation 1]] aufgesetzt. genauere Infos dazu findet man dort.


---
---
# Switch Konfig