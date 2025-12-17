# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. 


## Adresstabelle

| Bauteil:           | Name:      | IP-Adresse:                                                                       | Description:                                              |
| ------------------ | ---------- | --------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Windows PC (Noé)   | *TLABPC01* | *172.21.3.10*                                                                     | -                                                         |
| Linux PC (Noé)     | *TLABPC02* | *172.21.5.10*                                                                     | -                                                         |
| Router (Noé)       | *TLABr08*  | **FA0/0.1:** *172.21.3.1*<br>**FA0/0.2:** *172.21.5.1*<br>**FA0/1:** *172.21.1.1* | TLABs05 (VLAN 10)<br>TLABs05 (VLAN 20)<br>TLABr07         |
| Switch (Noé)       | *TLABs05*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr08 TRUNK<br>TLABPC01 (VLAN 10)<br>TLABPC02 (VLAN 20) |
| Windows PC (Kevin) | *TLABPC04* | *172.21.4.10*                                                                     | -                                                         |
| Linux PC (Kevin)   | *TLABPC03* | *172.21.6.10*                                                                     | -                                                         |
| Router (Kevin)     | *TLABr07*  | **FA0/0.1:** *172.21.4.1*<br>**FA0/0.2:** *172.21.6.1*<br>**FA0/1:** *172.21.2.1* | TLABs06 (VLAN 30)<br>TLABs06 (VLAN 40)<br>TLABr08         |
| Switch (Kevin)     | *TLABs06*  | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>                            | TLABr07 TRUNK<br>TLABPC04 (VLAN 30)<br>TLABPC03 (VLAN 40) |



---
---
# Switch Konfig

## Namen ändern

## Banner erstellen

## Passwörter setzen
## VTY-Access

## VLAN erstellen

## Interface

## Dummy VLAN erstellen



---
---
# Router Konfig

## Namen ändern

## Passwörter setzen

## Banner erstellen

## VTY-Access

## Interface erstellen



---
---



