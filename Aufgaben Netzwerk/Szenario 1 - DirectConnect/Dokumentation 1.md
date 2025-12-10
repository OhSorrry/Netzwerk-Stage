
# Einleitung

## Auftrag
asdy





## Adresstabelle

| Bauteil:   | Name:      | IP-Adresse:                                        |
| ---------- | ---------- | -------------------------------------------------- |
| Windows PC | *TLABPC01* | *172.21.1.10*                                      |
| Linux PC   | *TLABPC02* | *172.21.2.10*                                      |
| Router     | *TLABr08*  | **FA0/0:** *172.21.1.1*<br>**FA0/1:** *172.21.2.1* |

---
---
# Hauptteil
## IP Domain-lookup deaktivieren
Zuerst mit `enable` in den "**enable**" Modus wechseln, danach mit `conf t` in den "**global configuration mode**" wechseln und mit `no ip domain-lookup` den lookup deaktivieren:
![[Pasted image 20251203164512.png]]

---
## Name ändern
Zuerst in den "**enable**" Modus wechseln:
![[Pasted image 20251203162942.png]]

Danach in den "**global configuration mode**" wechseln und mit `hostname <NeuerName>` den gewünschten Namen setzen:
![[Pasted image 20251203161209.png]]

---
## Interfaces erstellen
Zuerst mit `enable` in den "**enable**" Modus wechseln, danach mit `conf t` in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Dann mittels `interface FAx/x` das gewünschte interface anwählen:
![[Pasted image 20251203165328.png]]

Als nächstes mit `ip address <gewünschte IP> <Subnetzmaske>` die IP ändern.
![[Pasted image 20251203165339.png]]

Mittels `no shutdown` das Interface aktvieren:
![[Pasted image 20251203165723.png]]

Mit `end` aus dem **Interface-config** modus:
![[Pasted image 20251203170011.png]]

---
## Interface description erstellen
Zuerst in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Dann mittels `interface FAx/x` das gewünschte interface anwählen:
![[Pasted image 20251203165328.png]]

Dann mittels `description <Gewünsche Beschreibung>` eine Beschreibung hinzufügen:
![[Pasted image 20251204143110.png]]

---
## Interface überprüfen
zuerst in den "**enable**" Modus wechseln:
![[Pasted image 20251204104251.png]]

Danach mittels `show ip interface brief` den Status abfragen:
![[Pasted image 20251204104233.png]]

`show run interface FAx/x `ist eine weitere Variante ein einzelnes Interface abzufragen:
![[Pasted image 20251204143300.png]]

 ---
## Console Passwort setzen
Zuerst in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Danach mittels `line console 0` den Zugriff bearbeiten:
![[Pasted image 20251204131354.png]]

Dann mit `password <GewünschtesPasswort>` ein Passwort erstellen und mit `end` aus der Konfiguration gehen:
![[Pasted image 20251204131525.png]]

---
## Enable Passwort setzen
Zuerst in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Danach mittels `enable password <NeuesPasswort>` ein Passwort setzen:
![[Pasted image 20251204091454.png]]

Danach mit `end` aus dem Konfigurationsmodus gehen:
![[Pasted image 20251204091514.png]]

Nun sollte jedes wenn man in den "**enable mode**" will eine Passwort abfrage durchgeführt werden:
![[Pasted image 20251204091853.png]]

---
## VTY Access
Zuerst in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Danach mittels `line vty 0 4` die VTY-Lines bearbeiten:
![[Pasted image 20251204145647.png]]

Dann mit `password <GewünschtesPasswort>` ein Passwort für den Connect setzen:
![[Pasted image 20251204145804.png]]

Mittels `login` einen login absetzen:
![[Pasted image 20251204150414.png]]

Dann mittels `transport input telnet` den input auf telnet setzen:
![[Pasted image 20251204150426.png]]

Nun kann man sich über eine reguläre shell (Windows/Linux) ohne direkte Konsolenverbindung auf den Router verbinden, solange diese auf einem Gerät ist das sich im selben Netz befindet. 

Dies funktioniert wenn man in der gewünschten shell den Befehl `telnet <IPdesInterface>` abgibt:
![[Pasted image 20251204150921.png]]
    
Nun sollte noch das gesetze Passwort abgefragt werden. sobald dieses korrekt eingegeben ist, funktioniert die shell wie wenn man direkt auf die Konsole des Routers zugreift:
![[Pasted image 20251204151029.png]]

---
## Banner erstellen
Zuerst in den "**global configuration mode**" wechseln:
![[Pasted image 20251203165244.png]]

Danach mittels `banner motd #Gewünschtes Banner#` den gewünschten Bannertext eingeben:
![[Pasted image 20251204144137.png]]

---
## Konfiguration speichern
Die konfiguration kann im "**enable mode**" mittels `write memory` gespeichert werden:
![[Pasted image 20251203171119.png]]

---
## Konfiguration löschen
zuerst in den "**enable**" Modus wechseln:
![[Pasted image 20251204104251.png]]

Mittels `wr er` die Konfiguration löschen und mit Enter bestätigen:
![[Pasted image 20251208085145.png]]



---
---
## IP auf Windows PC ändern
In den Einstellungen im PC auf "**Netzwerk --> Ethernet**". Dort auf "**IP-Einstellungen bearbeiten**" und die gewünschte IP und Subnetzmaske eintragen.
![[Pasted image 20251204103531.png]]


---
---
# Verwendete Befehle

```
enable
conf t
end
exit
write memory

no ip domain-lookup

hostname <NeuerName>

interface FAx/x
ip address <gewünschte IP> <Subnetzmaske>
no shutdown


```
