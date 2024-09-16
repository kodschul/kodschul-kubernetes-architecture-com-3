
# Horizontales und Vertikales Skalieren von Pods

## Einführung

In Kubernetes gibt es zwei Hauptmethoden, um Anwendungen zu skalieren: **Horizontales Skalieren** und **Vertikales Skalieren**. Beide Skalierungstechniken dienen dazu, die Leistung und Verfügbarkeit von Anwendungen in einem Kubernetes-Cluster zu verbessern.

## Horizontales Skalieren

Beim horizontalen Skalieren wird die Anzahl der Replikate eines Pods erhöht oder verringert. Dies bedeutet, dass mehrere Instanzen desselben Pods erstellt werden, um den Arbeitsaufwand zu verteilen und die Anwendung besser verfügbar zu machen.

### Beispiel: Horizontal Pod Autoscaler (HPA)

Der **Horizontal Pod Autoscaler** in Kubernetes passt automatisch die Anzahl der Pods basierend auf der CPU-Auslastung oder anderen Metriken an.

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

In diesem Beispiel wird Kubernetes angewiesen, die Anzahl der Pods des `nginx-deployment` basierend auf einer CPU-Auslastung von 80 % zu skalieren.

### Manuelles horizontales Skalieren

Man kann die Anzahl der Pods auch manuell skalieren:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

## Vertikales Skalieren

Beim vertikalen Skalieren wird die Ressourcenzuweisung eines einzelnen Pods erhöht oder verringert. Dies bedeutet, dass die CPU- und Speicherressourcen, die einem Pod zugewiesen sind, angepasst werden.

### Beispiel: Vertical Pod Autoscaler (VPA)

Der **Vertical Pod Autoscaler** passt die Ressourcenanforderungen eines Pods basierend auf dessen tatsächlichem Ressourcenverbrauch an.

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: nginx-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       nginx-deployment
  updatePolicy:
    updateMode: "Auto"
```

In diesem Beispiel überwacht der Vertical Pod Autoscaler den Ressourcenverbrauch der Pods und passt die Ressourcenzuweisung automatisch an.

## Praktische Tipps

- Das horizontale Skalieren eignet sich gut für Anwendungen, die leicht parallelisiert werden können, wie Webanwendungen.
- Das vertikale Skalieren ist sinnvoll, wenn ein einzelner Pod mehr Ressourcen benötigt, um effizient zu arbeiten, ohne zusätzliche Pods zu erstellen.
- Die Verwendung von HPA und VPA kann gemeinsam erfolgen, um sowohl die Anzahl der Pods als auch deren Ressourcenanforderungen dynamisch anzupassen.

## Skalierung testen

Um die Skalierung in einer Testumgebung zu prüfen, kann man eine Simulation hoher Last durchführen und sehen, wie der Horizontal Pod Autoscaler darauf reagiert:

```bash
kubectl run -i --tty load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://nginx-service; done
```

## Weiterführende Links

- [Horizontal Pod Autoscaler Dokumentation](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Vertical Pod Autoscaler Dokumentation](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)
