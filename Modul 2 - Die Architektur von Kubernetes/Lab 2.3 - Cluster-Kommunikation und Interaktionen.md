
# Kubernetes Schulung: Cluster-Kommunikation und Interaktionen

## Übersicht

In dieser Schulung behandeln wir die verschiedenen Kommunikationswege und Interaktionen, die in einem Kubernetes-Cluster stattfinden. Kubernetes ist ein verteiltes System, in dem verschiedene Komponenten miteinander kommunizieren müssen, um sicherzustellen, dass Workloads effizient bereitgestellt und verwaltet werden.

## Inhalt

1. **Cluster-Kommunikationsmodelle**
    - **Interne Kommunikation**: Kommunikation zwischen Pods innerhalb des Clusters.
    - **Externe Kommunikation**: Kommunikation von außerhalb des Clusters zu den Anwendungen innerhalb des Clusters.
    - **Service Discovery**: Kubernetes bietet eingebaute Unterstützung für die automatische Erkennung von Diensten im Cluster.

2. **Interaktionen zwischen Komponenten**
    - **Kube-apiserver**: Zentraler Verwaltungspunkt des Clusters, der Anfragen entgegen nimmt und an die entsprechenden Komponenten weiterleitet.
    - **Kubelet**: Der Agent, der auf jedem Worker Node läuft und die Kommunikation mit dem apiserver und den Containern verwaltet.
    - **Etcd**: Kommuniziert mit dem apiserver und speichert den aktuellen Zustand des Clusters.

3. **Interne Cluster-Kommunikation**
    - Kubernetes verwendet ein flaches Netzwerkmodell, was bedeutet, dass alle Pods in einem Cluster in der Lage sind, direkt miteinander zu kommunizieren, ohne NAT.
    - **Beispiel für Pod-zu-Pod-Kommunikation**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: busybox-pod
      spec:
        containers:
        - name: busybox
          image: busybox
          command:
          - sleep
          - "3600"
      ```

      Verwenden Sie den Befehl, um die IP-Adresse eines anderen Pods herauszufinden und direkt zu kommunizieren:
      ```bash
      kubectl exec -it busybox-pod -- ping <other-pod-ip>
      ```

4. **Services und Service Discovery**
    - **ClusterIP Service**: Standarddiensttyp, der eine interne IP-Adresse vergibt, um Pods untereinander zu verbinden.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: internal-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 8080
        type: ClusterIP
      ```

    - **NodePort Service**: Ermöglicht den Zugriff auf einen Dienst von außerhalb des Clusters.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: external-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 8080
        type: NodePort
      ```

    - **LoadBalancer Service**: Erstellt einen extern zugänglichen Load Balancer.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: loadbalancer-service
      spec:
        selector:
          app: my-app
        ports:
        - protocol: TCP
          port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

5. **Ingress Controller**
    - Ingress ermöglicht es, HTTP- und HTTPS-Anfragen von außerhalb des Clusters zu verwalten und an die entsprechenden Dienste im Cluster weiterzuleiten. Ingress ist für komplexe Routing-Anforderungen sehr nützlich.
    - **Ingress-Beispiel**:
      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: example-ingress
      spec:
        rules:
        - host: "example.com"
          http:
            paths:
            - path: "/"
              pathType: Prefix
              backend:
                service:
                  name: my-service
                  port:
                    number: 80
      ```

6. **Externe Kommunikation und Sicherheitsaspekte**
    - **Network Policies**: Ermöglicht es, den Datenverkehr in und aus Pods zu steuern, ähnlich wie Firewalls.
      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: NetworkPolicy
      metadata:
        name: allow-specific-traffic
      spec:
        podSelector:
          matchLabels:
            app: my-app
        policyTypes:
        - Ingress
        ingress:
        - from:
          - podSelector:
              matchLabels:
                app: allowed-app
      ```

## Fazit

Die Kommunikationswege und Interaktionen in einem Kubernetes-Cluster sind entscheidend für den Erfolg eines jeden Deployments. Das Verständnis dieser Mechanismen hilft, hochverfügbare und sichere Anwendungen in Kubernetes bereitzustellen.

## Weiterführende Links

- [Kubernetes Networking Dokumentation](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
- [Service und Networking in Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/)
