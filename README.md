# Anycast Nameserver - Installationsanleitung

## Voraussetzungen

- Node.js (v14 oder höher)
- NPM (Node Package Manager)
- Root-Rechte für Port 53 (Standard-DNS-Port)

## Installation

1. Erstelle ein neues Verzeichnis und initialisiere ein Node.js-Projekt:

```bash
mkdir anycast-nameserver
cd anycast-nameserver
npm init -y
```

2. Installiere die benötigten Abhängigkeiten:

```bash
npm install dns2 axios geoip-lite
```

3. Kopiere den Code in eine Datei `server.js`

4. Starte den Server:

```bash
sudo node server.js
```

Root-Rechte (sudo) werden benötigt, da der DNS-Server auf Port 53 läuft.

## Verwendung

Der Nameserver übernimmt nun DNS-Anfragen und leitet sie an die nächstgelegene IP-Adresse aus der API weiter. Um eine Domain über diesen Nameserver zu verwenden:

1. Konfiguriere die betreffenden Domains, dass sie diesen Nameserver als autoritativen DNS-Server verwenden.
2. Stelle sicher, dass die IPs aus der API die entsprechenden Dienste bereitstellen.

## Funktionsweise

- Der Server fragt regelmäßig die API `https://staticapi.fastasfuck.net/locations` ab, um verfügbare IP-Adressen zu erhalten.
- Alle 5 Minuten wird für jede IP-Adresse geprüft, ob der Server auf TCP-Port 25565 erreichbar ist.
- Bei DNS-Anfragen wird anhand der IP-Adresse des anfragenden Clients dessen ungefährer Standort ermittelt.
- Die geografisch nächstgelegene **erreichbare** IP-Adresse aus der API wird als Antwort zurückgegeben.
- Wenn keine Server erreichbar sind, wird als Fallback die geografisch nächste IP verwendet.
- DNS-Anfragen werden sowohl über TCP als auch UDP beantwortet.

## Erweiterungsmöglichkeiten

1. Erweiterte Fehlerbehandlung und Fallback-Strategien
2. Unterstützung für verschiedene Anfrage-Typen (A, AAAA, CNAME, etc.)
3. Lastenausgleich zwischen mehreren nahen IPs
4. Cache-Mechanismen für DNS-Anfragen
5. Monitoring und Logging-Funktionen
6. Konfigurationsdatei für anpassbare Einstellungen
