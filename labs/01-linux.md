# 🐧 Lab 01 — Linux Survival Kit

🎯 **Goal:** Diagnose a "slow server" like an on-call engineer.
🧰 **Prereqs:** Any Linux VM, WSL, or `docker run -it ubuntu bash`

## 👣 Steps
```bash
# 1. Who am I, where am I?
whoami; hostname; cat /etc/os-release; uptime

# 2. CPU & memory: what's hungry?
top -o %CPU          # press q to quit
free -h
ps aux --sort=-%mem | head -5

# 3. Disk: what's full?
df -h
sudo du -xh /var --max-depth=2 2>/dev/null | sort -rh | head -10

# 4. Network: what's listening?
ss -tulpn
curl -I https://example.com
dig example.com +short

# 5. Logs: what happened?
sudo journalctl -p err --since "1 hour ago"
sudo tail -f /var/log/syslog
```

## ✅ Verify
You can answer: *Which process uses the most memory? Which disk is most full? What's listening on port 22?*

## 🧹 Cleanup
`sudo journalctl --vacuum-time=7d` safely trims old logs. I've used this as a real fix on a monitoring server whose disk was filling up!

## 🧠 Interview angle
> **"A server is slow. Walk me through it."** → `uptime` (load) → `top` (CPU/mem) → `df -h` (disk) → `iostat` (IO) → `ss` (connections) → `journalctl` (errors). Go from broad to narrow, and use data, not guesses.
