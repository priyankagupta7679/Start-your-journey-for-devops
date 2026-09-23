# 🐳 Lab 02 — Build & Ship Your First Docker Image

🎯 **Goal:** Containerise a tiny web app and push it to a registry.
🧰 **Prereqs:** Docker installed, a Docker Hub (or ECR) account

## 👣 Steps
Create `index.html`:
```html
<h1>Hello from my first container 🚀</h1>
```

Create `Dockerfile`:
```dockerfile
FROM nginx:1.27-alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
```

Build and run:
```bash
docker build -t hello-devops:1.0 .
docker run -d --name web -p 8080:80 hello-devops:1.0
curl localhost:8080
docker logs web
docker exec -it web sh      # look around inside, then exit
```

### Push to a registry
```bash
# Docker Hub
docker tag hello-devops:1.0 <your-dockerhub-user>/hello-devops:1.0
docker push <your-dockerhub-user>/hello-devops:1.0

# Amazon ECR (placeholders; never commit real IDs)
aws ecr get-login-password --region <REGION> | \
  docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com
```

## ✅ Verify
`curl localhost:8080` returns your HTML, and `docker ps` shows `(healthy)`.

## 🧹 Cleanup
```bash
docker rm -f web && docker rmi hello-devops:1.0
```

## 🧠 Interview angle
> **"How do you make an image smaller & safer?"** Use an alpine/distroless base, multi-stage builds, `.dockerignore`, combine `RUN` layers, run as a non-root `USER`, and scan with Trivy.
