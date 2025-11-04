# Aquapump GitOps Repository

This repository contains the Argo CD AppProject and ApplicationSet definitions plus the Helm overrides for every Aquapump environment.

## Environments

- **aquapump-dev** – development version (auto-updates on push)
- **aquapump-stage** – staging testing environment
- **aquapump-prod** – production version (tracks the `refs/tags/prod` tag in the application repository; update or retag this reference to promote a release)

## Bootstrap

To bootstrap these on your EKS cluster:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f applications/project.yaml
kubectl apply -n argocd -f applications/aquapump-applicationset.yaml
```

Environment-specific Helm overrides live under `environments/<env>/values.yaml`. The ApplicationSet combines those overrides with the main chart from the application repository.

Before syncing dev or stage, copy the runtime secret into each namespace (or configure External Secrets) so the backend has the Supabase/AI credentials it expects:

```bash
kubectl get secret aquapump-secrets -n aquapump -o yaml | \
  sed 's/namespace: aquapump/namespace: aquapump-dev/' | kubectl apply -f -
kubectl get secret aquapump-secrets -n aquapump -o yaml | \
  sed 's/namespace: aquapump/namespace: aquapump-stage/' | kubectl apply -f -
```

For local development clusters, the dev environment ships with `ingress.enabled=false` to avoid host collisions with staging/production.
