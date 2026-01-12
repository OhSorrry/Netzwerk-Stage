
## Auftrag

In dieser Aufgabe ist der genaue Ablauf eines Pings beschrieben. Darin sind alle Schritte enthalten die passieren wenn der PC **TLAPC04** den PC **TLABPC02** pingt. Dabei wird von dem folgenden Netzwerk ausgegangen:
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


# Ablauf Ping

**Ausgangslage**
- der Ping geht von **TLABPC04 (VLAN 30)** auf **TLABPC02 (VLAN 20)**
- Auf den Routern ist OSPF konfiguriert aber sie sind noch nicht Nachbarn
- Alle Geräte kennen sich bis jetzt nicht 

---

**1. TLABPC04 prüft Ziel-IP**

- TLABPC04 will 172.21.5.10 pingen
- Prüft: Liegt Ziel-IP im eigenen Subnetz 172.21.4.0/24?  
    Nein. Darum muss das Paket an den Default Gateway (172.21.4.1)

---

**2. ARP für Gateway**

- TLABPC04 kennt die MAC vom Gateway nicht
- Sendet ARP-Request an den Switch mittels Broadcast: Wer kennt die IP 172.21.4.1?
- Switch empfängt Broadcast und leitet ihn an alle Ports im VLAN 30 weiter
- TLABr07 antwortet mit ARP-Reply: Ich bin 172.21.4.1 und meine MAC ist XY.
- Dies wird über den Switch wieder an den TLABPC04 geleitet
- der PC Speichert die MAC dann in seinem ARP Cache

---

**3. PC04 sendet Ping**

- TLABPC04 erstellt ein Ping-Paket:

| Info:          | Adresse:      |
| -------------- | ------------- |
| **Ziel IP:**   | *172.21.5.10* |
| **Quell IP:**  | *172.21.4.10* |
| **Ziel MAC:**  | (TLABr07)     |
| **Quell MAC:** | (TLABPC04)    |
- TLABPC04 schickt Paket an Switch
- Switch leitet Paket an TLABr08 weiter, kennt ihn durch die ARP bereits und hat die Adresse in der MAC-Tabelle 

---

**4. TLABr07 schickt Paket an TLABr08**

- TLABr07 wählt Next-Hop und findet 172.21.1.1 - TLABr08
- Prüft MAC für 172.21.1.1
    - Da MAC unbekannt - ARP-Request auf Gi0/0/1.
    - TLABr08 antwortet mit MAC.
- TLABr07 sendet ICMP-Paket an TLABr08

---

**5. TLABr08 empfängt Paket**

- TLABr08 prüft Routing-Tabelle:
- Ziel-IP = 172.21.5.10 - liegt in VLAN 20.
	- Kennt VLAN 20 da es direkt angeschlossen ist
	
- Prüft MAC für TLABPC02:
    - Da MAC unbekannt - ARP-Request ins VLAN 20.
    - TLABPC02 antwortet mit MAC.

---

**6. Weiterleitung über Switch**

- TLABr08 sendet Paket an den Switch
- Switch sendet Paket an TLABPC02 - kennt in bereits durch ARP

---

**7. TLABPC02 empfängt Ping**

- Ping ist jetzt am Ziel - Es folgt der Rückweg mit einer Reply

---

**8. TLABPC02 prüft Ziel-IP**

- Ziel: 172.21.4.10 - TLABPC04.
- Prüft: Liegt im eigenen Subnetz 172.21.5.0/24?  
    Nein, muss an Default Gateway gesendet werden - 172.21.5.1

---

**9. Paket erstellen**

- TLABPC02 kennt MAC von 172.21.5.1 aus der ersten ARP bereits
- Erstellt ein Paket und schickt dieses an Switch:

| Info:          | Adresse:      |
| -------------- | ------------- |
| **Ziel IP:**   | *172.21.4.10* |
| **Quell IP:**  | *172.21.5.10* |
| **Ziel MAC:**  | (TLABr08)     |
| **Quell MAC:** | (TLABPC02)    |

---

**10. Switch leitet Paket weiter**

- Switch kennt MAC von TLABr08 - leitet direkt weiter

---

**11. TLABr08 empfängt Paket**

- Da dieser schon den TLABr07 kennt schickt er das Paket direkt weiter

---

**12. TLABr07 empfängt Paket**

- Zielnetz 172.21.4.0/24 das Netz ist direkt verbunden (VLAN 30).
- Adressen und Wege vom ersten Ping bereits bekannt
- Sendet Paket weiter an Switch 

---

**13. TLABPC04 empfängt Ping**

- Ping ist nun wieder zurück angekommen und wird als erfolgreich in der Konsole angezeigt

---
