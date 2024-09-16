
# Pods und ReplicaSets – Der Kern der Kubernetes-Workloads

## Einführung

In Kubernetes sind **Pods** und **ReplicaSets** grundlegende Bausteine für das Ausführen und Skalieren von Anwendungen. Sie ermöglichen es, containerisierte Anwendungen effizient zu verwalten und eine hohe Verfügbarkeit sicherzustellen.

## Inhalt

1. **Was ist ein Pod?**
    - Ein **Pod** ist die kleinste und einfachste Einheit in Kubernetes. Ein Pod enthält einen oder mehrere Container, die zusammen ausgeführt werden. Alle Container in einem Pod teilen sich die gleichen Netzwerkressourcen und das Dateisystem.
    - Beispiel eines Pods:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: my-pod
      spec:
        containers:
        - name: my-container
          image: nginx:latest
          ports:
          - containerPort: 80
      ```

2. **Was ist ein ReplicaSet?**
    - Ein **ReplicaSet** sorgt dafür, dass zu jeder Zeit eine festgelegte Anzahl von identischen Pods im Cluster ausgeführt werden. Es überwacht die Pods und stellt sicher, dass immer die gewünschte Anzahl an Pods aktiv ist.
    - Beispiel eines ReplicaSets:
      ```yaml
      apiVersion: apps/v1
      kind: ReplicaSet
      metadata:
        name: my-replicaset
      spec:
        replicas: 3
        selector:
          matchLabels:
            app: my-app
        template:
          metadata:
            labels:
              app: my-app
          spec:
            containers:
            - name: my-container
              image: nginx:latest
              ports:
              - containerPort: 80
      ```

3. **Pod vs. ReplicaSet**
    - **Pods** sind flüchtige Entitäten. Sobald ein Pod stirbt, wird er nicht automatisch ersetzt.
    - **ReplicaSets** hingegen überwachen Pods und stellen sicher, dass immer eine bestimmte Anzahl verfügbar ist. Sollte ein Pod ausfallen, startet das ReplicaSet automatisch einen neuen.

4. **Praktische Anwendung**
    - **Erstellen eines Pods**:
      ```bash
      kubectl apply -f pod.yaml
      ```

    - **Erstellen eines ReplicaSets**:
      ```bash
      kubectl apply -f replicaset.yaml
      ```

    - **Überprüfen des Status eines ReplicaSets**:
      ```bash
      kubectl get replicaset
      ```

    - **Skalieren eines ReplicaSets**:
      ```bash
      kubectl scale replicaset my-replicaset --replicas=5
      ```

5. **Selbstheilung und Skalierbarkeit**
    - ReplicaSets sorgen für die Selbstheilung von Anwendungen, indem sie sicherstellen, dass ausgefallene Pods automatisch ersetzt werden.
    - Sie bieten auch die Möglichkeit, Anwendungen zu skalieren, indem sie die Anzahl der ausgeführten Pods erhöhen oder verringern.

6. **Strategien zur Replikation**
    - **Rolling Update**: Aktualisiert Pods Schritt für Schritt, ohne Ausfallzeit.
    - **Recreate**: Alle Pods werden beendet, bevor neue Pods erstellt werden.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
- [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
