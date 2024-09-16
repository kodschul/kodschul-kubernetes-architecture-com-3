
# Kubernetes Architektur Schulung

## Services – Wie Anwendungen im Cluster kommunizieren

In Kubernetes sind **Services** eine Abstraktionsebene, die es ermöglicht, eine konstante IP-Adresse und einen DNS-Namen für eine dynamische Gruppe von Pods bereitzustellen. Services bieten die Möglichkeit, den Netzwerkzugriff auf die containerisierten Anwendungen zu steuern und Lastverteilung (Load Balancing) durchzuführen.

## Inhalt

1. **Arten von Kubernetes Services**
    - **ClusterIP (Standard)**: Erzeugt eine interne IP-Adresse, die nur innerhalb des Clusters erreichbar ist.
    - **NodePort**: Öffnet einen spezifischen Port auf allen Nodes des Clusters, um externen Traffic zu einem Pod zu routen.
    - **LoadBalancer**: Erstellt einen Load Balancer für die Bereitstellung einer externen IP-Adresse.
    - **ExternalName**: Löst einen DNS-Namen auf und leitet Anfragen an den externen DNS-Namen weiter.

2. **Praktische Anwendung von Services**

    - **ClusterIP Beispiel**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-clusterip-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 9376
        type: ClusterIP
      ```

    - **NodePort Beispiel**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-nodeport-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 9376
          nodePort: 30007
        type: NodePort
      ```

    - **LoadBalancer Beispiel**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-loadbalancer-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 9376
        type: LoadBalancer
      ```

3. **Service Discovery**
    - Kubernetes stellt sicher, dass alle Pods, die einem bestimmten Service zugewiesen sind, eine DNS-Name-basierte Erreichbarkeit im Cluster haben. Jeder Service erhält eine eigene DNS-Eintragung im Cluster, z.B. `my-service.my-namespace.svc.cluster.local`.

4. **Load Balancing und Skalierung**
    - Kubernetes-Services bieten eingebaute **Load-Balancing-Funktionalitäten**, die Anfragen auf mehrere Pods verteilen.
    - Die Anzahl der Pods, die einen bestimmten Service bedienen, kann dynamisch skaliert werden. Dies funktioniert nahtlos zusammen mit der Service-Architektur.

5. **Ingress für externe Kommunikation**
    - Für komplexere Anwendungsfälle kann ein **Ingress** verwendet werden, um externen HTTP(S)-Zugriff auf das Cluster bereitzustellen:
      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: example-ingress
      spec:
        rules:
        - host: example.com
          http:
            paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                  name: my-app
                  port:
                    number: 80
      ```

6. **Fehlerbehebung**
    - Tools wie `kubectl describe service <service-name>` und `kubectl get endpoints` helfen bei der Diagnose und Überwachung von Services im Kubernetes-Cluster.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation zu Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Ingress Ressourcen in Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [CoreDNS in Kubernetes](https://coredns.io/)

