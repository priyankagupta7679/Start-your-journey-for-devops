<div align="center">

<img src="https://capsule-render.vercel.app/api?type=venom&color=0:1a2a6c,50:b21f1f,100:fdbb2d&height=220&text=DevOps%20Zero%20to%20Hero&fontSize=52&fontColor=ffffff&desc=Roadmap%20%E2%86%92%20Labs%20%E2%86%92%20Interview%20Prep&descSize=20&descAlignY=72" width="100%"/>

**A free, step-by-step path into DevOps, from someone who made the switch herself.** 💪

![Stars](https://img.shields.io/github/stars/priyankagupta7679/devops-zero-to-hero?style=for-the-badge&color=yellow)
![Forks](https://img.shields.io/github/forks/priyankagupta7679/devops-zero-to-hero?style=for-the-badge&color=blue)
![Beginner Friendly](https://img.shields.io/badge/Beginner-Friendly-2ea44f?style=for-the-badge)
![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-ff69b4?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

**[🗺️ Roadmap](ROADMAP.md)** · **[🧪 Labs](labs/README.md)** · **[🎯 Interview Prep](interview-prep/README.md)** · **[👩‍💻 My Story](#-my-story--why-this-repo-exists)**

</div>

---

## 👩‍💻 My Story — Why This Repo Exists

I didn't start in DevOps. I **zig-zagged** into it, and that's exactly why I think *you* can do it too.

```
2019 ─► 👩‍💻 Software Engineer (C#)          fintech software
2020 ─► ⏸️  Career gap                         (yes, it's okay — keep going)
2021 ─► 📈 Data Science Intern               FAANG stock-market analysis in Python
2022 ─► 🎧 Technical Support Engineer        learning how production really breaks
2023 ─► 🛠️ Application Support Engineer      → promoted to Team Lead of 6
2025 ─► 🚀 DevOps Engineer                   observability, alerting, AWS, Kubernetes
```

My career had gaps, I switched domains, and I spent years in support roles. **None of that held me back.** Support work actually taught me the most important DevOps skill: **understanding how production fails, and staying calm while fixing it.**

This repo is the guide I wish I'd had: **what to learn, in what order, with hands-on labs and real interview questions.**

> 💡 *If you're coming from support, testing, development or a career break: your experience counts. DevOps is where all of it comes together.*

---

## 🗺️ The Path (at a glance)

```mermaid
flowchart LR
    A[🐧 Linux] --> B[🌿 Git]
    B --> C[🐳 Docker]
    C --> D[☁️ AWS Core]
    D --> E[🏗️ Terraform]
    E --> F[☸️ Kubernetes]
    F --> G[⛵ Helm]
    G --> H[🐙 ArgoCD / GitOps]
    H --> I[📈 Observability]
    I --> J[🔁 CI/CD]
    J --> K[📨 Kafka · RabbitMQ · Redis]
    K --> L[🔐 Secrets & Security]
    L --> M[🎯 Interview Ready!]

    style A fill:#FCC624,color:#000
    style C fill:#2496ED,color:#fff
    style D fill:#FF9900,color:#000
    style E fill:#7B42BC,color:#fff
    style F fill:#326CE5,color:#fff
    style I fill:#F46800,color:#fff
    style M fill:#2ea44f,color:#fff
```

| Phase | Weeks | Topics | Go |
|:---:|:---:|---|:---:|
| 🧱 **Foundations** | 1 | Linux · Networking basics · Git | [→](ROADMAP.md#-phase-1--foundations-week-1) |
| 🐳 **Containers** | 1–2 | Docker · Dockerfiles · Compose · Registries | [→](ROADMAP.md#-phase-2--containers-week-12) |
| ☁️ **Cloud** | 2 | AWS: VPC · EC2 · S3 · IAM · RDS · Lambda · CloudWatch · Route53 | [→](ROADMAP.md#%EF%B8%8F-phase-3--cloud-aws-week-2) |
| 🏗️ **IaC** | 2–3 | Terraform: providers · state · modules · remote backend | [→](ROADMAP.md#%EF%B8%8F-phase-4--infrastructure-as-code-week-23) |
| ☸️ **Orchestration** | 3 | Kubernetes · Helm · ArgoCD / GitOps | [→](ROADMAP.md#%EF%B8%8F-phase-5--orchestration-week-3) |
| 📈 **Observability** | 4 | Prometheus · Grafana · Loki · Tempo · Alertmanager · Zabbix · OpenSearch | [→](ROADMAP.md#-phase-6--observability-week-4) |
| 🔁 **Delivery** | 4 | CI/CD · Kafka · RabbitMQ · Redis · Secrets · Security | [→](ROADMAP.md#-phase-7--delivery--data-week-4) |
| 🎯 **Get Hired** | 5 | Troubleshooting drills · Mock interviews · Resume | [→](interview-prep/README.md) |

---

## 🧪 Hands-On Labs

Every lab includes: **🎯 Goal → 🧰 Prereqs → 👣 Steps → ✅ Verify → 🧹 Cleanup** (so nothing keeps billing you!)

| # | Lab | Difficulty | Time |
|:--:|---|:--:|:--:|
| 01 | [Linux survival kit](labs/01-linux.md) | 🟢 Easy | 45m |
| 02 | [Build & ship your first Docker image](labs/02-docker.md) | 🟢 Easy | 1h |
| 03 | [Provision AWS with Terraform](labs/03-terraform.md) | 🟡 Medium | 1.5h |
| 04 | [Deploy an app on Kubernetes](labs/04-kubernetes.md) | 🟡 Medium | 2h |
| 05 | [Package it with Helm](labs/05-helm.md) | 🟡 Medium | 1h |
| 06 | [GitOps with ArgoCD](labs/06-argocd.md) | 🟠 Hard | 1.5h |
| 07 | [Full observability stack (PLG + alerts)](labs/07-observability.md) | 🟠 Hard | 2h |
| 08 | [CI/CD pipeline: build → registry → deploy](labs/08-cicd.md) | 🟠 Hard | 2h |
| 09 | [Break it & fix it: troubleshooting drill](labs/09-troubleshooting.md) | 🔴 Real-world | 1h |

👉 **[See all labs](labs/README.md)**

---

## 🎯 Interview Prep

- 📘 [Core Q&A](interview-prep/README.md#-core-questions): Docker, K8s, Terraform, AWS, Observability
- 🔥 [Scenario questions](interview-prep/README.md#-scenario-based-questions): "The pod is in CrashLoopBackOff. What do you do?"
- 🗣️ [How to tell your story](interview-prep/README.md#%EF%B8%8F-how-to-tell-your-story-career-switchers): especially if you're switching careers
- ✅ [30-day plan](ROADMAP.md#-30-day-sprint-plan)

---

## 🌟 Golden Rules

1. ⌨️ **Type every command yourself.** Don't copy-paste. Muscle memory is what gets you hired.
2. 🧹 **Always clean up** cloud resources after a lab. Set a billing alert on day 1.
3. 🔐 **Never commit secrets.** Use `.gitignore`, environment variables and secret managers.
4. 📝 **Write down what broke and how you fixed it.** Those notes become your interview stories.
5. 🧠 **Understand *why*, not just *how*.** Interviewers always ask "why".

---

<div align="center">

### ⭐ If this helped you, star the repo and share it with someone starting their journey!

Made with ❤️ by **[Priyanka Gupta](https://github.com/priyankagupta7679)**, DevOps Engineer

</div>
