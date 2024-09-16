
# Kubernetes und Container Schulung

## Was sind Container?

Container sind leichtgewichtige, standardisierte Softwarepakete, die alles enthalten, was zur Ausführung einer Anwendung erforderlich ist, einschließlich des Codes, der Laufzeit, der Bibliotheken und der Systemwerkzeuge. Im Gegensatz zu virtuellen Maschinen benötigen Container keinen kompletten Betriebssystem-Overhead, sondern teilen sich den Kernel des Host-Systems, was sie effizienter und schneller macht.

## Was ist Kubernetes?

Kubernetes ist eine Plattform zur Verwaltung containerisierter Anwendungen in einem Cluster von Maschinen. Es automatisiert viele der manuellen Prozesse, die mit der Bereitstellung und dem Betrieb von containerisierten Anwendungen verbunden sind, wie z.B. die Skalierung, Fehlerbehebung und Ausfallsicherung.

## Inhalt

1. **Container Grundlagen**
    - **Was ist ein Container?**: Container sind isolierte Umgebungen, in denen Anwendungen unabhängig von ihrer Umgebung laufen können.
    - **Docker**: Das am weitesten verbreitete Containerisierungstool, das zur Erstellung, Bereitstellung und Ausführung von Containern verwendet wird.
    - **Container Image**: Eine unveränderliche Vorlage zur Erstellung von Containern.
    - **Container Registry**: Speicherort, an dem Container-Images gespeichert und abgerufen werden können (z.B. Docker Hub).

2. **Warum Kubernetes und Container kombinieren?**
    - Container bieten eine standardisierte Umgebung, um Anwendungen auszuführen, während Kubernetes die Verwaltung und Orchestrierung dieser Container in großem Maßstab übernimmt.
    - **Automatisierung**: Kubernetes automatisiert die Bereitstellung, das Skalieren und die Verwaltung von Containern.
    - **Selbstheilung**: Kubernetes überwacht die Container und startet sie bei Ausfällen automatisch neu.
    - **Skalierbarkeit**: Kubernetes kann dynamisch Container hinzufügen oder entfernen, um den Anforderungen gerecht zu werden.

3. **Praktische Anwendung mit Kubernetes und Containern**
    - **Erstellen eines einfachen Containers**:
      ```bash
      docker run -d --name my-nginx-container -p 80:80 nginx:latest
      ```
      Dies erstellt einen laufenden NGINX-Webserver in einem Container.

    - **Erstellen eines Pods in Kubernetes, der einen Container nutzt**:
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

4. **Verwendung von Docker und Kubernetes zusammen**
    - Kubernetes kann Docker als Container-Laufzeit verwenden, um Anwendungen zu starten und zu verwalten.
    - **Erstellen eines Docker-Images und Bereitstellen in Kubernetes**:
      ```bash
      docker build -t my-app-image .
      docker push my-app-image
      ```

      Dann kann das Image in Kubernetes verwendet werden:
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: my-app-deployment
      spec:
        replicas: 2
        template:
          spec:
            containers:
            - name: my-app
              image: my-app-image
      ```

5. **Container und Kubernetes Netzwerke**
    - Kubernetes verwaltet die Netzwerke zwischen Containern und bietet automatische Service-Discovery.
    - **Service für Container-Verbindungen**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-app-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 80
        type: LoadBalancer
      ```

6. **Speicherverwaltung für Container**
    - Kubernetes ermöglicht es, Daten persistent zu speichern, die von Containern genutzt werden können, indem Volumes verwendet werden.

## Weiterführende Links

- [Docker Dokumentation](https://docs.docker.com/)
- [Kubernetes Dokumentation](https://kubernetes.io/docs/home/)
- [Docker Hub](https://hub.docker.com/)

