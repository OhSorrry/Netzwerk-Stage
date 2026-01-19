
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
- Vom Windows Client **172.21.20.10** den virtuellen Gateway pingen: 
```cisco
ping 172.21.20.1
```

- Vom Windows Client **172.21.20.10** den Linux Client **172.21.30.10** pingen:
```cisco
ping 172.21.30.10
```

#### Erwartetes Ergebnis:
- Der ping von Client zu Gateway ist erfolgreich, Routing im VLAN funktioniert
- Der Ping von Client zu Client ist erfolgreich, Routing über verschiedene VLANs funktioniert

#### Ergebnis:


#### Mängelklasse:



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
Alle Geräte sind korrekt konfiguriert und eingeschaltet. Ein Dauerhafter Ping vom Windows Client zum Gateway:
```cisco
ping -t 172.21.30.1
```
#### Durchführung:
- Ping wird auf Windows Client gestartet:
```cisco
ping -t 172.21.30.1
```
- Das Kabel wird am Router 1 auf dem interface **fa0/0** ausgesteckt, um einen Router Absturz zu simulieren
- Auf dem laufendem Ping beobachten wieviele Pakete verloren gehen.

- Das Kabel wird am Router wieder auf **fa0/0** wieder eingesteckt.
- Auf dem Ping wird wieder beobachtet wieviele Pakete verloren gehen.
#### Erwartetes Ergebnis:
- Nach maximal 3 Sekunden übernimmt der Router 2 das Routing.
- Der Ping unterbricht nur kurz, nach maximal 1-2 verlorenen Paketen übernimmt der Router 2

#### Ergebnis:


#### Mängelklasse:




---
---
### Testfall 3: STP Redundanz 
#### Angaben des Testers:
Noé Messmer
noe.messmer@raiffeisen.ch
+41 78 767 86 46

#### Testart:
- Sanity Test

#### Ziel: 
Sicherstellen, dass der Spanning Tree bei einem Link-Ausfall einen alternativen Pfad öffnet.

#### Voraussetzung: 
Linux-Client pingt das Gateway $172.21.20.1$. Der Pfad läuft aktuell von S3 direkt nach S1.

#### Durchführung:

    1. Die Verbindung zwischen **S3 und S1** trennen.
    
    2. Den Pfad prüfen (z.B. via `show spanning-tree` auf S3).

#### Erwartetes Ergebnis:

    - Der Port von S3 zu S2 (der vorher vermutlich im Status `Altn BLK` war) wechselt auf `FWD` (Forwarding).
    
    - Der Datenverkehr fließt nun den Umweg über S3 -> S2 -> S1 zum Router.
    
    - Die Verbindung bleibt nach einer kurzen STP-Konvergenzzeit bestehen.


#### Ergebnis:


#### Mängelklasse:

