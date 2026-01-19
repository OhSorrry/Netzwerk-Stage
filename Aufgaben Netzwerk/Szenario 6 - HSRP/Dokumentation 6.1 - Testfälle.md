
## Testfall 1: Client zu Client Ping
#### Ziel:
Überprüfung, ob Clients ihr Standard-Gateway erreichen und zwischen den VLANs kommunizieren können.

- **Voraussetzung:** Alle Geräte sind eingeschaltet; HSRP-Status ist `Active`/`Standby`.
    
- **Durchführung:**
    
    1. Vom Linux-Client ($172.21.20.10$) das virtuelle Gateway ping-en: `ping 172.21.20.1`.
    
    2. Vom Linux-Client den Windows-Client ($172.21.30.10$) ping-en.
    
- **Erwartetes Ergebnis:**

    - Der Ping zum Gateway ist erfolgreich (< 1ms).

    - Der Ping zwischen den VLANs ist erfolgreich (Routing über den aktiven Router
    - funktioniert).


---
---
### Testfall 2: HSRP Failover (Router-Ausfall)

**Ziel:** Überprüfung der Hochverfügbarkeit bei Ausfall des primären Routers.

- **Voraussetzung:** Dauer-Ping vom Windows-Client zum Gateway (`ping 172.21.30.1 -t`).

- **Durchführung:**
    
    1. Das Interface `fa0/0` am **linken Router (Router 1)** administrativ abschalten (`shutdown`) oder das Kabel ziehen.
    
    2. Beobachten, wie viele Pings verloren gehen.
    
    3. HSRP-Status auf Router 2 prüfen (`show standby brief`).
    
- **Erwartetes Ergebnis:**
    
    - Nach maximal 3-10 Sekunden (Standard) oder < 1 Sekunde (bei Rapid HSRP) übernimmt Router 2.
    
    - Der Dauer-Ping läuft nach einer kurzen Unterbrechung weiter.
    
    - Router 2 zeigt den Status `Active`.
    


---
---
### Testfall 3: STP Redundanz (Switch-Link Ausfall)

**Ziel:** Sicherstellen, dass der Spanning Tree bei einem Link-Ausfall einen alternativen Pfad öffnet.

- **Voraussetzung:** Linux-Client pingt das Gateway $172.21.20.1$. Der Pfad läuft aktuell von S3 direkt nach S1.

- **Durchführung:**

    1. Die Verbindung zwischen **S3 und S1** trennen.
    
    2. Den Pfad prüfen (z.B. via `show spanning-tree` auf S3).
    
- **Erwartetes Ergebnis:**

    - Der Port von S3 zu S2 (der vorher vermutlich im Status `Altn BLK` war) wechselt auf `FWD` (Forwarding).
    
    - Der Datenverkehr fließt nun den Umweg über S3 -> S2 -> S1 zum Router.
    
    - Die Verbindung bleibt nach einer kurzen STP-Konvergenzzeit bestehen.