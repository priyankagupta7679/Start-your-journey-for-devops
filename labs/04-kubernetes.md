# ☸️ Lab 04 — Deploy an App on Kubernetes

🎯 **Goal:** Deployment + Service + probes + autoscaling on a free local cluster.
🧰 **Prereqs:** `kind` or `minikube`, `kubectl`

## 👣 Steps
```bash
kind create cluster --name lab
kubectl create namespace demo
```

```yaml
# app.yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: web, namespace: demo }
spec:
  replicas: 2
  selector: { matchLabels: { app: web } }
  template:
    metadata: { labels: { app: web } }
    spec:
      containers:
        - name: web
          image: nginx:1.27-alpine
          ports: [{ containerPort: 80 }]
          resources:
            requests: { cpu: 50m, memory: 64Mi }
            limits:   { cpu: 200m, memory: 128Mi }
          readinessProbe: { httpGet: { path: /, port: 80 }, initialDelaySeconds: 3 }
          livenessProbe:  { httpGet: { path: /, port: 80 }, initialDelaySeconds: 10 }
---
apiVersion: v1
kind: Service
metadata: { name: web, namespace: demo }
spec:
  selector: { app: web }
  ports: [{ port: 80, targetPort: 80 }]
```

```bash
kubectl apply -f app.yaml
kubectl -n demo get pods -o wide
kubectl -n demo port-forward svc/web 8080:80          # open http://localhost:8080
kubectl -n demo scale deploy/web --replicas=4
kubectl -n demo autoscale deploy/web --cpu-percent=60 --min=2 --max=6   # needs metrics-server
kubectl -n demo set image deploy/web web=nginx:1.27   # rolling update
kubectl -n demo rollout status deploy/web
kubectl -n demo rollout undo deploy/web
```

## ✅ Verify
All pods are `Running` and `READY 1/1`, and the page loads through port-forward.

## 🧹 Cleanup
`kind delete cluster --name lab`

## 🧠 Interview angle
> **Readiness vs Liveness:** a failing *readiness* probe removes the pod from Service traffic. A failing *liveness* probe **restarts** the container. Mixing them up leads to restart loops.
