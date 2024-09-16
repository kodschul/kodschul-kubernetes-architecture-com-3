
# Getting Started mit Kubernetes

## Einführung

Kubernetes ist eine Plattform zur Automatisierung der Bereitstellung, Skalierung und Verwaltung containerisierter Anwendungen. Diese Anleitung führt durch die ersten Schritte mit Kubernetes und wie man einfache Anwendungen in einem Cluster betreibt.

## Voraussetzungen

Bevor du mit Kubernetes beginnst, stelle sicher, dass du die folgenden Tools installiert hast:
- **kubectl**: Kubernetes-Befehlszeilentool.
- **Minikube**: Lokale Kubernetes-Installation für Tests und Entwicklungszwecke.
- **Docker**: Container-Laufzeitumgebung.

## 1. Minikube installieren

Minikube ist eine leichtgewichtige Kubernetes-Implementierung, die lokal auf deinem Computer ausgeführt wird. Es ist eine großartige Möglichkeit, um Kubernetes zu lernen.

Installation von Minikube (Beispiel für MacOS):

```bash
brew install minikube
```

Starte Minikube:

```bash
minikube start
```

Verifiziere, dass Minikube läuft:

```bash
kubectl get nodes
```

## 2. Kubernetes CLI - kubectl

Mit dem Tool `kubectl` kannst du mit deinem Kubernetes-Cluster interagieren.

### Wichtige Befehle:

- **Pods auflisten**:
  ```bash
  kubectl get pods
  ```

- **Deployment erstellen**:
  ```bash
  kubectl create deployment nginx --image=nginx
  ```

- **Pods beschreiben**:
  ```bash
  kubectl describe pod <pod-name>
  ```

- **Pod löschen**:
  ```bash
  kubectl delete pod <pod-name>
  ```

## 3. Erste Anwendung bereitstellen

Nachdem du Minikube gestartet hast, kannst du deine erste Anwendung in Kubernetes bereitstellen.

### Beispiel: NGINX Deployment

Erstelle ein einfaches Deployment mit NGINX:

```bash
kubectl create deployment nginx --image=nginx
```

Verifiziere, dass der Pod läuft:

```bash
kubectl get pods
```

Exponiere den NGINX Pod als Service:

```bash
kubectl expose deployment nginx --type=NodePort --port=80
```

Rufe die IP-Adresse des Minikube-Clusters ab:

```bash
minikube service nginx --url
```

Öffne die Anwendung in deinem Browser, indem du die bereitgestellte URL eingibst.

## 4. Skalieren der Anwendung

Kubernetes ermöglicht das einfache Skalieren von Anwendungen.

Skaliere das NGINX Deployment auf 3 Replikate:

```bash
kubectl scale deployment nginx --replicas=3
```

Prüfe, ob die neuen Replikate erstellt wurden:

```bash
kubectl get pods
```

## 5. Aufräumen

Lösche den Service und das Deployment, um Ressourcen freizugeben:

```bash
kubectl delete service nginx
kubectl delete deployment nginx
```

## Nächste Schritte

Jetzt, wo du eine grundlegende Einführung in Kubernetes hast, kannst du fortgeschrittene Konzepte wie Ingress, ConfigMaps, und Volumes erkunden.

## Nützliche Links

- [Offizielle Kubernetes Dokumentation](https://kubernetes.io/docs/)
- [Minikube Dokumentation](https://minikube.sigs.k8s.io/docs/)
- [kubectl Referenz](https://kubernetes.io/docs/reference/kubectl/)