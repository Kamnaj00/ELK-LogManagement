# 🔍 ELK Log Management Projekt

## 🧾 Projektbeschreibung

Dieses Projekt realisiert ein zentrales Log-Management-System auf Basis des **ELK-Stacks** (Elasticsearch, Logstash, Kibana) mit Unterstützung von **Beats** (Filebeat & Winlogbeat). Ziel ist es, Systemereignisse zentral zu erfassen, zu analysieren und visuell darzustellen. Das Setup wurde in einer virtualisierten Umgebung mit **Proxmox** realisiert.

---

## 🏗️ Projektarchitektur

### 🔧 Infrastruktur:
- **Hypervisor:** Proxmox VE
- **Container:**  
  - `elk-container` mit Elasticsearch + Kibana
- **Virtuelle Maschinen (VMs):**  
  - Ubuntu-VM mit Filebeat  
  - Windows Server VM mit Winlogbeat

## Datenfluss

```mermaid
graph TD
  Windows[Windows Server + Winlogbeat] -->|Logs| Elasticsearch
  Ubuntu[Ubuntu VM + Filebeat] -->|Logs| Elasticsearch
  Elasticsearch --> Kibana[Kibana Dashboard]

# Status
✅ Elasticsearch läuft  
✅ Filebeat sendet Logs  
✅ Winlogbeat funktioniert  
✅ Dashboards in Kibana eingerichtet


