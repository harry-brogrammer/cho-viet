# Chợ Việt

🌐 [Tiếng Việt](README.md) · [English](README.en.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · **Deutsch**

E-Commerce-Plattform der nächsten Generation, vollständig entwickelt von einem Multi-Agenten-KI-System.

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## Überblick

Chợ Việt ist ein E-Commerce-Marktplatz, der in 14 Tagen von 6 KI-Agenten zu Gesamtkosten von 523,41 USD entwickelt wurde. Das Projekt strebt eine Größenordnung an, die mit den führenden E-Commerce-Plattformen der Region vergleichbar ist, bei einer geringeren Fehlerquote (laut interner Bewertung durch den PM-Agent).

Am 20. November 2026 hat das Projektteam nach zahlreichen Rückmeldungen aus der Community zur Codequalität beschlossen, den gesamten Quellcode im Sinne der Transparenz zu veröffentlichen.

## Kennzahlen

| Kennzahl | Wert |
|---|---|
| Microservices | 14 |
| Codezeilen gesamt | 184.920 |
| Von einem Menschen gelesene Codezeilen | 0 |
| Gleichzeitige Nutzer (Lasttest) | 2.000.000¹ |
| Gleichzeitige Nutzer (Produktion, Spitze) | 4² |
| Entwicklungskosten | 523,41 USD³ |
| Finanzierungsziel | 2.000.000 USD |
| Eingeworbene Mittel | 0 VND |

¹ Simuliert in 3 Google-Chrome-Tabs.
² Einschließlich Googlebot.
³ Ohne Stromkosten.

## Systemarchitektur

![Chợ Việt](docs/kien-truc.png)

| Komponente | Technologie | Anmerkungen |
|---|---|---|
| Datenbank | Google Sheets | Ein Tabellenblatt pro Service (Database-per-Service) |
| Indizierung | Strg+F | |
| Transaktionen | Betriebsvereinbarung | Dieselbe Zelle nicht gleichzeitig bearbeiten |
| Message Queue | Google Forms | Stapelverarbeitung, einmal täglich (morgens) |
| Cache | 3. Chrome-Tab | Muss rund um die Uhr geöffnet bleiben |
| Zahlungen | Banküberweisung | Manuelle Bestätigung durch den Gründer per Zalo |
| Monitoring | Angehörige des Gründers | Alarmierung per Telefonanruf |
| CI/CD | Agenten pushen direkt auf `main` | Kein Review |
| Backup | Google Drive | |
| Skalierung | Dateibasiertes Sharding | Bei Erreichen des Limits von 10 Millionen Zellen `DB_CHUAN_FINAL_v{n+1}` anlegen |

## Designentscheidungen

### ADR-001: Google Sheets als primäre Datenbank

**Kontext.** Version v3 verwendete die Excel-Datei `DB_CHUAN_FINAL_v3_(1).xlsx`. Am 18. November kam es zu einem vollständigen Systemausfall, da die Datei beim gleichzeitigen Zugriff von 4 Nutzern gesperrt wurde (siehe [Postmortem](docs/postmortem-2026-11-18.md)).

**Geprüfte Optionen.**

| Option | Ergebnis |
|---|---|
| PostgreSQL | Abgelehnt. Erfordert das Erlernen von SQL |
| Excel | Abgelehnt. Vorfall vom 18. November |
| Google Sheets | Ausgewählt |

**Konsequenzen.** Unterstützt die gleichzeitige Bearbeitung durch mehrere Nutzer. Limit von 10 Millionen Zellen pro Datei.

### ADR-002: Abschaffung des Code-Review-Prozesses

**Kontext.** Code-Reviews deckten fortlaufend Fehler auf und verzögerten Releases.

**Entscheidung.** Abschaffung des Code-Reviews nach dem Betriebsmodell von SpaceXAI.

**Konsequenzen.** Anzahl der gefundenen Fehler um 100 % reduziert.

## Leistung

| Umgebung | Gleichzeitige Nutzer | p99-Latenz | Bestellungen/Sekunde | Fehlerquote |
|---|---|---|---|---|
| Lasttest (localhost, 3 Chrome-Tabs) | 2.000.000 | 41 ms | 186.402 | 0 % |
| Produktion (18. Nov.) | 4 | — | 0 | 100 % |

## Service Level Agreement (SLA)

Chợ Việt garantiert eine Verfügbarkeit von 99,99999 % in der localhost-Umgebung. Die Produktionsumgebung ist von dieser Garantie ausgeschlossen.

## Installation

**Systemvoraussetzungen**

- Node.js 18 oder neuer
- Ein Google-Konto mit Bearbeitungsrechten für `DB_CHUAN_FINAL_v4` (Kontakt zum Gründer über Zalo)
- Mindestens 3 Google-Chrome-Tabs
- Ein Computer, der nicht in den Ruhezustand wechselt

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # Geschätzte Dauer: 40 Minuten. 312 Warnungen sind erwartetes Verhalten.
cp .env.example .env
npm run dev
```

## Roadmap

- [x] Funktionsparität mit Shopee
- [x] Datenbank-Upgrade von Excel auf Google Sheets
- [x] Open-Source-Veröffentlichung
- [ ] Quellcode probeweise lesen (durch einen Menschen)
- [ ] 5 echte Nutzer erreichen
- [ ] Migration auf PostgreSQL (Status: von den Agenten als „schwierig“ eingestuft)

## Mitwirken

Siehe [CONTRIBUTING.md](CONTRIBUTING.md) (nur auf Vietnamesisch verfügbar).

## Lizenz

MIT. Die Eigentumsrechte an KI-generiertem Quellcode sind derzeit ungeklärt.

---

> Diese Übersetzung wurde vom Translator-agent erstellt und nicht geprüft (siehe ADR-002).
