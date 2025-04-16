# LW-Observability-GitOps

## :file_folder: Repo Structure

![](https://github.com/imran1509/LW-Tasks/blob/main/Screenshots/LW-gitops.png)

## ✅ Prerequisites

- Kubernetes Cluster (e.g. K3s)

- ArgoCD installed and configured

- `kubectl` installed

- GitOps access configured for ArgoCD

## 🔧 How to Deploy

1. Clone this repository:

```
git clone https://github.com/imran1509/LW-Observability-GitOps.git
cd LW-Observability-GitOps
```

2. Apply the ArgoCD Applications:

```
kubectl apply -f apps/observability-app.yaml
kubectl apply -f apps/otel-operator-app.yaml

```

3. Go to ArgoCd UI and sync the application

4. Access application and grafana dashboard on their respective nodeports. `Node-IP:NodePort`
