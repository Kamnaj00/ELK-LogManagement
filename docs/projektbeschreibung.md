Projektdokumentation – Zentrales Log-Management mit dem ELK-Stack

1. Zielsetzung und Motivation

In modernen IT-Infrastrukturen ist die zentrale Erfassung und Auswertung von Logdaten essenziell für:

- Betriebssicherheit
- Fehleranalyse
- Sicherheitsüberwachung (Security Monitoring)
- Compliance und Auditing

Das Ziel dieses Projekts ist die Implementierung eines zentralisierten, skalierbaren Log-Management-Systems auf Basis des **ELK-Stacks** (Elasticsearch, Logstash, Kibana) in Verbindung mit **Filebeat** und **Winlogbeat** für die Sammlung und Weiterleitung von Logdaten aus verschiedenen Systemen. 

Die Motivation ergibt sich aus dem Bedarf an:

- Einheitlicher Überwachung heterogener Systeme (Linux und Windows)
- Echtzeit-Sichtbarkeit von sicherheitsrelevanten Ereignissen
- Transparenz im Systembetrieb für IT-Administratoren
- Vorbereitung auf spätere Erweiterungen wie Alerting oder SIEM-Funktionen


2. Netzwerk- & Systemarchitektur

Das System wurde in einer virtualisierten Testumgebung auf **Proxmox VE** realisiert. Es besteht aus folgenden Komponenten:

- Infrastrukturüberblick

| System                 | Funktion                             | Plattform   |
|------------------------|--------------------------------------|-------------|
| `elk-container`        | Elasticsearch + Kibana               | LXC         |
| `logstash-container`   | Logstash (Log-Verarbeitung)          | LXC         |
| `ubuntu-vm`            | Linux Client mit Filebeat            | KVM         |
| `windows-server-vm`    | Windows Client mit Winlogbeat        | KVM         |

- Datenfluss

Die Logdaten folgen dem folgenden Pfad:

Filebeat / Winlogbeat → Logstash → Elasticsearch → Kibana


Das zugehörige **Mermaid-Diagramm** befindet sich unter [`architecture/datenfluss.mmd`](../architecture/datenfluss.mmd).


3. Sicherheitskonzept

3.1. TLS-Verschlüsselung

Die gesamte Kommunikation zwischen den Komponenten erfolgt verschlüsselt über **SSL/TLS**, um Datenintegrität und Vertraulichkeit sicherzustellen.

- Zertifikate wurden mit dem Tool `elasticsearch-certutil` erzeugt
- Pfade zu Zertifikaten sind in den jeweiligen Konfigurationsdateien (`filebeat.yml`, `winlogbeat.yml`, `logstash.conf`, `elasticsearch.yml`) hinterlegt

3.2. Authentifizierung

Elasticsearch nutzt die integrierte Benutzerverwaltung:

- Rollenbasierte Zugriffskontrolle (RBAC)
- Schutz der Kibana-Oberfläche durch Login
- Verhinderung unautorisierter API-Zugriffe


4. Konfiguration & Implementierung

4.1. Elasticsearch

- Konfiguriert mit aktivierter Authentifizierung (`xpack.security.enabled: true`)
- Speicherort für strukturierte Logs, Suchindexe und Metadaten

4.2. Logstash

- Konfigurationsdatei `logstash.conf` definiert die Input-, Filter- und Output-Blöcke
- TLS-Verbindung mit Beats
- Optionale Filter zur Formatierung der Logs

Filebeat (Linux)

```yaml

filebeat.inputs:
  - type: log
    paths:
      - /var/log/syslog
      - /var/log/auth.log

output.logstash:
  hosts: ["10.10.0.10:5044"]
  ssl.certificate_authorities: ["/etc/filebeat/certs/ca.crt"]



Winlogbeat (Windows)

```yaml

winlogbeat.event_logs:
  - name: Application
  - name: Security
  - name: System

output.logstash:
  hosts: ["10.10.0.10:5044"]
  ssl.enabled: true


5. Evaluierung & Tests

5.1. Testkomponente Ergebnis


| Testkomponente           | Ergebnis                     |
| ------------------------ | -----------------------------|
| Elasticsearch erreichbar | ✅ über Port 9200            |
| TLS-Verbindung aktiv     | ✅ mit gültigen Zertifikaten |
| Filebeat sendet Logs     | ✅ aus `/var/log/syslog`     |
| Winlogbeat sendet Events | ✅ aus „Security“ & „System“ |
| Kibana-Dashboards        | ✅ erfolgreich eingerichtet  |
| Zugriffskontrolle Kibana | ✅ Login-Seite aktiv         |


Screenshots:

Die wichtigsten Ansichten sind unter screenshots/ abgelegt:

✅ Dashboards

✅ Login-Masken

✅ Konfigurationsbeispiele

6. Herausforderungen & Lösungen

| Herausforderung                      | Lösung                                                |
| ------------------------------------ | ----------------------------------------------------- |
| Zertifikatseinbindung in Beats       | Pfade und Dateiberechtigungen sorgfältig konfiguriert |
| Winlogbeat mit Logstash über TLS     | Eigenes CA-Zertifikat und korrektes SSL-Setup         |
| Parserprobleme bei komplexen Logs    | Filter im Logstash angepasst                          |
| Visualisierung von Feldern in Kibana | Dashboards importiert oder angepasst                  |


7. Fazit & Ausblick

Dieses Projekt demonstriert erfolgreich die Implementierung einer sicheren, zentralen Log-Analyse-Plattform. Alle Kernkomponenten funktionieren wie vorgesehen und bieten eine solide Basis für weiterführende Projekte.


Mögliche Erweiterungen:

✅ Alerting mit Elastalert oder Kibana Alerting Rules

✅ Integration von Metricbeat für Systemmetriken

✅ Langzeitarchivierung in Cold Storage

✅ Einbindung in ein Security Information and Event Management (SIEM)


Autor
Kamal Najib
Fachinformatiker Systemintegration 

Letzte Aktualisierung: Juli 2025