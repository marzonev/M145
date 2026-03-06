# Cisco Packet Tracer

- [Cisco Packet Tracer](#cisco-packet-tracer)
  - [Aufgabe 1 - Navigate the IOS](#aufgabe-1---navigate-the-ios)
    - [Part 1](#part-1)
      - [Serial Connection](#serial-connection)
    - [Part 2](#part-2)
      - [Befehle anzeigen](#befehle-anzeigen)
      - [priveleged EXEC mode öffnen](#priveleged-exec-mode-öffnen)
      - [Global Configuration mode](#global-configuration-mode)
    - [Part 3](#part-3)
  - [Aufgabe 2 - Configure Initial Switch Settings](#aufgabe-2---configure-initial-switch-settings)
    - [Switch config](#switch-config)
    - [Passwörter wechseln](#passwörter-wechseln)
      - [Console Passwort](#console-passwort)
      - [Enable Passwort](#enable-passwort)
      - [Veschlüsseltes Passwort](#veschlüsseltes-passwort)
    - [MOTD Banner erstellen](#motd-banner-erstellen)
    - [Config speichern in nvram](#config-speichern-in-nvram)
  - [Aufgabe 3 - Implement Basic Connectivity](#aufgabe-3---implement-basic-connectivity)
    - [Management IP setzen](#management-ip-setzen)
  - [Aufgabe 4 - Identify MAC and IP Addresses](#aufgabe-4---identify-mac-and-ip-addresses)
    - [PDU-Tabellen](#pdu-tabellen)
      - [Part 1 – Lokale Kommunikation (172.16.31.5 → 172.16.31.2)](#part-1--lokale-kommunikation-17216315--17216312)
      - [Part 2 – Remote-Kommunikation (172.16.31.5 → 10.10.10.2)](#part-2--remote-kommunikation-17216315--1010102)
    - [Reflection Questions – Kurzantworten](#reflection-questions--kurzantworten)
  - [Aufgabe 5](#aufgabe-5)
    - [Wichtige Befehle](#wichtige-befehle)
      - [ARP-Befehle (End Device)](#arp-befehle-end-device)
      - [Switch-Befehle (CLI)](#switch-befehle-cli)
      - [Router-Befehle (CLI)](#router-befehle-cli)
    - [Ablauf auf einen Blick](#ablauf-auf-einen-blick)

## Aufgabe 1 - Navigate the IOS

### Part 1

Verbindung zwischen Switch und PC

![connection](media/connection.png)

#### Serial Connection

Wichtig bei der Serial Verbindung ist, dass die Baud Rate auf beiden Geräten gleich ist.

![serial](media/serial.png)

### Part 2

#### Befehle anzeigen

Mit `?` werden alle Befehle angezeigt.

Mit `r?` können alle Befehle welche mit "r" beginnen angezeigt werden.

#### priveleged EXEC mode öffnen

``` cisco
enable
```

#### Global Configuration mode

Hiermit kann man den Global Configuration mode öffnen

``` cisco
configure
```

### Part 3

Hier sieht man wie ich mit `clock` die Uhrzeit und das Datum geändert habe.

![clock](media/clock.png)

## Aufgabe 2 - Configure Initial Switch Settings

### Switch config

Hier sieht man meine Switch Config mit `sh run`

### Passwörter wechseln

**Wichtig** Passwöter werden im Klartext gespeichert

#### Console Passwort

``` cisco
line console 0
```

``` cisco
password letmein
```

``` cisco
login
```

#### Enable Passwort

``` cisco
enable password c1$c0
```

#### Veschlüsseltes Passwort

``` cisco
enable secret itsasecret
```

Passwort im Klartext nachträglich verschlüsseln

``` cisco
service password-encryption
```

### MOTD Banner erstellen

``` cisco
banner motd "This is a secure system. Authorized Access Only!"
```

### Config speichern in nvram

Mit diesem Befehl in NVRAM speichern dass es nach dem neustarten nicht zurückgesetzt wird.

``` cisco
copy running-config startup-config
```

Kann aber auch mit dem veralteten Befehlt gemacht werden.

``` cisco
write
```

oder

``` cisco
wr
```

## Aufgabe 3 - Implement Basic Connectivity

### Management IP setzen

``` cisco
interface vlan 1
```

``` cisco
ip address 192.168.1.253 255.255.255.0
```

``` cisco
no shutdown
```

## Aufgabe 4 - Identify MAC and IP Addresses  

### PDU-Tabellen

#### Part 1 – Lokale Kommunikation (172.16.31.5 → 172.16.31.2)

| At Device   | Dest. MAC      | Src MAC        | Src IPv4    | Dest IPv4   |
| ----------- | -------------- | -------------- | ----------- | ----------- |
| 172.16.31.5 | 000C.85CC.1DA7 | 00D0.D311.C788 | 172.16.31.5 | 172.16.31.2 |
| Switch1     | 000C:85CC:1DA7 | 00D0:D311:C788 |             |             |
| Hub         | N/A            | N/A            | N/A         | N/A         |
| 172.16.31.2 | 00D0:D311:C788 | 000C:85CC:1DA7 | 172.16.31.2 | 172.16.31.5 |

> Switch und Hub leiten das PDU weiter, ohne IP-Adressen zu verarbeiten.  
> Beim Hub werden MACs ignoriert – er broadcastet auf alle Ports.

---

#### Part 2 – Remote-Kommunikation (172.16.31.5 → 10.10.10.2)

| At Device    | Dest. MAC        | Src MAC          | Src IPv4     | Dest IPv4    |
|--------------|------------------|------------------|--------------|--------------|
| 172.16.31.5  | 00D0:BA8E:741A   | 00D0:D311:C788   | 172.16.31.5  | 10.10.10.2   |
| Switch1      | 00D0:BA8E:741A   | 00D0:D311:C788   | N/A          | N/A          |
| Router       | 0060:2F84:4AB6   | 00D0:588C:2401   | 172.16.31.5  | 10.10.10.2   |
| Switch0      | 0060:2F84:4AB6   | 00D0:588C:2401   | N/A          | N/A          |
| Access Point | N/A              | N/A              | N/A          | N/A          |
| 10.10.10.2   | 00D0:588C:2401   | 0060:2F84:4AB6   | 10.10.10.2   | 172.16.31.5  |

> Am Router ändern sich die MAC-Adressen – die IP-Adressen bleiben konstant.

---

### Reflection Questions – Kurzantworten

| #   | Frage                                           | Antwort                                                                                                                                   |
| --- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Verschiedene Kabeltypen?                        | Ja – Kupferkabel (wired) und WLAN (wireless)                                                                                              |
| 2   | Kabel beeinflussen PDU?                         | Nein – Inhalt bleibt gleich, nur physische Übertragung ändert sich                                                                        |
| 3   | Hub verliert Infos?                             | Nein – broadcastet alles unverändert                                                                                                      |
| 4   | Hub und MAC/IP?                                 | Hub ignoriert beides – arbeitet nur auf Layer 1                                                                                           |
| 5   | Access Point verändert Infos?                   | Nein – leitet nur weiter (Wired ↔ Wireless)                                                                                               |
| 6   | MAC/IP durch WLAN verloren?                     | Nein – alle Adressen bleiben erhalten                                                                                                     |
| 7   | Höchster OSI-Layer bei Hub/AP?                  | Hub: Layer 1 · Access Point: Layer 1–2                                                                                                    |
| 8   | Hub/AP replizieren abgelehnte PDUs?             | Ja – Hub broadcastet an alle Ports, Kopien werden mit rotem X abgelehnt                                                                   |
| 9   | Welche MAC zuerst im PDU Details?               | Destination MAC                                                                                                                           |
| 10  | Warum Dest. zuerst?                             | Ethernet-Frame-Struktur – Destination steht als erstes Feld im Header                                                                     |
| 11  | Muster bei MAC-Adressen?                        | Lokale MACs auf gleichem Segment · Router tauscht MACs an Netzwerkgrenze                                                                  |
| 12  | Switch repliziert abgelehnte PDUs?              | Nur beim ersten Mal, bevor MAC-Tabelle gelernt ist                                                                                        |
| 13  | Wo änderten sich die MACs (10 ↔ 172)?           | Am **Router**                                                                                                                             |
| 14  | Gerät mit MAC 00D0:BA?                          | Router (LAN-Interface Richtung 172.16.31.0)                                                                                               |
| 15  | Andere MAC-Adressen?                            | `00D0:D311:C788` = 172.16.31.5 · `000C:85CC:1DA7` = 172.16.31.2 · `00D0:588C:2401` = Router WAN-Interface · `0060:2F84:4AB6` = 10.10.10.2 |
| 16  | IPv4-Adressen geändert?                         | Nein – Source/Dest-IP bleiben immer gleich                                                                                                |
| 17  | Beim Pong tauschen IPv4-Adressen?               | Ja – Quelle und Ziel werden vertauscht                                                                                                    |
| 18  | IPv4-Muster?                                    | Zwei Netze: **172.16.31.0/24** (LAN) und **10.10.10.0/24** (WLAN), verbunden durch Router                                                 |
| 19  | Warum braucht Router verschiedene IPs pro Port? | Um Routing-Entscheidungen treffen zu können – jedes Interface muss in einem anderen Netz liegen                                           |
| 20  | Was wäre mit IPv6 anders?                       | 128-bit Hex-Adressen · ARP → NDP · kein Broadcast, stattdessen Multicast                                                                  |

## Aufgabe 5

### Wichtige Befehle

#### ARP-Befehle (End Device)

ARP-Tabelle anzeigen:

``` bash
arp -a
```

ARP-Tabelle leeren:

``` bash
arp -d
```

Ping senden (löst ARP-Request aus, wenn MAC unbekannt):

``` bash
ping <IP>
```

---

#### Switch-Befehle (CLI)

MAC-Adresstabelle des Switches anzeigen:

``` cisco
show mac-address-table
```

---

#### Router-Befehle (CLI)

In den Privileged EXEC Mode wechseln:

``` cisco
enable
```

ARP-Tabelle des Routers anzeigen:

``` cisco
show arp
```

MAC-Tabelle anzeigen:

``` cisco
show mac-address-table
```

### Ablauf auf einen Blick

1. **ARP-Request** – Gerät kennt Ziel-IP, aber nicht die MAC → sendet Broadcast an `FF:FF:FF:FF:FF:FF`
2. **ARP-Reply** – Zielgerät antwortet mit seiner MAC (Unicast)
3. **ICMP (Ping)** – Erst jetzt wird der eigentliche Ping gesendet
4. **Remote-Kommunikation** – Bei Zielen außerhalb des eigenen Netzes wird die MAC des **Default Gateways (Router)** per ARP ermittelt, nicht die des Zielgeräts