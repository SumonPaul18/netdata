# 🚀 Netdata: Full-Stack Observability with Zero-Config & On-Prem AI

> **"Per-second metrics. Zero sampling. Real-time AI that explains root cause in plain English."**

Netdata gives **lean DevOps teams, SREs, and homelab engineers** enterprise-grade observability — without complexity, cloud lock-in, or hidden costs.

---

## 📖 Table of Contents

1. [What is Netdata?](#-what-is-netdata)
2. [Is Netdata Fully Open-Source?](#-is-netdata-fully-open-source)
3. [How Netdata Actually Works](#-how-netdata-actually-works)
4. [Netdata vs Popular Tools (Prometheus, Grafana, Datadog, Zabbix)](#-netdata-vs-popular-tools)
5. [Can You Use Netdata in Production for Free?](#-can-you-use-netdata-in-production-for-free)
6. [Installation Guide](#-installation-guide)
   - [6.1 Direct Install](#61-direct-install)
   - [6.2 Docker Compose (Advanced, Production-Ready)](#62-docker-compose-advanced-production-ready)
7. [Step-by-Step: Connect to Netdata Cloud Using `.env`](#-step-by-step-connect-to-netdata-cloud-using-env)
8. [Monitor Linux, Windows & Network Devices](#-monitor-linux-windows--network-devices)
9. [References & Resources](#-references--resources)

---

## ❓ What is Netdata?

Netdata is a **real-time, distributed, open-core observability platform** that collects **thousands of metrics per second** from every layer of your infrastructure — **without sampling** and **without requiring configuration**.

Key capabilities:
- **Infrastructure Monitoring**: CPU, RAM, disk I/O, temperature, processes
- **Application Monitoring**: MySQL, Nginx, Redis, Node.js, Prometheus, etc.
- **Container & Kubernetes**: Docker, K3s, K8s pod/node metrics
- **Synthetic Checks**: HTTP, DNS, TCP health probes
- **Logs**: Centralized ingestion + visualization
- **AI-Powered Root Cause Analysis**: Explains *why* something broke — in plain English

> **Core Promise**: "What’s wrong? Why? How to fix?" — answered in seconds, not hours.

---

## 🔓 Is Netdata Fully Open-Source?

**Yes and No — it’s a hybrid model:**

| Component | License | Open? | Description |
|----------|--------|------|------------|
| **Netdata Agent (Core)** | GPLv3 | ✅ Yes | Collects metrics, runs dashboard, stores data locally. 100% open-source on [GitHub](https://github.com/netdata/netdata). No telemetry by default. |
| **Netdata Cloud** | Proprietary | ❌ No | Optional SaaS for unified dashboard, AI troubleshooting, alert routing. Free tier available. |
| **Exporters / Plugins** | Mixed (mostly MIT/Apache) | ✅ Yes | Most collectors (Prometheus, SNMP, etc.) are open. |

> ✅ **You can run Netdata 100% offline, on-prem, forever — with zero cost.**  
> 🌐 **Netdata Cloud is optional** (like Grafana Cloud vs self-hosted Grafana).

> ℹ️ According to [netdata.cloud/open-source/](https://www.netdata.cloud/open-source/):  
> _“At Netdata, our ultimate goal is to make it simpler for everyone to understand and manage the technology that runs the world.”_  
> The **open-source agent** is the foundation — the community and contributors shape its future.

---

## ⚙️ How Netdata Actually Works

### Architecture Overview

```
[Your Server] → [Netdata Agent] → [Local Web UI (port 19999)]
                              ↘ (Optional) [Netdata Cloud ← Metadata only]
```

1. **Agent runs on every host** (bare metal, VM, container).
2. Uses **high-efficiency collectors** (written in Go, C, Python) to pull **per-second metrics**.
3. Stores **1 hour of high-res data in RAM**, **24–72h in disk (round-robin DB)** — extremely efficient (40x better than TSDBs).
4. Serves **interactive, real-time dashboard** via embedded web server.
5. **Never sends raw metrics to cloud** — only anonymized metadata (hostname, alarms) if you opt into Cloud.

> 🧠 **AI Engine**: Runs on Netdata Cloud. It analyzes your **on-prem metrics** (via secure metadata tunnel) and returns **natural language explanations**.

---

## 🔍 Netadata vs Popular Tools

| Feature | **Netdata** | **Prometheus + Grafana** | **Datadog** | **Zabbix** |
|--------|------------|--------------------------|------------|-----------|
| **Sampling Rate** | ⚡ **Per-second** | ⏱️ 15–60s (default) | ⏱️ Sampled | ⏱️ 30–60s |
| **Setup Time** | ⏱️ **<1 min** | ⏳ 30+ mins | ⏳ 20+ mins | ⏳ Hours |
| **Config Needed?** | ❌ **Zero** | ✅ YAML files | ✅ Integrations | ✅ Templates |
| **On-Prem Only?** | ✅ **Yes** | ✅ Yes | ❌ No (SaaS-focused) | ✅ Yes |
| **AI Root Cause** | ✅ **Built-in (Cloud)** | ❌ | ✅ (Paid) | ❌ |
| **Cost at Scale** | 💰 **Free + Low infra cost** | 💰 Medium (storage, maintenance) | 💸 **Very High** | 💰 Medium |
| **Windows Monitoring** | ⚠️ Via exporters | ✅ (with effort) | ✅ Native | ✅ Native |
| **Data Sovereignty** | ✅ **100%** | ✅ | ❌ | ✅ |
| **Storage Efficiency** | 🚀 **40x better** | Normal | High (paid) | Medium |

> Netdata = **All-in-one observability** with **edge-native efficiency**.

---

## ✅ Can You Use Netdata in Production for Free?

**Absolutely YES.**

- The **Netdata Agent is 100% free and open-source** for **any use** — personal, commercial, or production.
- **No feature gating** — alarms, health checks, dashboards, all collectors are included.
- **Netdata Cloud is optional**. You can:
  - Use **only local dashboard** (ideal for air-gapped, regulated, or privacy-first environments)
  - Or connect to **free tier of Netdata Cloud** (up to 5 nodes, unlimited metrics)

> ✅ **Production-Ready Features**:
> - High-availability via **streaming** (on-prem replication)
> - Role-based access control (RBAC)
> - Custom dashboards, alarms, and notifications (email, Slack, webhook)
> - TLS/SSL, authentication, reverse proxy support

---

## 🛠️ Installation Guide

### 6.1 Direct Install (Linux/macOS)

```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

- Installs under `/opt/netdata`
- Starts systemd service
- Dashboard: `http://<IP>:19999`

> No data leaves your machine.

---

### 6.2 Docker Compose (Advanced, Production-Ready)

Use this for **Docker hosts, edge devices, or homelabs**.

#### Step 1: Create `.env` file (optional but recommended)

```env
# .env
NETDATA_HOSTNAME=netdata-docker
NETDATA_CLAIM_TOKEN=
NETDATA_CLAIM_URL=https://app.netdata.cloud
DISABLE_TELEMETRY=1
```

> Keep `.env` in your Git repo (token empty) — fill token only on deployment machine.

#### Step 2: `docker-compose.yml`

```yaml
version: '3.8'
services:
  netdata:
    image: netdata/netlatest
    container_name: netdata
    hostname: ${NETDATA_HOSTNAME:-netdata}
    ports:
      - 19999:19999
    restart: unless-stopped
    cap_add:
      - SYS_PTRACE
    security_opt:
      - apparmor:unconfined
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - netdataconfig:/etc/netdata
      - netdatalib:/var/lib/netdata
    environment:
      - NETDATA_UPDATE_CHECK_DISABLE=${DISABLE_TELEMETRY:-1}
      - NETDATA_TELEMETRY_DISABLE=${DISABLE_TELEMETRY:-1}
      - NETDATA_CLAIM_TOKEN=${NETDATA_CLAIM_TOKEN}
      - NETDATA_CLAIM_URL=${NETDATA_CLAIM_URL}
    # Optional: for deeper host visibility
    # pid: "host"
    # network_mode: host

volumes:
  netdataconfig:
  netdatalib:
```

#### Step 3: Deploy

```bash
docker-compose up -d
```

> ✅ Config and metric history persist.  
> ✅ Telemetry disabled by default.

---

## ☁️ Step-by-Step: Connect to Netdata Cloud Using `.env`

### 🔹 Step 1: Create Account
- Go to [https://app.netdata.cloud](https://app.netdata.cloud)
- Sign up with GitHub or email (free)

### 🔹 Step 2: Create a “Space”
- A **Space** = your monitoring group (e.g., “Production”, “Homelab”)
- After creating, note the **Space ID** from URL:  
  `https://app.netdata.cloud/spaces/abc123-def456...` → `abc123-def456...`

### 🔹 Step 3: Generate Claim Token
1. In your Space, click **“Add node”**
2. Choose **“Linux / Docker”**
3. Copy the **Claim Token** (looks like: `a1b2c3d4-5678-90ef-ghij-klmnopqrstuv`)

> ⚠️ **Token is shown only once!** Save it immediately.

### 🔹 Step 4: Update `.env`

```env
NETDATA_CLAIM_TOKEN=a1b2c3d4-5678-90ef-ghij-klmnopqrstuv
NETDATA_CLAIM_URL=https://app.netdata.cloud
```

### 🔹 Step 5: Redeploy

```bash
docker-compose down
docker-compose up -d
```

✅ Within 10 seconds, your node appears in Netdata Cloud!

> 🔒 **Security Note**:  
> - Claim token is **one-time use** (expires after first connection)  
> - All communication is **TLS-encrypted**  
> - **Raw metrics never leave your host**

---

## 🌐 Monitor Linux, Windows & Network Devices

### ➕ Linux Server
On target Linux machine:
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
sudo /usr/libexec/netdata/netdata-claim.sh \
  -token=YOUR_CLAIM_TOKEN \
  -rooms=YOUR_SPACE_ID \
  -url=https://app.netdata.cloud
```

### ➕ Windows Machine
1. Install [windows_exporter](https://github.com/prometheus-community/windows_exporter) on Windows.
2. On your Netdata host (Linux/Docker), enable Prometheus plugin:
   ```yaml
   # /etc/netdata/go.d/prometheus.conf
   jobs:
     - name: windows-office
       url: http://192.168.1.100:9182/metrics
   ```
3. Restart Netdata → metrics appear.

### ➕ Router / Switch (via SNMP)
1. Enable SNMP on device (community string = `public` or custom).
2. Add to Netdata:
   ```yaml
   # /etc/netdata/go.d/snmp.conf
   jobs:
     - name: cisco-router
       address: 192.168.1.1:161
       community: public
       options:
         interfaces:
           walk:
             - 1.3.6.1.2.1.2.2.1.10  # inOctets
             - 1.3.6.1.2.1.2.2.1.16  # outOctets
   ```
3. Restart Netdata.

---

## 🔗 References & Resources

- 📚 [Official Docs](https://learn.netdata.cloud/)
- 💻 [GitHub (Open-Source Agent)](https://github.com/netdata/netdata)
- ☁️ [Netdata Cloud](https://app.netdata.cloud)
- 🧪 [800+ Integrations List](https://learn.netdata.cloud/docs/agent/collectors)
- 📊 [Case Studies](https://www.netdata.cloud/customers/)

---

> 💡 **Final Thought**:  
> Netdata proves that **observability shouldn’t be complex, expensive, or slow**.  
> With **per-second truth**, **on-prem sovereignty**, and **AI that actually helps**, it’s the fastest path to “what’s wrong?” — even for lean teams.
