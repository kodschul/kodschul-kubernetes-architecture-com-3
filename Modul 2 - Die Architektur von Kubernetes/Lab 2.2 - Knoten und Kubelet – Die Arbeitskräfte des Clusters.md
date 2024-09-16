
# Knoten und Kubelet – Die Arbeitskräfte des Clusters

## Knoten

Ein Knoten in Kubernetes ist eine Maschine, physisch oder virtuell, die als Arbeitskraft im Cluster fungiert. Jeder Knoten führt die Pods aus, die die Anwendung und ihre abhängigen Container enthalten. Ein Cluster besteht aus mehreren Knoten, die zusammenarbeiten, um die Anwendungen auszuführen und zu skalieren.

### Komponenten eines Kubernetes Knotens:
1. **Kubelet**: Die Hauptkomponente auf jedem Knoten, verantwortlich für die Verwaltung der Pods.
2. **Container Runtime**: Führt die Container aus (z.B. Docker, containerd).
3. **Kube-proxy**: Unterstützt die Netzwerkkommunikation und Load Balancing für Pods.
4. **Pods**: Die kleinste ausführbare Einheit, in der Container laufen.

### Beispiel:
Um die Knoten im Cluster anzuzeigen, kann der folgende Befehl verwendet werden:
```bash
kubectl get nodes
```

## Kubelet

Das **Kubelet** ist eine der wichtigsten Komponenten auf einem Worker Node. Es ist für das Starten, Stoppen und Verwalten von Containern verantwortlich, die in Pods definiert sind. Es kommuniziert mit dem API-Server und führt Anweisungen basierend auf den manifestierten Pods aus.

### Aufgaben des Kubelet:
1. **Pod-Management**: Das Kubelet ist verantwortlich für das Bereitstellen und Überwachen von Pods auf einem Knoten.
2. **Status-Berichterstattung**: Kubelet berichtet regelmäßig über den Status der Pods an den API-Server.
3. **Ressourcenüberwachung**: Verwendet Tools wie cAdvisor zur Überwachung von Ressourcenverbrauch (CPU, Speicher, Netzwerkauslastung).

### Beispiel-Konfiguration eines Pods mit dem Kubelet:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: app-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

## Praktische Anwendung

1. **Pod-Status anzeigen**:
   Der Status der Pods, die von Kubelet auf den Knoten verwaltet werden, kann über den folgenden Befehl angezeigt werden:
   ```bash
   kubectl get pods
   ```

2. **Logs eines Pods anzeigen**:
   Die Logs eines bestimmten Pods können zur Fehlerbehebung hilfreich sein:
   ```bash
   kubectl logs <pod-name>
   ```

3. **Knoten-Details anzeigen**:
   Zeige die Details eines Knotens im Cluster an, um zu sehen, wie viele Pods auf diesem Knoten laufen:
   ```bash
   kubectl describe node <node-name>
   ```

## Fazit

Die Knoten und das Kubelet sind die eigentlichen Arbeitskräfte in einem Kubernetes-Cluster. Während der Knoten die Umgebung bereitstellt, in der die Pods laufen, ist das Kubelet der zentrale Dienst, der für die Verwaltung dieser Pods verantwortlich ist.

## Weiterführende Links

- [Kubernetes Node Dokumentation](https://kubernetes.io/docs/concepts/architecture/nodes/)
- [Kubelet Dokumentation](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
