# Dispatch City – Distributed Systems Lab

Projektarbeit im Modul **Verteilte Systeme & Containerisierung** an der TEKO.

Dispatch City ist eine verteilte Food-Delivery-Simulation, die schrittweise von einer einfachen Kubernetes-Anwendung zu einer ereignisgesteuerten und persistenten Architektur erweitert wurde.

## Projektaufbau

Die Anwendung besteht unter anderem aus:

- Dashboard – Visualisierung der simulierten Stadt
- Control API – REST-, SSE-, Health- und Metrics-Endpunkte
- RabbitMQ – Event-basierte Kommunikation
- Worker – Verarbeitung der fachlichen Events
- PostgreSQL / CloudNativePG – persistente Datenhaltung
- Traefik – Ingress und Routing
- Grafana – Monitoring und Visualisierung
- Kubernetes / k3d – Container-Orchestrierung

Die Kubernetes-Konfiguration wird mit **Kustomize** verwaltet.

## Unterrichtsblöcke

Die Lösung wurde schrittweise erweitert.

| Block | Inhalt |
|---|---|
| 03 | Kubernetes Foundation |
| 04 | Ingress und Load Balancing |
| 05 | RabbitMQ und Messaging |
| 06 | PostgreSQL / CloudNativePG und Persistenz |
| 07 | Resilienz, Skalierung und Observability |

Die entsprechenden Kubernetes-Stände befinden sich unter `deploy/overlays/`.

## Voraussetzungen

Für die lokale Ausführung werden benötigt:

- Docker Desktop
- kubectl
- k3d
- Git
- Helm
- PowerShell
- Go
- Node.js / npm

Docker Desktop muss gestartet sein.

## Kubernetes-Cluster

Verwendeter Cluster:

```powershell
k3d cluster create teko-k8s --agents 2
kubectl config use-context k3d-teko-k8s
