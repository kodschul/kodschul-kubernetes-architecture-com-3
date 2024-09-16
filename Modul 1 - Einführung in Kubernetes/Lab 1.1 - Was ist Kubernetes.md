
# Kubernetes Architektur Schulung

## Was ist Kubernetes?

Kubernetes ist eine Open-Source-Plattform zur Automatisierung der Bereitstellung, Skalierung und Verwaltung containerisierter Anwendungen. Es ermöglicht die Verwaltung großer Cluster von Maschinen, die als Knoten bezeichnet werden, und sorgt dafür, dass die Anwendungen verfügbar und stabil bleiben, auch wenn es zu Ausfällen von Maschinen oder anderen Problemen kommt.

## Inhalt

1. **Kubernetes Komponenten**
    - **Master Node**: Verwaltungsknoten, auf dem die Kontrolle über das Cluster läuft.
    - **Worker Nodes**: Hier laufen die Anwendungen und Container.
    - **Etcd**: Schlüssel-Wert-Speicher für alle Cluster-Daten.
    - **Kube-apiserver**: Schnittstelle für die Interaktion mit dem Cluster.
    - **Kube-scheduler**: Zuständig für das Scheduling der Pods auf die Worker Nodes.
    - **Kube-controller-manager**: Verwaltet die verschiedenen Controller, die für das Cluster wichtig sind.
    - **Kubelet**: Nimmt Befehle vom apiserver entgegen und führt sie auf den Worker Nodes aus.
    - **Container Runtime**: Führt Container aus, zum Beispiel Docker oder containerd.

2. **Grundlagen der Kubernetes-Architektur**
    - Kubernetes basiert auf einer **Master-Worker-Architektur**, wobei der **Master Node** die Steuerung und Verwaltung übernimmt, während die **Worker Nodes** die eigentlichen Container-Workloads ausführen.

3. **Praktische Anwendung**
    - **Erstellen eines Pods**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: nginx-pod
      spec:
        containers:
        - name: nginx
          image: nginx:latest
          ports:
          - containerPort: 80
      ```

    - **Erstellen eines Deployments**:
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: nginx-deployment
      spec:
        replicas: 3
        selector:
          matchLabels:
            app: nginx
        template:
          metadata:
            labels:
              app: nginx
          spec:
            containers:
            - name: nginx
              image: nginx:latest
              ports:
              - containerPort: 80
      ```

    - **Service für Load Balancing**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: nginx-service
      spec:
        selector:
          app: nginx
        ports:
        - protocol: TCP
          port: 80
          targetPort: 80
        type: LoadBalancer
      ```

4. **Skalierung und Verwaltung**
    - Kubernetes ermöglicht es, die Anzahl der Replikate eines Deployments dynamisch zu skalieren:
      ```bash
      kubectl scale deployment nginx-deployment --replicas=5
      ```

5. **Monitoring und Logging**
    - Kubernetes bietet eingebaute Unterstützung für **Logging** und **Monitoring**. Tools wie **Prometheus** und **Grafana** können nahtlos integriert werden, um Metriken zu sammeln und zu visualisieren.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation](https://kubernetes.io/docs/home/)
- [Kubernetes Github Repository](https://github.com/kubernetes/kubernetes)
- [Prometheus Monitoring](https://prometheus.io/)