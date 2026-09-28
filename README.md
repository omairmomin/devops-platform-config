# devops-platform-config

GitOps configuration repo for the Online Boutique deployment.
ArgoCD watches this repository and syncs the manifests in `manifests/`
to the Kubernetes cluster.

Application source lives in `devops-platform-apps`.
