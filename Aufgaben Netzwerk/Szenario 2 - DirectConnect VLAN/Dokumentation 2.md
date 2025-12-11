
# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. es gibt zwei Clients die sich über einen Switch und einen Router pingen können und zwar indem der eine Client auf den Switch zugreift, von dort aus auf den Router, dann wieder zurück auf den Switch und schlussendlich auf den anderen Client. Das Ziel ist das wenn man die Kabel von Switch zu Router an dem jeweils anderen Port anschliesst, der Ping nicht mehr funktionieren sollte. Unsere erste Idee war direkt das man dies mit einem VLAN lösen könnte, da auch der Name der Aufgabe so war. Wir haben dann die Konfigurationen wie in der folgenden Dokumentation vorgenommen. 





## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                                          | Description:                                                                   |
| ---------- | ---------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Windows PC | *TLABPC01* | *172.21.1.10*                                                        | -                                                                              |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                                        | -                                                                              |
| Router     | *TLABr08*  | **FA0/0:** *172.21.1.1*<br>**FA0/1:** *172.21.2.1*                   | TLABs05 (vlan10)<br>TLABs05 (vlan20)                                           |
| Switch     | TLABs05    | **FA1/0/1:** -<br>**FA1/0/2:** -<br>**FA1/0/3:** -<br>**FA1/0/4:** - | TLABPC01 (vlan10)<br>TLABPC02 (vlan20)<br>TLABs05 (vlan10)<br>TLABs05 (vlan20) |


---
---
# Hauptteil

## Router Konfig
Den Router haben wir gemäss Aufgabe 1 - DirectConnect aufgesetzt. genauere Infos dazu finde 


## Name ändern
asd
![[Pasted image 20251208132814.png]]

asd
![[Pasted image 20251208132833.png]]

 ---
## Vlan erstellen
asd
![[Pasted image 20251208132814.png]]

asd
 


---
## Interface erstellen

![[Pasted image 20251208155053.png]]

![[Pasted image 20251208155113.png]]



![[Pasted image 20251208160454.png]]


---
## Interface Description erstellen

![[Pasted image 20251208155053.png]]


![[Pasted image 20251208155135.png]]
