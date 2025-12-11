
# Einleitung

## Auftrag
Bei dieser Aufgabe ging es darum ein Netzwerk Aufbauen das der unteren Zeichnung entspricht. 





## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                            | Description:                                              |
| ---------- | ---------- | ------------------------------------------------------ | --------------------------------------------------------- |
| Windows PC | *TLABPC01* | *172.21.1.10*                                          | -                                                         |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                          | -                                                         |
| Router     | *TLABr08*  | **FA0/0:** *172.21.1.1*<br>                            | TLABs05                                                   |
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

![](Screenshots/Pasted%20image%2020251211164125.png)

![](Screenshots/Pasted%20image%2020251211164136.png)




---
## Dummy VLAN erstellen

![](Screenshots/Pasted%20image%2020251211140923.png)

![](Screenshots/Pasted%20image%2020251211141013.png)

![](Screenshots/Pasted%20image%2020251211141046.png)

![](Screenshots/Pasted%20image%2020251211141113.png)

![](Screenshots/Pasted%20image%2020251211141138.png)


![](Screenshots/Pasted%20image%2020251211141332.png)


---
## VTY-Access


![](Screenshots/Pasted%20image%2020251211163135.png)







