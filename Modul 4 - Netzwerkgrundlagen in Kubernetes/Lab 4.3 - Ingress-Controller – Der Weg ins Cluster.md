
# Ingress-Controller – Der Weg ins Cluster

## Was ist ein Ingress-Controller?

Ein **Ingress-Controller** in Kubernetes ist eine Komponente, die eingehende HTTP(S)-Anfragen von außerhalb des Kubernetes-Clusters zu Diensten innerhalb des Clusters weiterleitet. Er fungiert als Gateway, das den Verkehr von außen ins Cluster lenkt, oft unter Verwendung von Regeln für Routing, Authentifizierung, TLS und Load-Balancing.

## Inhalt

1. **Kernkonzepte**
    - **Ingress**: Ein Kubernetes-Objekt, das HTTP- und HTTPS-Routen für den Datenverkehr zu Diensten innerhalb des Clusters definiert.
    - **Ingress-Controller**: Eine Komponente, die Ingress-Ressourcen in Routing-Regeln für eingehenden Datenverkehr umsetzt.
    - **Service**: Verbindet interne Pods und verteilt den Datenverkehr im Cluster.

2. **Warum Ingress verwenden?**
    - Ein Ingress ermöglicht eine einfache, zentralisierte Verwaltung des HTTP- und HTTPS-Datenverkehrs im Cluster.
    - Unterstützt Funktionen wie:
        - Load Balancing
        - SSL/TLS-Terminierung
        - Virtuelle Hosts
        - Pfadbasiertes Routing

3. **Ingress-Controller einrichten**
    Um Ingress-Ressourcen nutzen zu können, muss ein Ingress-Controller im Cluster installiert sein. Ein gängiger Controller ist **NGINX Ingress Controller**.

    Beispiel: NGINX Ingress Controller installieren:
    ```bash
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
    ```

4. **Beispiel für Ingress-Definition**
    Eine einfache Ingress-Ressource zur Weiterleitung von Anfragen an einen Dienst:
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: example-ingress
      annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
    spec:
      rules:
      - host: example.com
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: example-service
                port:
                  number: 80
    ```

5. **Anwendungsbeispiel: TLS mit Ingress**
    Das folgende Beispiel zeigt, wie Sie TLS für eine Ingress-Ressource konfigurieren können:
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: example-ingress-tls
      annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
    spec:
      tls:
      - hosts:
        - example.com
        secretName: example-tls-secret
      rules:
      - host: example.com
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: example-service
                port:
                  number: 80
    ```

6. **Monitoring und Logging**
    - Der NGINX Ingress-Controller bietet detaillierte Logs für eingehende Anfragen und kann mit **Prometheus** und **Grafana** zur Überwachung integriert werden.

7. **Load Balancing und Autoscaling**
    - Kubernetes ermöglicht es, Services und den Ingress-Controller mit Load-Balancing und Autoscaling zu betreiben, was eine bessere Verteilung des eingehenden Verkehrs ermöglicht.

## Weiterführende Links

- [Ingress-Dokumentation von Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/)
- [TLS-Zertifikate für Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)
