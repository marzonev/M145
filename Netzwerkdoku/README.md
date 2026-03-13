# Netzwerkdoku interpretieren

## Fragen & Antworten

---

### 1. Wie lautet die Netzwerkadresse vom Standort Samedan?

**192.168.3.0/24**

---

### 2. Auf welcher IP-Adresse befindet sich der AccessPoint in Bellinzona?

Der AccessPoint trägt die IP **.30**, also **192.168.4.30**.  
Er ist an Port 24 des Switches angeschlossen (VLAN-3).

---

### 3. In welchem VLAN befinden sich die Arbeitsplatz-PCs?

**VLAN-2 (Office)**, Ports 1–23 des jeweiligen Switches.

---

### 4. Über welche IP-Adresse erreicht man den Manageable-Switch am Standort Chur?

**192.168.2.253**  
(Beide gestackten ZyXEL XGS1910-24 teilen sich diese Adresse.)

---

### 5. Welche IP-Adressen LAN-seitig und tunnelseitig ist der VPN-Gateway am Standort Bellinzona konfiguriert?

| Seite | IP-Adresse |
|-------|-----------|
| LAN-seitig (Gateway/DHCP) | **192.168.4.1** |
| Tunnelseitig VPN-B (Point-to-Point nach Chur) | **172.200.4.2/24** |

---

### 6. Wie lautet der am Standort Samedan an den Arbeitsplatz-PCs eingetragene Standardgateway?

**192.168.3.1** (Router/Firewall/Web-Proxy)

---

### 7. Ein Aussendienstmitarbeiter benötigt Zugriff auf das Firmennetzwerk. Wie muss er seine VPN-SW IP-mässig konfigurieren?

Der RAS-VPN (VPN-C) läuft über den Hauptsitz Chur:

| Parameter | Wert |
|-----------|------|
| VPN-Zieladresse | `ingcaprez.ch` (DynDNS) bzw. `82.4.12.158` |
| VPN-Tunnel-Netz | **172.200.5.x/24** (VPN-C RAS) |
| Gateway (Chur-seitig) | **172.200.5.1** |

---

### 8. Wer hat im Büro CAD Wasserbau die VoIP-Telefone installiert?

**abisang** (Einträge E10/1, E10/3, E11/1, E11/3 in der Belegungsliste)

---

### 9. Wann wurde im Bistro der AccessPoint installiert?

**23.07.2014**, installiert von **rsauter** (Eintrag E8/1 in der Belegungsliste).

---

### 10. René Sauter (rsauter) verlässt die Firma – betroffene Verantwortlichkeiten

**Installationen von rsauter laut Belegungsliste:**

| Anschluss | Lokation | Beschreibung | Datum |
|-----------|----------|-------------|-------|
| E1/1 | Empfang EG | Arbeitsplatz-PC | 29.11.13 |
| E2/1 | CAD Tiefbau | CAD Workstation | 13.08.13 |
| E3/2 | Bauleiterbüro | MF-Kopierer | 11.10.13 |
| E8/1 | Bistro | AccessPoint | 23.07.14 |

**Empfohlener Nachfolger: blaeuchli**  
blaeuchli hat die meisten weiteren Installationen durchgeführt (E2/4, E6/1, E10/2, E10/4, E11/2, E11/4) und verfügt über entsprechende Erfahrung mit der Infrastruktur.

---

### 11. Wieviele RJ45-Steckdosen stehen im Bauleiterbüro zur Verfügung und wieviele sind noch frei?

Laut Verkabelungsplan: Dosen **E3 (2×), E4 (2×), E5 (2×), E6 (2×)** = **total 8 Anschlüsse**

| Anschluss | Status |
|-----------|--------|
| E3/1 | ✅ **Frei** |
| E3/2 | Belegt – MF-Kopierer |
| E4/1 | Belegt – VoIP-Telefon |
| E4/2 | Belegt – Arbeitsplatz-PC |
| E5/1 | Belegt – Arbeitsplatz-PC |
| E5/2 | Belegt – VoIP-Telefon |
| E6/1 | Belegt – Arbeitsplatz-PC |
| E6/2 | ✅ **Frei** |

**2 Anschlüsse frei** (E3/1 und E6/2)

---

### 12. Im CAD Tiefbau muss ein weiteres VoIP-Telefon installiert werden – Patchpanelbelegung und Switch-Port?

Anschluss **E2/2** ist laut Belegungsliste frei.

| Parameter | Wert |
|-----------|------|
| Patchpanel-Anschluss | **E2/2** |
| VLAN | **VLAN-1 (Telefon)** |
| Switch-Port-Bereich | Ports 24–47 (Chur, ZyXEL XGS1910-24) |

Der Switch-Port muss auf VLAN-1 konfiguriert werden.

---

### 13. Im Bistro soll eine Projektpräsentation stattfinden – Ethernet-Verbindung bereitstellen

Verfügbare Dosen im Bistro: **E8 (2×), E9 (2×)**

| Anschluss | Status |
|-----------|--------|
| E8/1 | Belegt – AccessPoint |
| E8/2 | ✅ **Frei** |
| E9/1 | ✅ **Frei** |
| E9/2 | ✅ **Frei** |

**Vorgehen:** E8/2 oder E9/1 verwenden, Patchkabel vom Patchpanel-Port zum Switch-Port legen, VLAN-2 (Office) zuweisen.

---

### 14. Im CAD Wasserbau soll temporär ein weiterer Arbeitsplatz eingerichtet werden

Alle Dosen E10 (4×) und E11 (4×) sind laut Belegungsliste **vollständig belegt** – es gibt keine freien Anschlüsse im Büro.

**Lösungsansätze:**

1. **Unmanaged Switch** an einen bestehenden Port hängen, um zusätzliche Ports bereitzustellen (temporäre Lösung)
2. **WLAN-Verbindung** über den vorhandenen AccessPoint nutzen (VLAN-2, Office) – geeignet für temporären Einsatz

---

### 15. Wieviele Switches stehen am Standort Chur zur Verfügung?

**2 Switches** (ZyXEL XGS1910-24), betrieben als **Stack** – logisch ein Gerät, physisch zwei Einheiten.

---

### 16. Frau Sommer (CAD-Workstation) hat keine Internetverbindung mehr – was wird überprüft?

Schrittweise Fehleranalyse:

1. **Physisch:** Patchkabel am Arbeitsplatz (E2/1) und am Patchpanel auf festen Sitz prüfen, Link-LED am Switch-Port kontrollieren
2. **IP-Konfiguration:** DHCP-Bezug von 192.168.2.3 prüfen, Gateway 192.168.2.1 eingetragen?
3. **Ping auf Gateway:** `ping 192.168.2.1` – kein Ping → Switch/LAN-Problem
4. **Ping auf DNS-Server:** `ping 192.168.2.3` – kein Ping → Server-Problem
5. **Ping auf externe IP:** z. B. `ping 8.8.8.8` – kein Ping → Router/Firewall (IPFire) oder ADSL-Verbindung prüfen
6. **Proxy-Einstellungen:** Web-Proxy im Browser korrekt konfiguriert? (IPFire ist als Web-Proxy aktiv)

---

### 17. Wie lautet die SSID des Churer AccessPoints?

Die SSID ist in der vorliegenden Dokumentation **nicht angegeben**.  
Das Netzwerkdiagramm enthält nur Gerätetyp (ZyXEL NWA1123-AC) und VLAN-Zuordnung.  
Die SSID muss direkt in der **Konfigurationsoberfläche des AccessPoints** nachgeschlagen werden.