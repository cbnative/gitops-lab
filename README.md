# Gitops lab

Demo manifests for Argo CD. Argo CD watches this repo and keeps the cluster in sync with it. No secrets in here, so it can stay public.

📝 Full walkthrough: [GitOps with Argo CD: From One App to Many](https://cbnative.com/posts/gitops-with-argocd), which installs Argo CD, syncs one app, breaks things on purpose to prove the reconcile loop, then wraps both apps under a single root Application.

## What is in this repo

- **Workload manifests** (`apps/`): plain Kubernetes YAML, a Deployment and a Service for nginx and for podinfo. Nothing Argo CD specific, just the workloads themselves.
- **Argo CD Applications** (`applications/`): one Application per app, each pointing at its folder under `apps/`.
- **Root application** (`root/app-of-apps.yaml`): the single Application you apply by hand. It points at `applications/`, so every file in that folder becomes a managed child, and applying this one file brings up everything else.

## Use it

Install Argo CD:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Apply the root app:

```bash
kubectl apply -f root/app-of-apps.yaml
```

That's it. Change a file, commit, push, and Argo CD updates the cluster.
