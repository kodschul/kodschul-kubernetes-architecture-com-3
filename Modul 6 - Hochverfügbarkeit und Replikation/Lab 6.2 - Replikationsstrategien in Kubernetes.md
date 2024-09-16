
# Replikationsstrategien in Kubernetes

## Überblick

In Kubernetes ermöglichen Replikationsstrategien die Verwaltung der Anzahl von Pod-Kopien, die in einem Cluster ausgeführt werden. Durch die Verwendung von Replikationscontroller, Replikationssätzen und Deployments sorgt Kubernetes für die kontinuierliche Verfügbarkeit und Skalierbarkeit von Anwendungen.

## Inhalt

1. **Replikationscontroller**
    - Der **Replikationscontroller** stellt sicher, dass zu jeder Zeit eine festgelegte Anzahl von Pods läuft. Wenn ein Pod ausfällt, startet der Replikationscontroller automatisch einen neuen.
    - Beispiel für einen Replikationscontroller:
      ```yaml
      apiVersion: v1
      kind: ReplicationController
      metadata:
        name: nginx-controller
      spec:
        replicas: 3
        selector:
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

2. **Replikationssatz (ReplicaSet)**
    - Ein **ReplicaSet** ist die neuere Version des Replikationscontrollers und wird hauptsächlich verwendet, um eine konstante Anzahl von Pod-Replikaten auszuführen.
    - Beispiel für ein ReplicaSet:
      ```yaml
      apiVersion: apps/v1
      kind: ReplicaSet
      metadata:
        name: nginx-replicaset
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

3. **Deployments**
    - **Deployments** bieten eine zusätzliche Abstraktionsschicht über ReplicaSets und ermöglichen es, die Aktualisierung, Rollback und das Skalieren von Anwendungen zu verwalten.
    - Beispiel für ein Deployment:
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

4. **Skalieren eines Deployments**
    - Kubernetes erlaubt das dynamische Skalieren von Deployments, um auf Änderungen im Ressourcenverbrauch zu reagieren:
      ```bash
      kubectl scale deployment nginx-deployment --replicas=5
      ```

5. **Rolling Updates und Rollbacks**
    - Mit einem Deployment kannst du **Rolling Updates** durchführen, um Container zu aktualisieren, ohne dass es zu Downtimes kommt:
      ```bash
      kubectl set image deployment/nginx-deployment nginx=nginx:1.17
      ```
    - **Rollback** auf eine vorherige Version:
      ```bash
      kubectl rollout undo deployment/nginx-deployment
      ```

6. **Horizontal Pod Autoscaling (HPA)**
    - Der **Horizontal Pod Autoscaler (HPA)** passt die Anzahl der laufenden Pods basierend auf Metriken wie CPU- und Speicherverbrauch an:
      ```yaml
      apiVersion: autoscaling/v1
      kind: HorizontalPodAutoscaler
      metadata:
        name: nginx-hpa
      spec:
        scaleTargetRef:
          apiVersion: apps/v1
          kind: Deployment
          name: nginx-deployment
        minReplicas: 1
        maxReplicas: 10
        targetCPUUtilizationPercentage: 80
      ```

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation](https://kubernetes.io/docs/home/)
- [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Kubernetes Deployment Tutorial](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
