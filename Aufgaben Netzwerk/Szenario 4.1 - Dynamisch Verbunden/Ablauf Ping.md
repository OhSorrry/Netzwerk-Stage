
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
- TLABr08 antwortet mit ARP-Reply: Ich bin 172.21.4.1 und meine MAC ist XY.
- Dies wird über den Switch wieder an den TLABPC04 geleitet
- der PC Speichert die MAC dann in seinem ARP Cache

---

 **3. PC04 sendet ICMP Ping**

- TLABPC04 erstellt ein Ping-Paket:

| Info:          | Adresse:      |
| -------------- | ------------- |
| **Ziel IP:**   | *172.21.5.10* |
| **Quell IP:**  | *172.21.4.10* |
| **Ziel MAC:**  | (TLABr08)     |
| **Quell MAC:** | (TLABPC04)    |
- TLABPC04 schickt Paket an Switch
- Switch leitet Paket an TLABr08 weiter, kennt ihn durch die ARP bereits und hat die Adresse in der MAC-Tabelle 
---

 **4. Router 2 empfängt Paket**

- Router 2 prüft Routing-Tabelle:
    - Kennt VLAN 30 (direkt verbunden).
    - Kennt VLAN 40 (direkt verbunden).
    - **Kennt VLAN 20 noch nicht**, weil EIGRP-Nachbarschaft fehlt.
- Router 2 sendet **EIGRP Hello** an 224.0.0.10 auf Gi0/0/1.
- Router 1 empfängt Hello, antwortet → **Nachbarschaft entsteht**.
- Beide tauschen Routing-Informationen (Update-Pakete).
- Router 2 lernt: „[172.21.5.0/24](http://172.21.5.0/24) erreichbar über Router 1.“

---

 **Schritt 5: Router 2 forwardet Paket an Router 1**

- Router 2 wählt Next-Hop = 172.21.1.1 (Router 1).
- Prüft ARP für [172.21.1.1](http://172.21.1.1/):
    - Falls unbekannt → ARP-Request auf Gi0/0/1.
    - Router 1 antwortet mit MAC.
- Router 2 sendet ICMP-Paket an Router 1.

---

 **Schritt 6: Router 1 empfängt Paket**

- Router 1 prüft Routing-Tabelle:
    - Kennt VLAN 20 (direkt verbunden).
- Ziel-IP = 172.21.5.x → liegt in VLAN 20.
- Prüft ARP für PC02:
    - Falls unbekannt → ARP-Request ins VLAN 20.
    - PC02 antwortet mit MAC.
- Router 1 sendet ICMP-Paket an PC02.

---

 **Schritt 7: PC02 empfängt Ping**

- PC02 antwortet mit ICMP Echo Reply.
- Rückweg identisch, aber ARP und Routing sind jetzt gecached → schneller.

---

 **Wichtige Punkte:**

- **ARP** wird auf jedem Hop benötigt, wenn MAC unbekannt.
- **EIGRP Hello** und Routing-Update passieren **beim ersten Paket**, weil vorher keine Nachbarschaft bestand.
- Switches lernen MAC-Adressen während des Prozesses.
- Nach dem ersten Ping ist alles im Cache → Folge-Pings sind sofort da.






**Ablauf: PC02 (VLAN 20)** **→** **PC04 (VLAN 30)**

**Ausgangslage**

- PC02 kennt seinen Gateway (172.21.5.1) MAC-Adresse (vom ersten Ping).
- Switch kennt MAC von PC02 und Router 1.
- Router 1 kennt Router 2 (EIGRP-Nachbarschaft steht).
- Router 2 kennt VLAN 30 und VLAN 40 Netze.
- PC04 kennt seinen Gateway (172.21.4.1) MAC-Adresse.

---

 **Schritt-für-Schritt:**

**1. PC02 prüft Ziel-IP**

- Ziel: 172.21.4.x (PC04).
- Prüft: Liegt im eigenen Subnetz ([172.21.5.0/24](http://172.21.5.0/24))?  
    → Nein → sendet an Default Gateway (172.21.5.1).

**2. ARP für Gateway?**

- PC02 kennt MAC von 172.21.5.1 (vom ersten Ping) → **kein ARP nötig**.
- Erstellt ICMP Echo Request → Ethernet-Frame an Router 1.

**3. Switch leitet Frame**

- Switch kennt MAC von Router 1 → leitet direkt an den richtigen Port.

**4. Router 1 empfängt Paket**

- Routing-Tabelle ist vollständig (EIGRP läuft).
- Zielnetz [172.21.4.0/24](http://172.21.4.0/24) → Next-Hop = Router 2 (172.21.1.2).
- Prüft ARP für Router 2:
    - Kennt MAC von 172.21.1.2 (vom ersten Ping) → **kein ARP nötig**.
- Sendet Paket an Router 2.

**5. Router 2 empfängt Paket**

- Zielnetz [172.21.4.0/24](http://172.21.4.0/24) → direkt verbunden (VLAN 30).
- Prüft ARP für PC04:
    - Kennt MAC von PC04 (vom ersten Ping) → **kein ARP nötig**.
- Sendet ICMP Echo Request an PC04.

**6. PC04 empfängt Ping**

- Antwortet mit ICMP Echo Reply.
- Rückweg identisch, alles gecached → **sehr schnell**.

---



 **Unterschiede zum ersten Ping:**

- **Keine ARP-Broadcasts mehr** → alles im Cache.
- **Keine EIGRP-Nachbarschaftsbildung mehr** → Routing steht.
- Switches nutzen ihre MAC-Tabellen → kein Flooding.
- Kommunikation ist **direkt und effizient**