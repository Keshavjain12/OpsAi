⚡ OpsAI — DevOps Intelligence Platform
> An AI-powered DevOps command center that combines a full Kubernetes operations dashboard with a Gemini LLM agent capable of diagnosing incidents, generating runbooks, and taking autonomous actions on your infrastructure.
![Platform](https://img.shields.io/badge/Platform-Web%20App-6c63ff?style=flat-square)
![AI](https://img.shields.io/badge/AI-Gemini%202.5%20Flash-00d4aa?style=flat-square)
![Auth](https://img.shields.io/badge/Auth-JWT%20%2B%20Spring%20Boot-ff6b6b?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-ffd93d?style=flat-square)
![Deploy](https://img.shields.io/badge/Deploy-Netlify%20%7C%20Vercel%20%7C%20GitHub%20Pages-blue?style=flat-square)
---
🎯 What is OpsAI?
OpsAI is a single-file, zero-dependency DevOps Intelligence Platform built for the intersection of Large Language Models and DevOps/SRE engineering. It gives platform engineers and SREs an AI-first interface to monitor, debug, and operate Kubernetes infrastructure — all from one browser tab.
The AI assistant is not just a chatbot. It is a tool-calling agent that can read live cluster state, execute kubectl-equivalent actions, generate incident runbooks, detect anomalies in real time, and respond to natural language commands like:
> *"Why is api-gateway OOMKilling? Restart the broken pod and scale the deployment to 10 replicas."*
---
✨ Key Features
🧠 LLM Agent Layer
Gemini 2.5 Flash integration with function calling (tool use)
AI can autonomously call: `restart_pod`, `scale_deployment`, `trigger_pipeline`, `get_cluster_status`, `get_logs`, `acknowledge_alert`, `rollback_deployment`
Streaming token responses — output appears word by word like ChatGPT
Dynamic system prompt injection — every AI call includes live cluster state (pod counts, active alerts, pipeline stage, node CPU/memory)
AI Confidence Indicator — High / Medium / Low badge on every response
kubectl command detection — special copy button for kubectl blocks in AI chat
Multi-turn conversation with full chat history
Spring Boot `/api/ai` backend passthrough with Gemini fallback
📊 Overview Dashboard
Global Cluster Health Score — animated SVG arc gauge (0–100)
Live metric cards — Total RPS, P99 Latency, Error Rate, Active Incidents (auto-updating)
Mini sparklines per metric card showing recent trend + delta indicator
3 real-time SVG sparkline charts — RPS, Error Rate, Latency (30 data points, updates every 3s)
Service Health Grid — 4 service cards with status, uptime, and mini health bars
Live Incident Event Feed — scrolling feed of cluster events with slide-in animation
AI Quick Prompts — one-click send for common DevOps queries
🔁 CI/CD Pipelines
7-stage pipeline visualizer (Build → Unit Tests → SAST → Docker Push → Deploy Staging → Smoke Tests → Deploy Prod)
Shimmer animation on running stages
Real-time pipeline advancement simulation
Pipeline table + build history with CSV export
Re-run, cancel, and view details actions
📦 Deployments
Service version tracking with replica counts
Deployment timeline (from → to version, by whom)
Scale and restart actions with Spring Boot API integration
Rollback modal with downtime warning
🔍 Logs & Traces
Live log streaming every 2.5s (real backend or mock)
Filter by service and log level (ERROR / WARN / INFO / DEBUG)
Pause/Resume stream, export logs, clear
Colorized log levels with service tags
🔔 Alerts
Active alerts table with severity, description, age
Alert rules engine (OOMKill, CPU, Memory, TLS Expiry)
Acknowledge actions + bulk ack
Alert rule editor
☁️ Kubernetes
Node table with SVG arc gauges for CPU and memory (animated on load)
Pod grid with health status (Running / Warning / OOMKilled)
kubectl Runner modal with pre-filled commands
EKS us-east-1 cluster context
🗄️ Databases
4 database instances: PostgreSQL 15, Redis 7.2, MongoDB 6, InfluxDB 2
Connections, size, replication lag per instance
Backup All action
🌐 Networking
VPC, Load Balancer (ALB/NLB), DNS Records table
RPS, P99 latency per load balancer
DNS record editor
🔐 Secrets Manager
18 secrets with namespace, type, age, rotation policy
Rotate secret action (Spring Boot API or offline mock)
Audit report generation
💡 AI Insights (Advanced)
Auto-analysis on page load — 3 proactive AI-generated insights (root cause, cost optimization, predicted next alert)
Anomaly detection — watches sparkline data, fires banner when error spike / latency spike / RPS drop detected
AI Runbook Generator — streams a full incident response runbook from Gemini
💰 Cost Intelligence (Advanced)
Monthly cost estimate breakdown (Compute / Storage / Network)
Resource waste detector — under-utilized pods, over-provisioned memory
AI Cost Optimizer — streams specific kubectl/Terraform commands to reduce spend
🛡️ Security Posture (Advanced)
Security Score gauge with breakdown (secrets rotation, TLS health, pod policies, failed auth)
Threat feed — suspicious IPs, secret access anomalies, root pod detection, unusual egress
CIS Kubernetes Benchmark checklist (10 items) with AI Audit capability
›_ Terminal Emulator (Advanced)
Full fake kubectl terminal with green-on-black styling and blinking cursor
Supported commands: `kubectl get pods`, `kubectl describe pod`, `kubectl logs`, `kubectl scale`, `kubectl rollout status`, `helm list`, `docker ps`, `top`, `clear`, `help`
Command history (↑/↓ arrow keys)
Tab completion for pod and service names
AI Terminal — sends last 5 commands to Gemini and streams back "what to run next"
📋 Incident Management (Advanced)
Kanban board — Detecting → Investigating → Resolving columns
Create incidents from any alert
AI Incident Commander — streams root cause, mitigation steps, long-term fix, ETA per incident
Incident timeline with event log
---
🏗️ Architecture
```
┌─────────────────────────────────────────────────────────┐
│                    OpsAI Frontend                        │
│              Single HTML File (index.html)               │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐ │
│  │  Dashboard  │  │  AI Agent    │  │  DevOps Views  │ │
│  │  Overview   │  │  (Gemini)    │  │  K8s/CI/Logs   │ │
│  └─────────────┘  └──────┬───────┘  └────────────────┘ │
└─────────────────────────┼───────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
  ┌──────────────┐ ┌────────────┐ ┌────────────────┐
  │ Spring Boot  │ │  Gemini    │ │  Offline Mock  │
  │  Backend     │ │  2.5 Flash │ │  Data Layer    │
  │  (Java)      │ │  API       │ │  (Fallback)    │
  └──────────────┘ └────────────┘ └────────────────┘
  /api/auth/login   Function        All data
  /api/auth/register Calling        simulated
  /api/ai           Streaming       locally
  /api/metrics      SSE tokens
  /api/pipelines
  /api/logs
  /api/deployments
  /api/secrets
```
---
🚀 Getting Started
Option 1 — Just open it (no setup needed)
```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/opsai.git
cd opsai

# Open directly in browser
open index.html
```
On first load, click "⚡ admin / admin123" for the quick demo login. The backend will not be found, and it will ask if you want Offline Demo Mode — click OK. Everything works with mock data.
Option 2 — With Spring Boot backend
```bash
# 1. Start your Spring Boot backend (default port 8080)
# Required endpoints: /api/auth/login, /api/auth/register, /api/ai,
#                     /api/metrics, /api/pipelines, /api/logs,
#                     /api/deployments, /api/secrets

# 2. Open index.html
# 3. Login with your backend credentials
# 4. It will use real API data automatically
```
Option 3 — Deploy to the web (free)
Netlify (30 seconds):
Go to netlify.com/drop
Drag and drop `index.html`
Live instantly
GitHub Pages:
Push this repo to GitHub
Settings → Pages → Source: main branch
Live at `https://USERNAME.github.io/opsai`
---
🔑 Connecting Your Gemini API Key
Get a free key at aistudio.google.com/app/apikey
On first AI chat message, you'll be prompted to enter it
The key is saved in `localStorage` — never sent anywhere except directly to Google's API
The app tries Gemini models in order: `gemini-2.5-flash` → `gemini-2.5-flash-lite` → `gemini-1.5-flash-latest`
---
🔐 Authentication
User	Password	Role	Access
`admin`	`admin123`	Admin	Full access — restart pods, rollback, manage secrets
`dev`	`dev123`	Developer	Trigger pipelines, view all — no secrets or rollback
Any	Any	Viewer	Read-only — all action buttons disabled
JWT tokens are decoded in-browser to extract role and expiry. A session countdown timer shows in the topbar. Auto-logout on expiry.
---
⌨️ Keyboard Shortcuts
Shortcut	Action
`G` then `D`	Go to Dashboard
`G` then `P`	Go to Pipelines
`G` then `K`	Go to Kubernetes
`/`	Focus AI chat
`Cmd/Ctrl + K`	Open Command Palette
`↑` / `↓`	Navigate terminal history
`Tab`	Autocomplete in terminal
---
🎨 Themes
Switch themes using the palette icon in the topbar:
Theme	Description
Deep Dark	Default — deep purple-black with violet accents
Midnight	Navy blue with sky blue accents
Charcoal	Warm dark gray with orange accents
---
🧰 Tech Stack
Layer	Technology
Frontend	Vanilla HTML5 / CSS3 / JavaScript (zero frameworks)
AI Model	Google Gemini 2.5 Flash (function calling + SSE streaming)
Backend (optional)	Spring Boot (Java) with JWT security
Fonts	JetBrains Mono + Syne (Google Fonts)
Charts	Pure SVG (no chart libraries)
Auth	JWT Bearer tokens via Spring Boot or demo mode
Deployment	Any static host (Netlify, Vercel, GitHub Pages, Cloudflare)
---
📁 Project Structure
```
opsai/
└── index.html          # Entire application — ~3400 lines
                        # CSS design system (CSS variables, 3 themes)
                        # HTML layout (15 views/sections)
                        # JavaScript (AI agent, mock data, all logic)
```
This is an intentional single-file architecture for maximum portability. No build step, no `npm install`, no node_modules. Open and run anywhere.
---
🤖 AI Agent — How Function Calling Works
When you type a message like "restart the crashed api-gateway pod", OpsAI:
Builds a live system prompt with current pod/alert/pipeline state from the UI
Sends the message + tools list to Gemini 2.5 Flash API
Gemini returns a `functionCall` object: `{ name: "restart_pod", args: { pod_name: "api-gateway-5d8c9b-mn4r1", namespace: "api-gateway" } }`
OpsAI executes the JS function — updates pod status to "Restarting" in the UI
Shows an AI Action bubble confirming what was done
Sends the function result back to Gemini for a final natural language response
Streams the response token by token into the chat bubble
Available tools:
```javascript
restart_pod(pod_name, namespace)
scale_deployment(service_name, replicas)
trigger_pipeline(pipeline_name, branch)
get_cluster_status()
get_logs(service_name, level)
acknowledge_alert(alert_title)
rollback_deployment(service_name, version)
```
---
🗺️ Roadmap
[ ] WebSocket connection for true real-time backend events
[ ] Prometheus metrics integration
[ ] Multi-cluster support (switch between EKS clusters)
[ ] Slack / PagerDuty alert notifications
[ ] Terraform plan preview panel
[ ] GitHub Actions pipeline integration
[ ] OpenTelemetry trace viewer
[ ] AI-generated Helm chart recommendations
---
🙏 Acknowledgements
Google Gemini API — LLM backbone with function calling
JetBrains Mono — terminal font
Syne — UI heading font
Built for the Emerging Tools Lab — LLM × DevOps track
---
📄 License
MIT License — free to use, modify, and distribute.
---
<div align="center">
  <strong>Built with ⚡ by OpsAI</strong><br/>
  <sub>LLM + DevOps = The future of platform engineering</sub>
</div>
