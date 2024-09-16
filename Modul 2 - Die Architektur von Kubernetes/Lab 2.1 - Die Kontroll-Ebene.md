
# Kubernetes Architektur Schulung

## Die Kontroll-Ebene (Control Plane)

Die Kontroll-Ebene ist der zentrale Verwaltungspunkt in einem Kubernetes-Cluster. Sie sorgt für die Gesamtüberwachung des Systems, indem sie sicherstellt, dass die definierten Richtlinien durchgesetzt werden und die gewünschten Zustände der Anwendungen aufrechterhalten werden.

## Komponenten der Kontroll-Ebene

1. **Kube-apiserver**:
    - Die zentrale Management-Komponente, die alle anderen Kubernetes-Komponenten steuert.
    - Exponiert die Kubernetes API, über die Benutzer, Controller und Worker Nodes mit dem System interagieren.
    - Beispielaufruf zur Anzeige aller Nodes:
      ```bash
      kubectl get nodes
      ```

2. **Etcd**:
    - Ein verteilter, zuverlässiger Schlüssel-Wert-Speicher, der als primäre Datenquelle für den Cluster dient.
    - Speichert den gesamten Zustand des Kubernetes-Clusters.
    - Beispiel: Zum Sichern der etcd-Datenbank:
      ```bash
      etcdctl snapshot save /backup/etcd-snapshot.db
      ```

3. **Kube-scheduler**:
    - Verantwortlich für die Zuweisung von Pods zu den Worker Nodes.
    - Berücksichtigt Ressourcenanforderungen, Verfügbarkeiten und Richtlinien zur Vermeidung von Überlastung.

4. **Kube-controller-manager**:
    - Verwaltet alle wichtigen Controller im Kubernetes-Cluster, wie den **Node-Controller**, **Replication-Controller**, und **Endpoint-Controller**.
    - Beispiel zur manuellen Überwachung von Pods:
      ```bash
      kubectl get pods
      ```

## Funktionsweise der Kontroll-Ebene

Die Kontroll-Ebene überwacht den Zustand des Clusters kontinuierlich und stellt sicher, dass die gewünschte Anzahl an Pods läuft, die richtigen Services bereitgestellt werden und alle Komponenten ordnungsgemäß funktionieren. Die Kommunikation zwischen den Komponenten der Kontroll-Ebene erfolgt hauptsächlich über die Kubernetes API, die über den **Kube-apiserver** exponiert wird.

### Beispiel: Manuelles Pod-Scheduling
Man kann einen Pod direkt auf einem bestimmten Node schedulen, indem man die Node-Auswahl im Pod-Manifest festlegt:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
  nodeSelector:
    kubernetes.io/hostname: worker-node-1
```

### Überwachung des Cluster-Zustands
Ein wichtiges Ziel der Kontroll-Ebene ist die Überwachung des Zustands des Clusters und die Sicherstellung, dass das gewünschte Systemverhalten durchgesetzt wird. Dies umfasst:
- **Replikation**: Sicherstellen, dass die richtige Anzahl von Pods läuft.
- **Auto-Scaling**: Automatisches Skalieren von Anwendungen basierend auf der Ressourcenauslastung.

### Beispiel: Horizontal Pod Autoscaler
Kubernetes bietet ein integriertes Auto-Scaling-Tool, um Pods basierend auf der CPU-Auslastung zu skalieren:
```bash
kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=1 --max=10
```

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation zur Control Plane](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components)
- [Etcd Schlüssel-Wert-Speicher](https://etcd.io/)
- [Kubernetes GitHub Repository](https://github.com/kubernetes/kubernetes)