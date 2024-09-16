
# Kubernetes und Continuous Integration (CI) Schulung

## Einführung in Continuous Integration (CI) mit Kubernetes

Continuous Integration (CI) ist eine Entwicklungspraktik, bei der Entwickler ihren Code häufig in einem zentralen Repository zusammenführen. Jedes Mal, wenn Code integriert wird, wird er automatisch gebaut und getestet. Kubernetes ist eine ausgezeichnete Plattform, um CI-Pipelines zu orchestrieren, da es die Skalierung und Automatisierung von Builds und Tests in einer containerisierten Umgebung ermöglicht.

## Inhalt

1. **Was ist Continuous Integration (CI)?**
    - CI ermöglicht es Entwicklern, Codeänderungen automatisch zu integrieren, zu testen und zu validieren, bevor er in die Produktionsumgebung gelangt.
    - Die CI-Pipeline stellt sicher, dass der Code stabil ist und keine Fehler in den Integrationen auftreten.

2. **Kubernetes als CI-Plattform**
    - **Vorteile der Nutzung von Kubernetes für CI**:
      - **Skalierbarkeit**: Kubernetes kann Tests und Builds parallel ausführen, was die Zeit zur Fertigstellung verkürzt.
      - **Isolation**: Jeder Build- und Testprozess läuft in isolierten Containern.
      - **Wiederholbarkeit**: Jeder Build wird in derselben Umgebung ausgeführt, was konsistente Ergebnisse gewährleistet.

3. **Beispiel für eine CI-Pipeline mit Kubernetes und Jenkins**
    - **Jenkins Pipeline YAML für Kubernetes**:
      ```yaml
      apiVersion: batch/v1
      kind: Job
      metadata:
        name: jenkins-build-job
      spec:
        template:
          spec:
            containers:
            - name: jenkins
              image: jenkins/jenkins:lts
              command: ["jenkins"]
              args: ["build"]
            restartPolicy: Never
      ```

    - **Integration eines Jenkins Agents auf Kubernetes**:
      Jenkins kann in Kubernetes integriert werden, um Builds in Kubernetes-Containern auszuführen:
      ```bash
      kubectl apply -f jenkins-build-job.yaml
      ```

    - **Verwendung von Jenkins zur Bereitstellung von Containern in Kubernetes**:
      ```groovy
      pipeline {
        agent any
        stages {
          stage('Build') {
            steps {
              script {
                kubernetesDeploy(configs: 'kubernetes/deployment.yaml', kubeconfigId: 'kube-config')
              }
            }
          }
        }
      }
      ```

4. **CI/CD Tools in Kubernetes**
    - **Jenkins**: Eine weitverbreitete Open-Source-Automatisierungsplattform, die in Kubernetes läuft und für CI/CD genutzt wird.
    - **Tekton**: Ein Kubernetes-natives Framework zur Erstellung von CI/CD-Systemen.
    - **GitLab CI**: Unterstützt Kubernetes nativ für Continuous Integration und Deployment.

5. **Skalierbarkeit von Builds**
    - Kubernetes ermöglicht das Skalieren von CI-Umgebungen, indem mehrere **Jobs** oder **Pods** parallel laufen. Dies erhöht die Effizienz der Builds und Tests:
      ```bash
      kubectl scale deployment jenkins --replicas=5
      ```

6. **Monitoring und Fehlersuche**
    - Mit Tools wie **Prometheus** und **Grafana** kann das CI-System überwacht werden. Dies hilft bei der Fehlerdiagnose und Optimierung der Pipeline-Leistung.

## Beispiel einer vollständigen CI-Pipeline
Hier ein vollständiges Beispiel für eine CI-Pipeline, die Jenkins nutzt, um Code zu bauen, zu testen und in Kubernetes bereitzustellen:
```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building..'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing..'
      }
    }
    stage('Deploy') {
      steps {
        script {
          kubernetesDeploy(configs: 'kubernetes/deployment.yaml', kubeconfigId: 'kube-config')
        }
      }
    }
  }
}
```

## Weiterführende Links

- [Jenkins Kubernetes Plugin](https://plugins.jenkins.io/kubernetes/)
- [Tekton CI/CD](https://tekton.dev/)
- [Kubernetes Jobs Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

