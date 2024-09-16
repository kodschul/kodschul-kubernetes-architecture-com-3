
# Grundlegende Netzwerkprinzipien in Kubernetes

## Übersicht

In Kubernetes spielt das Netzwerk eine entscheidende Rolle bei der Kommunikation zwischen Pods, Diensten und externen Clients. Die Netzwerkarchitektur von Kubernetes ermöglicht es den verschiedenen Komponenten eines Clusters, nahtlos miteinander zu kommunizieren, und unterstützt dabei eine flexible Skalierbarkeit und Ausfallsicherheit.

## Inhalt

1. **Grundlagen der Kubernetes-Netzwerkarchitektur**
    - Jeder Pod erhält eine eigene **IP-Adresse**, und es wird vorausgesetzt, dass Pods untereinander direkt kommunizieren können, unabhängig davon, auf welchem Node sie laufen.
    - Kubernetes verwendet ein **Flat Networking** Modell, bei dem alle Pods in einem Cluster in einem gemeinsamen, flachen Netzwerk miteinander kommunizieren können.

2. **Netzwerkkomponenten**
    - **Pod-to-Pod Kommunikation**: Pods können über ihre IP-Adressen direkt miteinander kommunizieren.
    - **Pod-to-Service Kommunikation**: Dienste bieten eine stabile IP-Adresse und DNS-Namen für Pods an, um den Zugriff auf Pods zu erleichtern, auch wenn diese dynamisch skaliert werden.
    - **Service Discovery**: Kubernetes bietet ein internes DNS, das es ermöglicht, Pods anhand von Dienstenamen zu finden.

3. **Praktische Anwendung**
    - **Erstellen eines Pods mit einer Netzwerkverbindung**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: nginx-pod
      spec:
        containers:
        - name: nginx
          image: nginx:latest
          ports:
          - containerPort: 80
      ```

    - **Erstellen eines ClusterIP Service**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: nginx-service
      spec:
        selector:
          app: nginx
        ports:
        - protocol: TCP
          port: 80
          targetPort: 80
        type: ClusterIP
      ```

    - **NodePort Service für externen Zugriff**:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: nginx-nodeport
      spec:
        selector:
          app: nginx
        ports:
        - protocol: TCP
          port: 80
          targetPort: 80
        type: NodePort
      ```

4. **Netzwerk-Plug-ins**
    - Kubernetes unterstützt verschiedene **CNI (Container Network Interface)** Plug-ins, um Netzwerkrichtlinien umzusetzen und den Netzwerkverkehr zwischen Pods zu steuern.
    - Bekannte CNI-Plug-ins:
        - **Flannel**
        - **Calico**
        - **Weave**
        - **Cilium**

5. **Netzwerksicherheitsrichtlinien (Network Policies)**
    - **Network Policies** ermöglichen es Administratoren, den Netzwerkzugriff auf Pods zu steuern.
    - Beispiel für eine **Netzwerkpolicy**, die nur den eingehenden Verkehr von bestimmten Pods erlaubt:
      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: NetworkPolicy
      metadata:
        name: allow-nginx
      spec:
        podSelector:
          matchLabels:
            app: nginx
        ingress:
        - from:
          - podSelector:
              matchLabels:
                app: allowed-pod
      ```

6. **Service Mesh**
    - Ein **Service Mesh** wie **Istio** oder **Linkerd** kann hinzugefügt werden, um fortschrittliche Netzwerkfeatures wie **Lastverteilung**, **Verschlüsselung** und **Fehlerbehebung** bereitzustellen.

## Weiterführende Links

- [Kubernetes Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
- [CNI Plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)
- [Istio Service Mesh](https://istio.io/)

