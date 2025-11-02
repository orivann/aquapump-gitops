# Aquapump GitOps Repository

This repository contains ArgoCD Application manifests for all environments.

## Environments

- **aquapump-dev** – development version (auto-updates on push)
- **aquapump-stage** – staging testing environment
- **aquapump-prod** – production version

## Bootstrap

To apply these on your EKS cluster:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f applications/project.yaml
kubectl apply -n argocd -f applications/aquapump-dev.yaml
kubectl apply -n argocd -f applications/aquapump-stage.yaml
kubectl apply -n argocd -f applications/aquapump-prod.yaml
