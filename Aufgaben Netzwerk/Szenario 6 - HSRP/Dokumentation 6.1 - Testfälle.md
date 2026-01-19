
## Testfall 1: Client zu Client Ping

#### Angaben des Testers:
Noé Messmer
noe.messmer@raiffeisen.ch
+41 78 767 86 46

#### Testart:
- Sanity Test

#### Ziel:
Die Clients prüfen ob sie sich gegenseitig und somit auch den Gateway pingen können.

#### Voraussetzungen:
Alle Geräte sind korrekt konfiguriert und eingeschaltet.

#### Durchführung:
- Vom Windows Client **172.21.30.10** den virtuellen Gateway pingen: 
```cisco
ping 172.21.30.1
```

- Vom Windows Client **172.21.30.10** den Linux Client **172.21.20.10** pingen:
```cisco
ping 172.21.20.10
```

#### Erwartetes Ergebnis:
- Der ping von Client zu Gateway ist erfolgreich, Routing im VLAN funktioniert
- Der Ping von Client zu Client ist erfolgreich, Routing über verschiedene VLANs funktioniert

#### Ergebnis:
Ping zu Gateway:
![](Screenshots/Pasted%20image%2020260119145556.png)

Ping zu Linux Client:
![](Screenshots/Pasted%20image%2020260119145659.png)

Beide Pings sind ohne Fehler verlaufen und alle Pakete sind empfangen worden. Der Test war ein Erfolg.
#### Mängelklasse:
Der Test war erfolgreich, es besteht keinen Grund eine Mängelklasse zu bestimmen.


---
---
### Testfall 2: Router Ausfall
#### Angaben des Testers:
Noé Messmer
noe.messmer@raiffeisen.ch
+41 78 767 86 46

#### Testart:
- 
#### Ziel:
Überprüfung der Übernahme des passiven Routers bei dem Ausfall des primären Routers.

#### Voraussetzung: 
Alle Geräte sind korrekt konfiguriert und eingeschaltet. 
Ein Dauerhafter Ping vom Windows Client zum Linux Client:
```cisco
ping -t 172.21.20.10
```
#### Durchführung:
- Ping wird auf Windows Client gestartet:
```cisco
ping -t 172.21.20.10
```
- Das Kabel wird am Router 1 auf dem interface **fa0/0** ausgesteckt, um einen Router Absturz zu simulieren
- Auf dem laufendem Ping beobachten wieviele Pakete verloren gehen.

- Das Kabel wird am Router wieder auf **fa0/0** wieder eingesteckt.
- Auf dem Ping wird wieder beobachtet wieviele Pakete verloren gehen.
#### Erwartetes Ergebnis:
- Nach maximal 3 Sekunden übernimmt der Router 2 das Routing.
- Der Ping unterbricht nur kurz, nach maximal 1-2 verlorenen Paketen übernimmt der Router 2

#### Ergebnis:
Ping zu Linux Client beim Ausstecken des Kabels:
![](Screenshots/Pasted%20image%2020260119150858.png)

Ping zu Linux Client beim Einstecken des Kabels:
![](Screenshots/Pasted%20image%2020260119151004.png)

Bei dem Ping gab es jeweils nur ein verlorenes Paket beim Aus- und Einstecken des Kabels. Der Test ist im erwarteten Rahmen und war somit erfolgreich.
#### Mängelklasse:
Der Test war erfolgreich, es besteht keinen Grund eine Mängelklasse zu bestimmen.


---
---
### Testfall 3: STP Redundanz 
#### Angaben des Testers:
Noé Messmer
noe.messmer@raiffeisen.ch
+41 78 767 86 46

#### Testart:
- 

#### Ziel: 
Sicherstellen dass das STP funktioniert und beim Ausfall der Haupt Route die Sekundäre schnell einspringt.

#### Voraussetzung: 
Alle Geräte sind korrekt konfiguriert und eingeschaltet. 
Der Pfad läuft standartmässig über Switch 1.
Ein Dauerhafter Ping vom Windows Client zum Gateway:
```cisco
ping -t 172.21.30.1
```
#### Durchführung:
- Das Kabel wird zwischen Switch 1 und Switch 3 getrennt.
- Der Ping zum Gateway wird beobachtet wie viele Pakete verloren gehen
- Die Route auf Switch 3 prüfen 
```cisco
show spanning-tree
``` 
  
#### Erwartetes Ergebnis:
- Der Datenverkehr fliesst nun vom Windows Client über **Switch 3 --> Switch 2 --> Switch 1** zum Router.
- Die Verbindung wird nur kurz unterbrochen, nach 1-2 verlorenen Paketen besteht die Verbindung wieder.


#### Ergebnis:


#### Mängelklasse:

