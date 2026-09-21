# Reflexion

## Bekannte Grenzen

Die Anwendung wurde als Lernprojekt für eine lokale Kubernetes-Umgebung mit k3d umgesetzt. Sie ist deshalb nicht als produktive Umgebung ausgelegt.

Einige Komponenten und Einstellungen wurden bewusst vereinfacht. Für einen produktiven Betrieb wären zusätzliche Massnahmen bezüglich Sicherheit, Hochverfügbarkeit, Backup und automatisiertem Deployment notwendig.

## Beobachtete Fehlerbilder

Während der Umsetzung und den Tests sind verschiedene Fehlerbilder aufgetreten.

### ImagePullBackOff

Bei einem Test wurde absichtlich ein nicht vorhandenes Container-Image (`nginx:absichtlich-falsch`) deployed. Der neue Pod konnte dadurch nicht gestartet werden und wechselte in `ErrImagePull` beziehungsweise `ImagePullBackOff`.

Die bestehenden funktionsfähigen Pods blieben verfügbar. Mit `kubectl rollout undo` konnte wieder auf den vorherigen Stand zurückgekehrt werden.

### Readiness

Bei einem simulierten Readiness-Ausfall blieb der Container zwar aktiv, der Pod war jedoch nicht mehr bereit (`0/1`). Kubernetes entfernte ihn aus den verfügbaren Service-Endpoints. Dadurch wurde kein neuer Traffic mehr an diesen Pod weitergeleitet.

### HPA und Metrics

Beim Test des Horizontal Pod Autoscalers waren die CPU-Metriken zunächst noch nicht verfügbar. Nachdem die benötigten Metriken zur Verfügung standen, konnte der HPA die CPU-Auslastung auswerten.

Unter Last stieg die CPU-Auslastung stark an und Kubernetes skalierte von zwei auf vier Pods. Nach Ende der Last wurde automatisch wieder auf zwei Pods reduziert.

### Port-Konflikte

Beim Port-Forwarding waren einzelne lokale Ports bereits belegt. Deshalb mussten teilweise alternative Ports verwendet werden, beispielsweise Port 3001 oder 3002 für Grafana.

### Dashboard

Während der Tests war das Dashboard zeitweise nicht erreichbar beziehungsweise zeigte einen Offline-Zustand. Dabei trat unter anderem ein Fehler beim Abruf von `/api/v1/snapshot` auf.

Dies zeigte, wie wichtig die Überwachung der Abhängigkeiten zwischen den einzelnen Komponenten einer verteilten Anwendung ist.

## Mögliche Verbesserungen

Für eine Weiterentwicklung des Projekts wären unter anderem folgende Verbesserungen sinnvoll:

- automatisierte Build- und Deployment-Pipeline
- automatisierte Smoke- und Integrationstests
- erweitertes Monitoring und Alerting
- sichere Verwaltung von Secrets
- NetworkPolicies zwischen den Komponenten
- regelmässige Backup- und Restore-Tests der PostgreSQL-Datenbank
- weitere Tests von Ausfall- und Failover-Szenarien
- Optimierung der Ressourcenlimits und HPA-Schwellwerte

## Fazit

Durch die schrittweise Erweiterung des Projekts wurde sichtbar, welche zusätzlichen Anforderungen bei verteilten Anwendungen entstehen. Neben der eigentlichen Anwendung spielen Messaging, Persistenz, Skalierung, Fehlertoleranz und Monitoring eine wichtige Rolle.

Besonders die praktischen Tests mit Readiness, fehlerhaften Deployments, automatischer Skalierung und Monitoring haben gezeigt, wie Kubernetes auf Fehler und Lastveränderungen reagiert.
