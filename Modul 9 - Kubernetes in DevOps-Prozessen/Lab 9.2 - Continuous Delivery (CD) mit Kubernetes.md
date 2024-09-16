
# Continuous Delivery (CD) mit Kubernetes

## Einführung in Continuous Delivery

Continuous Delivery (CD) ist eine Praxis der Softwareentwicklung, bei der Codeänderungen automatisch gebaut, getestet und zur Produktionsumgebung freigegeben werden können. Mit Kubernetes kann der gesamte Prozess der kontinuierlichen Auslieferung optimiert und automatisiert werden, was die Zuverlässigkeit und Geschwindigkeit von Releases erhöht.

## Inhalt

1. **Grundprinzipien von Continuous Delivery**
    - Automatisierung der Bereitstellung neuer Versionen
    - Vermeidung manueller Eingriffe in den Release-Prozess
    - Sicherstellung der Konsistenz über verschiedene Umgebungen (Staging, Testing, Production)

2. **Warum Kubernetes für CD verwenden?**
    - **Skalierbarkeit**: Kubernetes erleichtert die Skalierung von Diensten und ermöglicht Zero-Downtime-Deployments.
    - **Automatisierung**: Kubernetes kann in den Continuous Delivery-Prozess integriert werden, um automatisch neue Versionen von Anwendungen bereitzustellen.
    - **Self-Healing**: Kubernetes verwaltet den Zustand der Anwendungen und sorgt für automatisches Recovery im Falle eines Fehlers.

3. **Wichtige CD-Techniken mit Kubernetes**

    - **Blue/Green Deployment**:
      Ein Ansatz, bei dem eine neue Version der Anwendung (grüne Umgebung) parallel zur alten Version (blaue Umgebung) bereitgestellt wird.
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: green-deployment
      spec:
        replicas: 3
        selector:
          matchLabels:
            app: green
        template:
          metadata:
            labels:
              app: green
          spec:
            containers:
            - name: app
              image: app:v2
      ```

    - **Canary Deployment**:
      Bereitstellung einer neuen Version der Anwendung für eine kleine Benutzergruppe, um sicherzustellen, dass die neue Version stabil ist.
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: canary-deployment
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: canary
        template:
          metadata:
            labels:
              app: canary
          spec:
            containers:
            - name: app
              image: app:v2
      ```

    - **Rolling Updates**:
      Kubernetes bietet Rolling Updates als Standardmethode, um sicherzustellen, dass die Anwendung ohne Downtime aktualisiert wird.
      ```bash
      kubectl set image deployment/my-deployment app=app:v2
      ```

4. **Praktische Schritte zur Integration von CD mit Kubernetes**
    - **CI/CD Pipeline einrichten**: Mit Tools wie Jenkins, GitLab CI oder CircleCI können Build-, Test- und Deployment-Schritte automatisiert werden.
    - **Helm für Kubernetes**: Mit Helm können Anwendungen als Charts definiert und einfach in verschiedene Umgebungen ausgerollt werden.
    - **Automatisiertes Rollback**: Im Falle eines fehlgeschlagenen Deployments ermöglicht Kubernetes ein schnelles Rollback zur letzten stabilen Version.

5. **Beispiel für eine CI/CD Pipeline mit Kubernetes und Jenkins**
    - **Jenkins Pipeline Definition**:
      ```groovy
      pipeline {
        agent any
        stages {
          stage('Build') {
            steps {
              sh 'mvn clean package'
            }
          }
          stage('Deploy to Kubernetes') {
            steps {
              sh 'kubectl apply -f k8s/deployment.yaml'
            }
          }
        }
      }
      ```

6. **Monitoring und Fehlerbehebung**
    - Tools wie **Prometheus** und **Grafana** helfen bei der Überwachung von CD-Pipelines und der Bereitstellung in Kubernetes.
    - **Kubernetes Events** und Logs können für Debugging verwendet werden:
      ```bash
      kubectl logs <pod-name>
      kubectl describe pod <pod-name>
      ```

## Weiterführende Links

- [Kubernetes CD mit Jenkins](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/)
- [Helm Charts](https://helm.sh/)
- [Prometheus Monitoring](https://prometheus.io/)
