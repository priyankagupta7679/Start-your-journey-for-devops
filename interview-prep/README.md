<div align="center">

# 🎯 DevOps Interview Prep

*Questions I prepared for, answered simply. Read them, then practise saying them **out loud**.* 🎤

[⬅️ Home](../README.md) · [🗺️ Roadmap](../ROADMAP.md) · [🧪 Labs](../labs/README.md)

</div>

---

## 📘 Core Questions

<details>
<summary><b>🐳 Container vs Virtual Machine?</b></summary>

A VM virtualises hardware and runs its own full OS kernel on a hypervisor, so it's heavy (GBs) and slow to boot. A container **shares the host kernel** and isolates processes, filesystem and network using **namespaces + cgroups**, so it's lightweight (MBs) and starts in milliseconds. The trade-off: isolation is weaker, which is why we add seccomp, non-root users and read-only filesystems.
</details>

<details>
<summary><b>🐳 CMD vs ENTRYPOINT?</b></summary>

`ENTRYPOINT` defines the executable that always runs. `CMD` gives default arguments, which you can override at `docker run`. The common pattern is `ENTRYPOINT ["python","app.py"]` + `CMD ["--port","8080"]`.
</details>

<details>
<summary><b>☸️ What happens when you run <code>kubectl apply -f deploy.yaml</code>?</b></summary>

kubectl sends the manifest to the **API server** → it's validated and stored in **etcd** → the **Deployment controller** creates a ReplicaSet → the ReplicaSet creates Pods → the **scheduler** assigns each Pod to a node → that node's **kubelet** pulls the image and starts the containers through the container runtime → **kube-proxy** / CNI wire up the Service networking.
</details>

<details>
<summary><b>☸️ Deployment vs StatefulSet vs DaemonSet?</b></summary>

- **Deployment**: stateless, interchangeable pods (web/API)
- **StatefulSet**: stable identity + stable storage per pod (Kafka, databases)
- **DaemonSet**: one pod per node (log shippers like Promtail, node-exporter)
</details>

<details>
<summary><b>☸️ ClusterIP vs NodePort vs LoadBalancer vs Ingress?</b></summary>

ClusterIP = internal only. NodePort = opens a port on every node. LoadBalancer = a cloud load balancer per service. Ingress = one L7 entry point that routes by host/path to many services (much cheaper than one LB per service).
</details>

<details>
<summary><b>🏗️ What is Terraform state and why keep it remote?</b></summary>

State maps your code to real cloud resources. Keep it remote (S3 + locking) so the team shares one truth, concurrent applies are blocked, and the file (which can contain secrets) is encrypted instead of sitting on a laptop or in Git.
</details>

<details>
<summary><b>🏗️ <code>terraform plan</code> shows a resource will be destroyed. You didn't expect that. What now?</b></summary>

**Stop, don't apply.** Read *why* in the plan (`forces replacement`). Common causes: an immutable attribute changed, a resource was renamed (use a `moved` block), or someone changed things manually (drift). Fix it with `lifecycle { prevent_destroy = true }` for critical resources, and use `terraform state mv` / `import` when needed.
</details>

<details>
<summary><b>☁️ Security Group vs NACL?</b></summary>

A Security Group is **stateful** and attached to an instance/ENI, with allow rules only. A NACL is **stateless** and works at the subnet level, with allow + deny rules evaluated in order. Because NACLs are stateless, return traffic must be allowed explicitly.
</details>

<details>
<summary><b>☁️ How do you give an EC2 / Lambda access to S3 securely?</b></summary>

Use an **IAM role** (an instance profile or Lambda execution role) with a least-privilege policy. **Never** use static access keys stored on the box. I've migrated a service from an IAM user's keys to an instance role, then deleted the old user.
</details>

<details>
<summary><b>📈 Metrics vs Logs vs Traces?</b></summary>

Metrics tell you **that** something is wrong (cheap, numeric, good for alerting). Logs tell you **what** went wrong (detailed events). Traces tell you **where** in a distributed request it went wrong (spans across services). Grafana puts all three side by side: Prometheus, Loki and Tempo.
</details>

<details>
<summary><b>📈 SLI vs SLO vs SLA?</b></summary>

- **SLI**: the measurement (e.g. % of successful requests)
- **SLO**: the internal target (e.g. 99.9% monthly)
- **SLA**: the contractual promise to the client, with penalties

The gap between 100% and your SLO is the **error budget**. I build SLA dashboards that turn these numbers into monthly client reports.
</details>

<details>
<summary><b>📈 How do you reduce alert fatigue?</b></summary>

Alert on **symptoms** (user impact), not every cause. Use `for:` durations, severity tiers, grouping and inhibition (if the node is down, suppress its pod alerts). Route by category to the right team, and review noisy alerts every week. Also **test delivery**, because a silent pipeline is worse than a noisy one.
</details>

<details>
<summary><b>📨 Kafka vs RabbitMQ?</b></summary>

**Kafka** is a distributed, durable **log**: high throughput, replayable, with partitions and consumer groups. It's great for event streaming and IoT telemetry. **RabbitMQ** is a **message broker**: flexible routing (exchanges → queues), per-message ack, and a good fit for task queues and request/response work.
</details>

<details>
<summary><b>🔐 How do you manage secrets in Kubernetes?</b></summary>

K8s Secrets are only base64-encoded, so enable encryption at rest and tight RBAC. Better still, keep secrets in **AWS Secrets Manager / SSM** and sync them with the **External Secrets Operator**. Never commit them to Git, and scan repos for leaked secrets.
</details>

---

## 🔥 Scenario-Based Questions

<details>
<summary><b>A pod is in <code>CrashLoopBackOff</code>. Walk me through it.</b></summary>

1. `kubectl describe pod`: check the exit code, OOMKilled status, probe failures and Events
2. `kubectl logs <pod> --previous`: the logs from the crashed container
3. Look for config errors (missing env/secret/ConfigMap), a liveness probe that is too aggressive, or a dependency that's down
4. Check what changed recently: `kubectl rollout history`, and roll back if a deploy caused it
5. Fix, verify, and write a short RCA
</details>

<details>
<summary><b>Users say the app is slow, but all dashboards look green. What do you do?</b></summary>

The dashboards may be measuring the wrong thing. Check **latency percentiles (p95/p99)**, not averages. Check the database (slow queries, connection pool), the cache hit rate, the Kafka consumer lag and external dependencies. Use traces to find the slow span. Afterwards, add an SLI that would have caught it.
</details>

<details>
<summary><b>A Kafka broker's disk is filling fast. What do you do?</b></summary>

Short term: find which topic is growing (`kafka-log-dirs`), check consumer lag, reduce `retention.ms` on the heavy topic, or expand the volume (managed MSK supports storage scaling). Long term: set retention policies, add disk-usage alerts at warning/critical tiers, and review producer volume.
</details>

<details>
<summary><b>An alert that should have fired… never arrived. How do you investigate?</b></summary>

Walk the whole path: **Did the rule evaluate?** (Prometheus → Alerts tab) → **Did Alertmanager receive it?** → **Did routing match a receiver?** → **Did the notification template render?** → **Did the webhook accept it?** A broken template or a dead sidecar can drop every alert silently. The fix is a scheduled end-to-end test alert.
</details>

<details>
<summary><b>The monitoring server itself is out of disk / memory. What do you do?</b></summary>

Free space safely first: `journalctl --vacuum-time`, rotate logs, and clean old history/trends data. Tune memory-heavy services (e.g. PHP-FPM worker counts). Then fix the root cause: set retention, right-size the instance, and add an alert on the monitoring box from a **different** system (who monitors the monitor? 👀).
</details>

---

## 🗣️ How to Tell Your Story (Career Switchers)

Use **STAR**: **S**ituation → **T**ask → **A**ction → **R**esult (with numbers).

> **"Tell me about yourself."**
> "I started as a C# developer on fintech software, then moved into support. That's where I learned how production actually breaks. At my last company I was promoted to lead a team of 6 on a fintech product, handling clients and incidents directly. I moved into DevOps because I wanted to fix problems at the root instead of handling tickets. Today I own observability and alerting for multiple production environments on AWS and Kubernetes: SLA dashboards, alert routing and automated health reports."

**Turn "gaps" and "support roles" into strengths:**
| They might think… | You say… |
|---|---|
| "Only support experience" | "I've done hundreds of production RCAs. I know what good monitoring *should* catch." |
| "Career gap" | "I used it to retrain in data science and cloud, and here's what I built." |
| "New to DevOps" | "Here are the labs I built, and here's a real incident I fixed last month." |

---

## ✅ Final Checklist Before the Interview

- [ ] Can draw your production architecture on a whiteboard (VPC → LB → K8s → DB / Kafka / Redis → monitoring)
- [ ] 3 STAR incident stories ready (use your [Lab 09](../labs/09-troubleshooting.md) RCAs!)
- [ ] Can explain *why* for every tool on your resume
- [ ] Have questions ready for them: *"How do you handle on-call? What does your observability stack look like?"*
- [ ] **Never** share your current employer's account IDs, hostnames or internal details. Describe the architecture generically.

<div align="center">

### 💪 You've got this!

</div>
