# 📈 Lab 07 — Full Observability Stack (Metrics + Logs + Alerts)

🎯 **Goal:** Prometheus + Grafana + Loki + Alertmanager on K8s, **with a test alert that you actually receive.**

> 🔭 *This is my day job. The #1 lesson I've learned: an alert you haven't seen arrive is an alert you can't trust.*

## 👣 Steps
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

helm install kps prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
helm install loki grafana/loki-stack -n monitoring --set promtail.enabled=true

kubectl -n monitoring port-forward svc/kps-grafana 3000:80
# Grafana admin password (local lab only):
kubectl -n monitoring get secret kps-grafana -o jsonpath="{.data.admin-password}" | base64 -d
```

In Grafana: **Connections → Data sources → Loki** → URL `http://loki.monitoring:3100`
Then import dashboard **1860** (Node Exporter Full).

### Useful PromQL
```promql
# CPU per pod
sum(rate(container_cpu_usage_seconds_total{namespace="demo"}[5m])) by (pod)
# Restarting pods
kube_pod_container_status_restarts_total > 3
# Disk above 85%
(1 - node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100 > 85
```

### Useful LogQL
```logql
{namespace="demo"} |= "error"
sum by (pod) (count_over_time({namespace="demo"} |= "error" [5m]))
```

### A real alert rule
```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: demo-rules
  namespace: monitoring
  labels: { release: kps }
spec:
  groups:
    - name: demo
      rules:
        - alert: PodCrashLooping
          expr: increase(kube_pod_container_status_restarts_total{namespace="demo"}[10m]) > 3
          for: 2m
          labels: { severity: critical, category: application }
          annotations:
            summary: "{{ $labels.pod }} is crash-looping"
            runbook: "kubectl -n demo describe pod {{ $labels.pod }}"
```

### Route it (Slack / MS Teams / email)
```yaml
route:
  group_by: [alertname, namespace]
  receiver: team-chat
  routes:
    - matchers: [severity="critical"]
      receiver: team-chat
      repeat_interval: 1h
receivers:
  - name: team-chat
    msteamsv2_configs:
      - webhook_url_file: /etc/alertmanager/secrets/teams-webhook   # never hard-code webhooks!
```

## ✅ Verify
Deploy a pod that crashes on purpose:
```bash
kubectl -n demo run crasher --image=busybox --restart=Always -- sh -c "sleep 5; exit 1"
```
…and **watch the alert land in your channel.** 🎉

## 🧹 Cleanup
`helm uninstall kps loki -n monitoring && kubectl delete ns monitoring`

## 🧠 Interview angle
> **Metrics vs Logs vs Traces:** metrics tell you *that* something is wrong, logs tell you *what* went wrong, and traces tell you *where* in the request path it happened.
>
> **Alert fatigue?** Use severity tiers, `for:` durations, grouping, inhibition rules, and route by category to the right team.
>
> **Silent failures:** a broken notification template or a dead sidecar can block **all** alerts without any error you'd notice. Test delivery end to end, regularly.
