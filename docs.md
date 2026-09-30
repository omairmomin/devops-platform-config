# Phase 3: GitOps Deployment with ArgoCD

## Goal
Deploy the full Online Boutique application to the k3s cluster using
ArgoCD, with the `frontend` service running the custom image built and
scanned by the Jenkins pipeline.

## Setup
- `manifests/online-boutique.yaml` — Google's official Online Boutique
  Kubernetes manifests, with the `frontend` image replaced by
  `ghcr.io/omairmomin/frontend`, built and pushed by Jenkins
- `argocd/application.yaml` — ArgoCD Application resource pointing at
  this repo's `manifests/` folder
- Applied once with `kubectl apply -f <raw GitHub URL>`; from then on
  ArgoCD tracks the repo automatically

## Sync policy
- `automated` sync: any change pushed to `manifests/` is applied
  automatically, no manual intervention
- `prune: true`: resources removed from Git are removed from the cluster
- `selfHeal: true`: manual changes made directly in the cluster are
  reverted back to match Git — Git is the single source of truth
- `CreateNamespace=true`: the `online-boutique` namespace is created
  automatically

## Result
All 12 services running (`1/1 Running`) in the `online-boutique`
namespace, including `frontend` running the Jenkins-built,
Trivy-scanned image from GHCR. Verified working end to end via browser
after port-forwarding the `frontend` service.

## Access
- ArgoCD UI, Jenkins UI, and the app itself are all reached via SSH
  port-forwarding — no ports are exposed publicly on the EC2 security group
