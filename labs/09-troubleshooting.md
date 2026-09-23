# 🔥 Lab 09 — Break It & Fix It

🎯 **Goal:** Practise the incidents you'll actually see on-call. Have a friend break things, and you fix them.

| # | Break it with... | Symptom | Your investigation |
|:-:|---|---|---|
| 1 | Wrong image tag | `ImagePullBackOff` | `kubectl describe pod` → Events |
| 2 | `command: ["sh","-c","exit 1"]` | `CrashLoopBackOff` | `kubectl logs <pod> --previous` |
| 3 | Memory limit `10Mi` | `OOMKilled` | `describe` → Last State → raise limits |
| 4 | Service selector typo | App unreachable | `kubectl get endpoints` is empty |
| 5 | Readiness path `/nope` | Pod `0/1 READY` | Probe failures in Events |
| 6 | Huge CPU request | `Pending` | `describe` → FailedScheduling |
| 7 | Fill a disk (`fallocate -l 5G big`) | Disk alert fires | `df -h`, `du`, rotate/vacuum logs |
| 8 | Break the alert webhook | *No alert arrives!* 😱 | Check receiver config & template, **test delivery end to end** |

### 🧭 The universal debugging flow
```
kubectl get pods
  → kubectl describe pod <pod>
    → kubectl logs <pod> --previous
      → kubectl get events --sort-by=.lastTimestamp
        → check node / network / config / recent deploys
```

### 📝 Write a mini-RCA for each one
| What happened | Impact | Root cause | Fix | Prevention |
|---|---|---|---|---|
| | | | | |

> 💡 These RCAs become your **STAR stories** in interviews. See [Interview Prep](../interview-prep/README.md).
