# ELK Log Management Projekt

## Projektbeschreibung

Dieses Projekt realisiert ein zentrales Log-Management-System auf Basis des **ELK-Stacks** (Elasticsearch, Logstash, Kibana) mit Unterstützung von **Beats** (Filebeat & Winlogbeat). Ziel ist es, Systemereignisse zentral zu erfassen, zu analysieren und visuell darzustellen. Das Setup wurde in einer virtualisierten Umgebung mit **Proxmox** realisiert.

## Projektstruktur

elk-logging/
├── architecture/ # Architektur- und Datenflussdiagramme (Mermaid)
│ └── datenfluss.mmd
├── configs/ # Konfigurationsdateien für Beats
│ ├── filebeat.yml
│ └── winlogbeat.yml
├── docs/ # Projektdokumentation
│ └── projektbeschreibung.md
├── screenshots/ # Screenshots zur Dokumentation und Präsentation
├── .gitignore # Git-Ausnahmen
└── README.md # Diese Datei




## Verwendete Komponenten

| Komponente    | Version  | Beschreibung                                  |
|---------------|----------|-----------------------------------------------|
| Elasticsearch | 7.17.x   | Zentrale Speicherung und Suche der Logs       |
| Logstash      | 7.17.x   | Verarbeitung, Filterung und Parsing der Logs  |
| Kibana        | 7.17.x   | Dashboard-basierte Visualisierung der Logs    |
| Filebeat      | 7.17.x   | Lightweight Log-Shipper für Linux-Systeme     |
| Winlogbeat    | 7.17.x   | Spezieller Log-Shipper für Windows Event Logs |
---------------------------------------------------------------------------

## Sicherheit (TLS / Authentifizierung)

Zur Absicherung der Kommunikation wurden **SSL/TLS-Zertifikate** mit dem Tool `elasticsearch-certutil` erzeugt. Die Zertifikate liegen im Verzeichnis `elasticsearch/certs/` und sind in Elasticsearch, Logstash und Kibana eingebunden.

Die Authentifizierung erfolgt über die integrierte Benutzerverwaltung von Elasticsearch, um unautorisierten Zugriff zu verhindern.


# Infrastructure

 **Hypervisor:** Proxmox VE
- **Container:** 
  - `elk-container` mit Elasticsearch + Kibana
- **Virtuelle Maschinen (VMs):** 
  - Ubuntu-VM mit Filebeat 
  - Windows Server VM mit Winlogbeat


## Datenfluss

Das Datenflussdiagramm in [`architecture/datenfluss.mmd`](architecture/datenfluss.mmd) visualisiert die komplette Logverarbeitungskette:

## Setup-Hinweise

1. **Elasticsearch konfigurieren:** Einstellungen in `elasticsearch.yml` anpassen 
2. **Zertifikate generieren und einbinden:** optional für TLS-Verbindungen 
3. **Logstash konfigurieren:** Pipeline-Definitionen in `logstash.conf` hinterlegen 
4. **Filebeat & Winlogbeat konfigurieren:** Verbindungsdaten und Module definieren 
5. **Kibana starten und Dashboards einrichten** 




## Status

✅ Elasticsearch läuft

✅ Filebeat sendet Logs

✅ Winlogbeat funktioniert

✅ Dashboards in Kibana eingerichtet

## Dokumentation

Die ausführliche Projektdokumentation mit Hintergrund, Architektur, Sicherheitsaspekten und Ergebnissen befindet sich in [`docs/projektbeschreibung.md`](docs/projektbeschreibung.md).

## Screenshots

Visuelle Eindrücke der Dashboards und der Kibana-Konfiguration sind im Ordner [`screenshots/`](screenshots/) abgelegt.

## Lizenz

Dieses Projekt wurde im Rahmen der Weiterbildung zum **Fachinformatiker für Systemintegration** entwickelt. Nutzung und Weiterverwendung nach Absprache.

## Mitwirkende

- Kamal Najib (Projektverantwortlicher)

**Letzte Aktualisierung:** Juli 2025
