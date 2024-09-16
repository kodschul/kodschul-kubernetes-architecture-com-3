
# Fehlerbehebung von Anwendungen und Pods in Kubernetes

## Einführung

In Kubernetes kann es bei der Bereitstellung und Verwaltung von Anwendungen zu verschiedenen Problemen kommen, die die Funktionalität beeinträchtigen. Die Fehlersuche und Behebung ist eine wichtige Fähigkeit, um sicherzustellen, dass Ihre Anwendungen reibungslos laufen.

## Inhalt

1. **Grundlegende Kommandos zur Fehlersuche**
    - **kubectl get**: Zeigt eine Übersicht der Ressourcen an.
      ```bash
      kubectl get pods
      kubectl get services
      kubectl get deployments
      ```
    - **kubectl describe**: Detaillierte Informationen zu einem Pod oder einer Ressource.
      ```bash
      kubectl describe pod <pod-name>
      ```

    - **kubectl logs**: Zeigt die Logs eines Containers an.
      ```bash
      kubectl logs <pod-name>
      ```

    - **kubectl exec**: Führt Befehle in einem laufenden Pod aus.
      ```bash
      kubectl exec -it <pod-name> -- /bin/bash
      ```

2. **Häufige Probleme und deren Ursachen**

    - **Pod ist im "CrashLoopBackOff" Zustand**:
      Ursache: Der Container stürzt nach dem Start ab und wird wiederholt neu gestartet.
      - **Lösung**: Überprüfen Sie die Container-Logs.
        ```bash
        kubectl logs <pod-name>
        ```

    - **ImagePullBackOff**:
      Ursache: Kubernetes kann das angegebene Container-Image nicht herunterladen.
      - **Lösung**: Stellen Sie sicher, dass das Image im richtigen Repository existiert und der Container-Registry-Zugang korrekt ist.

    - **Pod Pending**:
      Ursache: Der Pod wartet darauf, einem Node zugewiesen zu werden.
      - **Lösung**: Prüfen Sie, ob genügend Ressourcen (CPU, RAM) im Cluster vorhanden sind.
        ```bash
        kubectl describe pod <pod-name>
        ```

    - **Network Issues**:
      Ursache: Der Pod kann nicht mit anderen Diensten kommunizieren.
      - **Lösung**: Prüfen Sie die Netzwerkkonfiguration und Services.
        ```bash
        kubectl get services
        kubectl describe service <service-name>
        ```

3. **Tipps zur Fehlersuche**
    - Verwenden Sie das **kubectl describe** Kommando, um detaillierte Informationen zu Events und Fehlern zu erhalten.
    - Setzen Sie **kubectl logs** ein, um Anwendungslogs zu überprüfen, die wertvolle Hinweise auf Fehler im Container liefern können.
    - Falls der Fehler bei der Bereitstellung liegt, nutzen Sie **kubectl get events**, um die Reihenfolge der Ereignisse zu analysieren.

4. **Erweiterte Debugging Techniken**
    - **Debugging mit einem temporären Pod**:
      Falls Sie ein Problem im Cluster selbst debuggen möchten, können Sie einen temporären Pod starten.
      ```bash
      kubectl run debug-pod --image=busybox --restart=Never -- /bin/sh
      ```

    - **Liveness und Readiness Probes**:
      Falls ein Pod regelmäßig abstürzt oder nicht erreichbar ist, sollten Sie die Konfiguration der Liveness und Readiness Probes überprüfen.
      ```yaml
      livenessProbe:
        httpGet:
          path: /healthz
          port: 8080
        initialDelaySeconds: 3
        periodSeconds: 3
      ```

5. **Best Practices für das Debugging in Kubernetes**
    - Verwenden Sie **Namespaces**, um Ihre Umgebung zu isolieren und Fehler einfacher zu identifizieren.
    - Stellen Sie sicher, dass Ihre **Monitoring-Tools** wie Prometheus oder Grafana korrekt eingerichtet sind.
    - Implementieren Sie **Liveness** und **Readiness Probes**, um frühzeitig zu erkennen, wenn Container nicht ordnungsgemäß funktionieren.
    - Nutzen Sie **Resource Limits** (CPU, RAM) in Ihrer Pod-Konfiguration, um zu vermeiden, dass Pods aufgrund von Ressourcenknappheit fehlschlagen.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation - Debugging Pods](https://kubernetes.io/docs/tasks/debug/debug-application/)
- [Kubernetes Github Repository](https://github.com/kubernetes/kubernetes)
- [Prometheus Monitoring für Kubernetes](https://prometheus.io/)

