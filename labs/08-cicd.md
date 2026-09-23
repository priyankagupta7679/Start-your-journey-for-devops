# 🔁 Lab 08 — CI/CD: Build → Registry → Deploy

🎯 **Goal:** Every push to `main` builds an image, scans it, pushes it, and triggers a deploy.
🧰 **Prereqs:** Lab 02 app in a GitHub repo, repo secrets `DOCKERHUB_USER` + `DOCKERHUB_TOKEN`

## 👣 Steps — GitHub Actions
```yaml
# .github/workflows/ci.yml
name: ci
on:
  push:
    branches: [main]
permissions:
  contents: read
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      IMAGE: ${{ secrets.DOCKERHUB_USER }}/hello-devops:${{ github.sha }}
    steps:
      - uses: actions/checkout@v4
      - uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USER }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - uses: docker/build-push-action@v6
        with:
          push: true
          tags: ${{ env.IMAGE }}
      - name: Scan image
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.IMAGE }}
          severity: CRITICAL,HIGH
      # GitOps step: bump the image tag in your Helm values repo → ArgoCD deploys it (Lab 06)
```

### AWS equivalent
```
CodePipeline (source) → CodeBuild (buildspec.yml: build → tag → push to ECR) → deploy to K8s (Helm / ArgoCD)
```
For AWS, use **OIDC role assumption** instead of long-lived access keys in CI.

## ✅ Verify
Push a commit, the Actions run goes green ✅, and the new SHA tag appears in your registry.

## 🧠 Interview angle
> **CI vs Continuous Delivery vs Continuous Deployment**, **why tag by commit SHA instead of `latest`** (traceability + rollback), and **how secrets reach the pipeline** (encrypted repo secrets / OIDC, never in code).
