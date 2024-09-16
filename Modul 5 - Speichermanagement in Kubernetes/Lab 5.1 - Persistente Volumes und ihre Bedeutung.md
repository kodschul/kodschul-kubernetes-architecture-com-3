
# Persistente Volumes und ihre Bedeutung

## Einführung

Ein **Persistentes Volume (PV)** in Kubernetes ist eine Ressource im Cluster, die wie eine Speicherkomponente agiert. Sie ermöglicht es, Daten über den Lebenszyklus von Pods hinaus zu speichern und wiederzuverwenden. Diese Volumes sind entscheidend für Anwendungen, die einen Zustand speichern müssen, wie Datenbanken oder Dateisysteme.

## Inhalt

1. **Wichtige Konzepte**
    - **Persistent Volume (PV)**: Eine physische Speichereinheit, die vom Administrator erstellt und bereitgestellt wird.
    - **Persistent Volume Claim (PVC)**: Eine Anfrage von Benutzern, um Speicherplatz zu erhalten, der von einem PV bereitgestellt wird.
    - **Storage Class**: Definiert die Speicheranforderungen wie Geschwindigkeit, Art des Speichers (z.B. SSD, HDD) und andere Parameter.

2. **Praktische Anwendung**
    - **Erstellen eines Persistent Volumes**:
      ```yaml
      apiVersion: v1
      kind: PersistentVolume
      metadata:
        name: pv-example
      spec:
        capacity:
          storage: 1Gi
        accessModes:
          - ReadWriteOnce
        persistentVolumeReclaimPolicy: Retain
        hostPath:
          path: "/mnt/data"
      ```

    - **Erstellen eines Persistent Volume Claims**:
      ```yaml
      apiVersion: v1
      kind: PersistentVolumeClaim
      metadata:
        name: pvc-example
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
      ```

    - **Verwendung von PVC in einem Pod**:
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: nginx-pod
      spec:
        containers:
        - name: nginx
          image: nginx:latest
          volumeMounts:
          - mountPath: "/usr/share/nginx/html"
            name: html-volume
        volumes:
        - name: html-volume
          persistentVolumeClaim:
            claimName: pvc-example
      ```

3. **Zugriffstypen**
    - **ReadWriteOnce (RWO)**: Das Volume kann nur von einem Knoten gelesen und beschrieben werden.
    - **ReadOnlyMany (ROX)**: Das Volume kann von mehreren Knoten gelesen, aber nicht beschrieben werden.
    - **ReadWriteMany (RWX)**: Das Volume kann von mehreren Knoten gelesen und beschrieben werden.

4. **Wichtige Speicherstrategien**
    - **Retain**: Das Volume wird nach dem Löschen des PVC behalten.
    - **Recycle**: Das Volume wird nach dem Löschen des PVC neu formatiert.
    - **Delete**: Das Volume wird nach dem Löschen des PVC ebenfalls gelöscht.

5. **Vorteile von Persistenten Volumes**
    - **Dauerhafter Speicher**: Daten bleiben bestehen, auch wenn Pods gelöscht oder neugestartet werden.
    - **Unabhängigkeit vom Lebenszyklus des Pods**: Volumes sind vom Lebenszyklus des Pods getrennt, was Flexibilität und Datenpersistenz sicherstellt.
    - **Datenmigration**: Daten können einfach zwischen Pods oder Cluster verschoben werden.

## Weiterführende Links

- [Offizielle Kubernetes Dokumentation zu Persistenten Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [Kubernetes Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
