<div align="center">

# 🧪 Hands-On Labs

*Reading doesn't make you a DevOps engineer. Doing does.* ⌨️

[⬅️ Home](../README.md) · [🗺️ Roadmap](../ROADMAP.md) · [🎯 Interview Prep](../interview-prep/README.md)

</div>

Every lab follows the same format:

```
🎯 Goal  →  🧰 Prereqs  →  👣 Steps  →  ✅ Verify  →  🧹 Cleanup  →  🧠 Interview angle
```

| # | Lab | Skills | Level |
|:--:|---|---|:--:|
| 01 | [Linux survival kit](01-linux.md) | processes, disk, network, logs | 🟢 |
| 02 | [Docker: build & ship](02-docker.md) | Dockerfile, healthcheck, registry | 🟢 |
| 03 | [Terraform on AWS](03-terraform.md) | VPC, subnet, S3, remote state | 🟡 |
| 04 | [Kubernetes app deploy](04-kubernetes.md) | Deployment, Service, probes, HPA | 🟡 |
| 05 | [Helm chart](05-helm.md) | chart, values, upgrade, rollback | 🟡 |
| 06 | [GitOps with ArgoCD](06-argocd.md) | Application, auto-sync, self-heal | 🟠 |
| 07 | [Observability stack](07-observability.md) | Prometheus, Grafana, Loki, Alertmanager | 🟠 |
| 08 | [CI/CD pipeline](08-cicd.md) | GitHub Actions → registry → K8s | 🟠 |
| 09 | [Break it & fix it](09-troubleshooting.md) | real incident debugging | 🔴 |

### 🛠️ Tools you'll need
`git` · `docker` · `kubectl` · `helm` · `terraform` · `aws` CLI · [`kind`](https://kind.sigs.k8s.io/) or `minikube` (a free local K8s cluster)

> 💸 **Cost tip:** Labs 01, 02, 04–07 and 09 run **100% free** on a local `kind` cluster. Only Lab 03 (and optionally 08) touches AWS. **Always run the cleanup step.**
>
> 🔐 **Security tip:** Never paste real account IDs, keys or passwords into code or commits. Use placeholders like `<ACCOUNT_ID>` and environment variables.
