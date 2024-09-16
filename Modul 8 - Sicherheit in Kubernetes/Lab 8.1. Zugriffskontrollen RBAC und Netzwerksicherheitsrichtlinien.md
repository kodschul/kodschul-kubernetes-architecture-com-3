
# Zugriffskontrollen: RBAC und Netzwerksicherheitsrichtlinien

## Was ist RBAC (Role-Based Access Control)?

RBAC ist ein Sicherheitsmechanismus, der die Rechte und Rollen von Benutzern in einem Kubernetes-Cluster regelt. Es stellt sicher, dass nur autorisierte Benutzer bestimmte Aktionen in bestimmten Bereichen des Clusters durchführen können.

## Inhalt

1. **Grundlagen von RBAC**
    - **Roles**: Definieren eine Sammlung von Berechtigungen (zum Beispiel Zugriff auf Ressourcen).
    - **RoleBindings**: Verknüpfen Benutzer oder Gruppen mit einer Rolle.
    - **ClusterRoles**: Ähnlich wie Roles, aber gelten für das gesamte Cluster.
    - **ClusterRoleBindings**: Verknüpfen ClusterRoles mit Benutzern oder Gruppen.

2. **Praktische Anwendung von RBAC**
    - **Erstellen einer Rolle (Role)**:
      ```yaml
      kind: Role
      apiVersion: rbac.authorization.k8s.io/v1
      metadata:
        namespace: default
        name: pod-reader
      rules:
      - apiGroups: [""]
        resources: ["pods"]
        verbs: ["get", "watch", "list"]
      ```

    - **Erstellen eines RoleBindings**:
      ```yaml
      kind: RoleBinding
      apiVersion: rbac.authorization.k8s.io/v1
      metadata:
        name: read-pods
        namespace: default
      subjects:
      - kind: User
        name: "jane"
        apiGroup: rbac.authorization.k8s.io
      roleRef:
        kind: Role
        name: pod-reader
        apiGroup: rbac.authorization.k8s.io
      ```

3. **Netzwerksicherheitsrichtlinien**
    - Kubernetes bietet **Network Policies**, um den Datenverkehr zwischen Pods zu steuern und zu regeln. Dies erhöht die Sicherheit, indem nur autorisierte Pods miteinander kommunizieren können.

4. **Praktische Anwendung von Netzwerksicherheitsrichtlinien**
    - **Erstellen einer Netzwerksicherheitsrichtlinie (Network Policy)**:
      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: NetworkPolicy
      metadata:
        name: allow-nginx
        namespace: default
      spec:
        podSelector:
          matchLabels:
            app: nginx
        policyTypes:
        - Ingress
        - Egress
        ingress:
        - from:
          - podSelector:
              matchLabels:
                app: frontend
          ports:
          - protocol: TCP
            port: 80
        egress:
        - to:
          - podSelector:
              matchLabels:
                app: backend
          ports:
          - protocol: TCP
            port: 8080
      ```

5. **Zusätzliche Sicherheitsmaßnahmen**
    - Durch die Kombination von **RBAC** und **Netzwerksicherheitsrichtlinien** kann eine robuste Sicherheitsarchitektur aufgebaut werden. Dies schützt den Zugriff auf Ressourcen und steuert den Netzwerkverkehr in und aus dem Cluster.

## Weiterführende Links

- [Kubernetes RBAC Dokumentation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [Best Practices für Kubernetes Sicherheit](https://kubernetes.io/docs/concepts/security/overview/)
