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

## ⚠️ Issues & Fixes

1. ❌ Grafana not accessible

Problem: Grafana was exposed using ClusterIP and couldn’t be accessed from outside.

Fix:

- Changed Service to NodePort.

2. ❌ LoadBalancer stuck in `<pending>`

Cause: K3s does not support LoadBalancer by default.

Fix: I used NodePort

3. ❌ ArgoCD SyncError: Invalid value in serviceAccountName

Cause: Wrong value (`image URL`) passed as serviceAccountName.

Fix: Corrected manifest values in `currencyservice` Deployment.

4. ❌ App stuck in Health: Degraded

Cause: `loadgenerator` used an outdated or missing image `busybox:latest`.

Fix: Updated to the correct version used by the official Online Boutique:

```
image: us-central1-docker.pkg.dev/google-samples/microservices-demo/loadgenerator:v0.10.2
```

5. ❌ ArgoCD ComparisonError - path not found

Cause: Misplaced ArgoCD Application file (`apps/otel-operator-app.yaml` inside `base/` instead of `apps/`)

Fix: Moved it to correct path under `apps/`.

6. ❌ API Version Conflict (`v1beta1` vs `v1alpha1`)

Despite all manifests using `v1alpha1`, we consistently encountered the following error:

```
request to convert CR from an invalid group/version: opentelemetry.io/v1beta1
```

This indicates the cluster is still referencing stale or misconfigured CRDs from a previous v1beta1 deployment.

7. ❌ Stuck or Corrupted CRDs

Attempts to delete or reapply OpenTelemetryCollector resources failed with similar conversion errors. 

Even forceful deletion via:
```
kubectl delete crd opentelemetrycollectors.opentelemetry.io  --force --grace-period=0

```
I resolved this by removing nd patching the finalizers in CRD

```
kubectl patch crd opentelemetrycollectors.opentelemetry.io -p '{"metadata":{"finalizers":[]}}' --type=merge
```
