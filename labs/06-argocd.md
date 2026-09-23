# 🐙 Lab 06 — GitOps with ArgoCD

🎯 **Goal:** Make Git the source of truth, so a push to Git updates the cluster.
🧰 **Prereqs:** Lab 05 chart pushed to a GitHub repo of your own

## 👣 Steps
```bash
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl -n argocd port-forward svc/argocd-server 8081:443
# initial admin password (local lab only; change it after first login):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

```yaml
# application.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata: { name: webapp, namespace: argocd }
spec:
  project: default
  source:
    repoURL: https://github.com/<you>/<your-gitops-repo>.git
    targetRevision: main
    path: charts/webapp
  destination: { server: https://kubernetes.default.svc, namespace: demo }
  syncPolicy:
    automated: { prune: true, selfHeal: true }
    syncOptions: [CreateNamespace=true]
```

## ✅ Verify
1. Change `replicaCount` in Git and push. ArgoCD syncs it automatically.
2. `kubectl -n demo delete deploy webapp` and watch ArgoCD **self-heal** it back. 🪄

## 🧹 Cleanup
`kubectl delete ns argocd demo`

## 🧠 Interview angle
> **Push vs pull deployment:** with CI-push, the pipeline needs cluster credentials. With GitOps-pull, the cluster pulls from Git itself. That means fewer secrets in CI, a full audit trail in Git, and automatic drift correction.
