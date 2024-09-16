
# Umgang mit Konfigurationsdaten: ConfigMaps und Secrets

## Was sind ConfigMaps und Secrets?

In Kubernetes werden **ConfigMaps** und **Secrets** verwendet, um Konfigurationsdaten von der Anwendung zu trennen. Dies verbessert die Flexibilität und Sicherheit, da sensible Informationen wie Passwörter oder API-Schlüssel (Secrets) und allgemeine Konfigurationsdaten (ConfigMaps) zentral verwaltet werden können.

- **ConfigMaps**: Dienen der Speicherung von nicht vertraulichen Konfigurationsdaten in Key-Value-Paaren.
- **Secrets**: Dienen der sicheren Speicherung vertraulicher Informationen, wie Passwörtern, Zertifikaten oder Tokens.

## Inhalt

1. **Erstellen und Verwenden von ConfigMaps**
    - **Erstellen einer ConfigMap**:
      ```bash
      kubectl create configmap app-config --from-literal=APP_COLOR=blue
      ```

    - **Verwendung einer ConfigMap in einem Pod**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: app-pod
      spec:
        containers:
        - name: app-container
          image: myapp:latest
          env:
          - name: APP_COLOR
            valueFrom:
              configMapKeyRef:
                name: app-config
                key: APP_COLOR
      ```

    - **Verwendung einer ConfigMap als Datei**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: app-pod
      spec:
        containers:
        - name: app-container
          image: myapp:latest
          volumeMounts:
          - name: config-volume
            mountPath: /etc/config
        volumes:
        - name: config-volume
          configMap:
            name: app-config
      ```

2. **Erstellen und Verwenden von Secrets**
    - **Erstellen eines Secrets**:
      ```bash
      kubectl create secret generic db-secret --from-literal=username=myuser --from-literal=password=mypassword
      ```

    - **Verwendung eines Secrets in einem Pod**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: db-pod
      spec:
        containers:
        - name: db-container
          image: mydb:latest
          env:
          - name: DB_USERNAME
            valueFrom:
              secretKeyRef:
                name: db-secret
                key: username
          - name: DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: db-secret
                key: password
      ```

    - **Verwendung eines Secrets als Datei**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: db-pod
      spec:
        containers:
        - name: db-container
          image: mydb:latest
          volumeMounts:
          - name: secret-volume
            mountPath: /etc/secret
        volumes:
        - name: secret-volume
          secret:
            secretName: db-secret
      ```

3. **Sicherheitsaspekte**
    - **Secrets** werden in Base64 codiert gespeichert, aber es ist wichtig, zusätzliche Sicherheitsmaßnahmen wie **Role-Based Access Control (RBAC)** und Verschlüsselung zu implementieren, um vertrauliche Daten zu schützen.
    - Kubernetes bietet die Möglichkeit, Secrets im Ruhezustand (at rest) zu verschlüsseln. Dies sollte in produktiven Umgebungen aktiviert werden.

4. **Best Practices**
    - Trenne Konfigurationsdaten von der Anwendung.
    - Vermeide die direkte Einbettung von sensiblen Daten in Pod-Definitionen.
    - Verwende **ConfigMaps** für allgemeine Konfigurationsdaten und **Secrets** für sensible Daten.
    - Überwache den Zugriff auf Secrets durch den Einsatz von RBAC.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation zu ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
- [Offizielle Kubernetes Dokumentation zu Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
