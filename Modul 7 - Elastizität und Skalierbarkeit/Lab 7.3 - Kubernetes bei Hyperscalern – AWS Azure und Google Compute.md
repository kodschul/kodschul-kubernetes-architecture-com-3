
# Kubernetes bei Hyperscalern – AWS, Azure und Google Compute

## Einführung

Kubernetes ist eine beliebte Plattform für die Orchestrierung containerisierter Anwendungen, und viele Unternehmen setzen auf Hyperscaler wie AWS, Azure und Google Cloud, um Kubernetes in der Cloud zu betreiben. Diese Cloud-Anbieter bieten vollständig verwaltete Kubernetes-Dienste, die Entwicklern und Betreibern die Verwaltung von Infrastruktur und Containern erleichtern.

## Hyperscaler und Kubernetes

### 1. **Amazon Elastic Kubernetes Service (EKS) – AWS**
   - **EKS** ist der verwaltete Kubernetes-Dienst von AWS. Er bietet eine vollständig verwaltete Umgebung für den Betrieb von Kubernetes-Clustern.
   - Vorteile:
     - Integriert mit anderen AWS-Diensten wie EC2, IAM, RDS, usw.
     - Hohe Verfügbarkeit durch mehrere Availability Zones (AZs).
     - Automatisierte Cluster-Upgrades und Patching.
     - **Beispiel: Erstellen eines EKS-Clusters**:
       ```bash
       eksctl create cluster --name my-eks-cluster --region us-west-2 --nodes 3
       ```

### 2. **Azure Kubernetes Service (AKS) – Microsoft Azure**
   - **AKS** ist der verwaltete Kubernetes-Dienst von Microsoft Azure, der Entwicklern die Möglichkeit gibt, Anwendungen zu skalieren und zu verwalten.
   - Vorteile:
     - Integriert mit Azure-Diensten wie Azure Monitor, Azure Active Directory (AAD), etc.
     - Automatisiertes Patching und Updates für den Kubernetes-Cluster.
     - Unterstützt native CI/CD-Integrationen mit Azure DevOps.
     - **Beispiel: Erstellen eines AKS-Clusters**:
       ```bash
       az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 3 --enable-addons monitoring --generate-ssh-keys
       ```

### 3. **Google Kubernetes Engine (GKE) – Google Cloud**
   - **GKE** ist der verwaltete Kubernetes-Dienst von Google Cloud und einer der ersten Cloud-Dienste, die Kubernetes vollständig integriert haben.
   - Vorteile:
     - Tief integriert mit Google-Diensten wie BigQuery, Cloud Storage und Stackdriver Monitoring.
     - Unterstützt Auto-Scaling, automatisierte Upgrades und Sicherungspatches.
     - Leistungsstarke Netzwerkoptionen wie VPC-Native Clustering und Multi-Region Clustering.
     - **Beispiel: Erstellen eines GKE-Clusters**:
       ```bash
       gcloud container clusters create my-gke-cluster --num-nodes=3 --region=us-central1
       ```

## Vergleich der Hyperscaler

| Feature                        | AWS EKS                     | Azure AKS                   | Google GKE                   |
|---------------------------------|-----------------------------|-----------------------------|------------------------------|
| Verfügbarkeit                   | Multi-AZ                    | Multi-AZ                    | Multi-Region                  |
| Integration mit nativen Diensten | EC2, RDS, IAM               | Azure AD, Monitor           | BigQuery, Cloud Storage       |
| CI/CD Integration               | AWS CodePipeline            | Azure DevOps                | Google Cloud Build            |
| Auto-Scaling                    | Ja                          | Ja                          | Ja                            |
| Automatische Updates            | Ja                          | Ja                          | Ja                            |

## Praktische Anwendung

- **Cluster-Verwaltung mit kubectl**:
  Alle Hyperscaler unterstützen die Verwendung von `kubectl`, um auf Kubernetes-Cluster zuzugreifen und sie zu verwalten. Beispiel:
  ```bash
  kubectl get nodes
  kubectl create -f deployment.yaml
  ```

- **Überwachung und Logging**:
  - AWS: CloudWatch
  - Azure: Azure Monitor
  - Google: Stackdriver

## Fazit

Jeder Hyperscaler bietet leistungsstarke Tools und Services zur Verwaltung von Kubernetes-Clustern, mit ähnlichen Features, jedoch speziellen Vorteilen für die Integration mit den jeweiligen nativen Cloud-Diensten.

## Weiterführende Links

- [AWS EKS Dokumentation](https://docs.aws.amazon.com/eks/)
- [Azure AKS Dokumentation](https://docs.microsoft.com/de-de/azure/aks/)
- [Google GKE Dokumentation](https://cloud.google.com/kubernetes-engine/docs)
