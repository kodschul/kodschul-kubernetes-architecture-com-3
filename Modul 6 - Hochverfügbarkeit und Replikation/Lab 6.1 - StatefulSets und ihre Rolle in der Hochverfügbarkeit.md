
# Kubernetes StatefulSets und ihre Rolle in der Hochverfügbarkeit

## Was ist ein StatefulSet?

Ein StatefulSet ist ein Kubernetes-Controller, der die Verwaltung von zustandsbehafteten Anwendungen ermöglicht. Während Deployments und ReplicaSets für zustandslose Anwendungen verwendet werden, kümmert sich StatefulSet um den Einsatz und die Verwaltung von Pods, die einzigartig sind und ihren Zustand beibehalten müssen. StatefulSets bieten jedem Pod eine stabile Netzwerkidentität, einen stabilen Speicher und eine vorhersehbare Reihenfolge beim Start und Beenden.

## Rolle von StatefulSets in der Hochverfügbarkeit

1. **Stabile Identität**:
    - Jeder Pod in einem StatefulSet erhält eine eindeutige, stabile Identität in Form eines Namens und einer IP-Adresse. Diese Identität bleibt bestehen, selbst wenn der Pod neu gestartet wird.

2. **Stabiler Speicher**:
    - Jeder Pod in einem StatefulSet hat einen eigenen PersistentVolume, das ihm zugeordnet ist. Selbst wenn der Pod zerstört wird und neu erstellt wird, bleibt sein Speicher bestehen.

3. **Geordnete Skalierung und Upgrades**:
    - Pods in einem StatefulSet werden in einer geordneten Reihenfolge erstellt und aktualisiert. Das ist wichtig für Anwendungen, die eine bestimmte Startreihenfolge benötigen, wie Datenbanken oder verteilte Systeme.

## Praktische Anwendung

1. **Erstellen eines StatefulSets für eine MySQL-Datenbank**:
    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: mysql
    spec:
      serviceName: "mysql"
      replicas: 3
      selector:
        matchLabels:
          app: mysql
      template:
        metadata:
          labels:
            app: mysql
        spec:
          containers:
          - name: mysql
            image: mysql:5.7
            ports:
            - containerPort: 3306
            volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumeClaimTemplates:
      - metadata:
          name: mysql-persistent-storage
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi
    ```

2. **Service für StatefulSet**:
    - Ein Service kann verwendet werden, um eine stabile Netzwerkverbindung zu einem Pod im StatefulSet herzustellen:
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: mysql
    spec:
      ports:
      - port: 3306
      clusterIP: None
      selector:
        app: mysql
    ```

## Hochverfügbarkeit durch StatefulSets

1. **Datenreplikation**:
    - StatefulSets in Kombination mit einem verteilten Dateisystem (wie GlusterFS oder Ceph) oder einer Datenbank (wie MySQL, Cassandra) können zur Erreichung von Hochverfügbarkeit beitragen, indem sie sicherstellen, dass Daten über mehrere Replikate hinweg synchronisiert werden.

2. **Failover und Wiederherstellung**:
    - Dank der stabilen Identität und des persistenten Speichers kann ein Pod bei einem Ausfall automatisch wiederhergestellt werden, ohne den Zustand zu verlieren. Das ist besonders wichtig für Anwendungen, die auf Konsistenz angewiesen sind, wie Datenbanken oder verteilte Systeme.

3. **Skalierung von StatefulSets**:
    - StatefulSets unterstützen das manuelle Skalieren der Anzahl der Replikate, um die Verfügbarkeit und Redundanz zu erhöhen:
      ```bash
      kubectl scale statefulset mysql --replicas=5
      ```

## Monitoring und Logging

- Monitoring-Tools wie **Prometheus** und **Grafana** sind nützlich für das Überwachen des Zustands von StatefulSets und der darunterliegenden Speicherinfrastruktur.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
- [StatefulSets und Hochverfügbarkeit](https://kubernetes.io/blog/2017/03/statefulset-run-scale-stateful-applications-in-kubernetes/)
- [Prometheus Monitoring](https://prometheus.io/)

