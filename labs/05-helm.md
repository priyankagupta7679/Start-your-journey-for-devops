# ⛵ Lab 05 — Package It with Helm

🎯 **Goal:** Turn Lab 04 into a reusable chart, then upgrade and roll back.
🧰 **Prereqs:** Lab 04 cluster, `helm` v3

## 👣 Steps
```bash
helm create webapp
# edit webapp/values.yaml:
#   image.repository: nginx
#   image.tag: "1.27-alpine"
#   replicaCount: 2
helm lint webapp
helm template webapp ./webapp | less          # preview the rendered YAML
helm install webapp ./webapp -n demo --create-namespace
helm upgrade webapp ./webapp -n demo --set replicaCount=3
helm history webapp -n demo
helm rollback webapp 1 -n demo
```

Multi-environment pattern:
```bash
helm upgrade --install webapp ./webapp -f values-dev.yaml  -n dev
helm upgrade --install webapp ./webapp -f values-prod.yaml -n prod
```

## ✅ Verify
`helm list -n demo` shows your release, and `helm history` shows its revisions.

## 🧹 Cleanup
`helm uninstall webapp -n demo`

## 🧠 Interview angle
> **Why Helm?** It gives you one templated chart for many environments, versioned releases, and one-command rollback.
