
# CSI (Container Storage Interface) und Storage-Backends Schulung

## Was ist CSI?

CSI (Container Storage Interface) ist ein Standard, der es ermöglicht, dass Container-Orchestratoren wie Kubernetes auf verschiedene Speicherlösungen zugreifen können. Der CSI-Standard abstrahiert die Interaktionen mit dem Speicher und bietet einheitliche Schnittstellen für alle Arten von Storage-Backends.

## Inhalt

1. **CSI Architektur**
    - **Container Orchestrator**: Kubernetes fungiert als Orchestrator, der Speicheranforderungen über den CSI-Treiber an das zugrunde liegende Speicher-Backend weiterleitet.
    - **CSI Treiber**: Ein Softwaremodul, das von Speicheranbietern entwickelt wird, um ihre Systeme an Kubernetes anzubinden.
    - **External Provisioner**: Erstellt PersistentVolumeClaims (PVC) und Provisionierung von PersistentVolumes (PV).
    - **External Attacher**: Verbindet PVs mit den Workloads.
    - **External Resizer**: Passt die Größe von Volumes dynamisch an.
    - **External Snapshotter**: Ermöglicht das Erstellen von Snapshots eines Volumes.

2. **Vorteile von CSI**
    - **Unabhängigkeit vom Container-Orchestrator**: CSI funktioniert mit verschiedenen Container-Orchestratoren, nicht nur Kubernetes.
    - **Flexibilität**: Es können verschiedene Speicherlösungen integriert werden, von lokalen Speichern bis hin zu Cloud-Storage-Lösungen.
    - **Skalierbarkeit**: CSI unterstützt dynamische Skalierung und Verwaltung von Speicher, basierend auf den Anforderungen der Workloads.

3. **Praktische Anwendung**
    - **Erstellen eines Persistent Volume Claims (PVC)**:
      ```yaml
      apiVersion: v1
      kind: PersistentVolumeClaim
      metadata:
        name: my-pvc
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 10Gi
        storageClassName: my-storage-class
      ```

    - **Erstellen eines CSI Storage Class**:
      ```yaml
      apiVersion: storage.k8s.io/v1
      kind: StorageClass
      metadata:
        name: my-storage-class
      provisioner: csi.storage.k8s.io
      volumeBindingMode: Immediate
      reclaimPolicy: Delete
      ```

4. **Storage-Backends in Kubernetes**
    - Kubernetes unterstützt verschiedene Storage-Backends über den CSI-Treiber. Einige der beliebtesten Backends sind:
      - **AWS EBS** (Elastic Block Store)
      - **Google Cloud Persistent Disk**
      - **Azure Disk**
      - **Ceph** (Distributed Storage)
      - **NFS** (Network File System)
      - **iSCSI** (Internet Small Computer System Interface)

    - Beispiel für die Konfiguration eines AWS EBS Persistent Volumes:
      ```yaml
      apiVersion: v1
      kind: PersistentVolume
      metadata:
        name: aws-ebs-pv
      spec:
        capacity:
          storage: 20Gi
        volumeMode: Filesystem
        accessModes:
        - ReadWriteOnce
        persistentVolumeReclaimPolicy: Retain
        storageClassName: aws-ebs
        csi:
          driver: ebs.csi.aws.com
          volumeHandle: vol-xxxxxxxx
      ```

5. **Skalierung und Verwaltung von Speichervolumen**
    - Speichervolumen können dynamisch skaliert und verwaltet werden, abhängig von der Workload:
      ```bash
      kubectl patch pvc my-pvc -p '{"spec": {"resources": {"requests": {"storage": "20Gi"}}}}'
      ```

6. **Monitoring und Logging**
    - CSI-Treiber bieten oft eingebaute **Monitoring**-Funktionen. Tools wie **Grafana**, **Prometheus** und spezifische Speicher-Plugins können verwendet werden, um die Performance der Speicher-Backends zu überwachen.

## Weiterführende Links

- [CSI Spezifikation](https://github.com/container-storage-interface/spec)
- [Kubernetes CSI Dokumentation](https://kubernetes-csi.github.io/docs/)
- [Ceph Distributed Storage](https://ceph.io/)
