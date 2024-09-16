
# Autoscaling in Kubernetes

## Was ist Autoscaling?

Autoscaling in Kubernetes bezieht sich auf die Fähigkeit, die Anzahl der Pods, die Ressourcen in einem Cluster oder die Clustergröße automatisch basierend auf der aktuellen Last zu skalieren. Dies stellt sicher, dass Anwendungen die benötigten Ressourcen dynamisch erhalten, um Ausfälle oder Überlastungen zu vermeiden, und dass ungenutzte Ressourcen reduziert werden, um Kosten zu sparen.

## Inhalt

1. **Arten von Autoscaling in Kubernetes**
    - **Horizontal Pod Autoscaler (HPA)**: Skaliert die Anzahl der Pods basierend auf der CPU-Auslastung oder anderen benutzerdefinierten Metriken.
    - **Vertical Pod Autoscaler (VPA)**: Passt die Ressourcenanforderungen eines Pods, wie CPU und RAM, basierend auf der aktuellen Auslastung an.
    - **Cluster Autoscaler**: Skaliert die Anzahl der Knoten im Cluster basierend auf den Anforderungen der Pods und den verfügbaren Ressourcen.

2. **Horizontal Pod Autoscaler (HPA)**
    - Der HPA passt die Anzahl der Pods dynamisch an, basierend auf Metriken wie CPU- oder Speicherauslastung.
    - Beispiel für die Erstellung eines HPA:
      ```bash
      kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=1 --max=10
      ```

      In diesem Beispiel wird die Anzahl der Pods des `nginx-deployment` zwischen 1 und 10 dynamisch angepasst, abhängig von der CPU-Auslastung, die über 50% liegt.

    - Beispiel YAML-Datei für HPA:
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
        targetCPUUtilizationPercentage: 50
      ```

3. **Vertical Pod Autoscaler (VPA)**
    - Der VPA passt die Ressourcenanforderungen eines Pods (CPU und Speicher) automatisch an.
    - Beispiel für die Verwendung eines VPA:
      ```yaml
      apiVersion: autoscaling.k8s.io/v1
      kind: VerticalPodAutoscaler
      metadata:
        name: nginx-vpa
      spec:
        targetRef:
          apiVersion: "apps/v1"
          kind:       "Deployment"
          name:       "nginx-deployment"
        updatePolicy:
          updateMode: "Auto"
      ```

4. **Cluster Autoscaler**
    - Der Cluster Autoscaler skaliert die Anzahl der Knoten basierend auf der Pod-Auslastung. Wenn es nicht genügend Ressourcen gibt, um Pods auszuführen, wird ein neuer Knoten hinzugefügt. Wenn Ressourcen im Überfluss vorhanden sind, werden ungenutzte Knoten entfernt.

    - Beispielkonfiguration für den Cluster Autoscaler:
      ```yaml
      apiVersion: cluster.k8s.io/v1alpha1
      kind: ClusterAutoscaler
      metadata:
        name: default-cluster-autoscaler
      spec:
        scaleDown:
          enabled: true
        scaleUp:
          enabled: true
      ```

5. **Monitoring und Metriken für Autoscaling**
    - Tools wie **Prometheus** und **Kubernetes Metrics Server** sind essenziell, um Metriken zu sammeln, die für das Autoscaling verwendet werden.
    - Beispiel für die Installation des Metrics Servers:
      ```bash
      kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
      ```

## Weiterführende Links

- [Horizontal Pod Autoscaler - Kubernetes Documentation](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Vertical Pod Autoscaler - Kubernetes Documentation](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)
- [Cluster Autoscaler - Kubernetes Documentation](https://github.com/kubernetes/autoscaler)
