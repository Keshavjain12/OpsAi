# Changelog

All notable changes to OpsAI are documented here.

---

## [v1.0.0] — 2025

### 🚀 Initial Release

#### AI Agent Layer
- Gemini 2.5 Flash integration with function calling (tool use)
- 7 built-in tools: `restart_pod`, `scale_deployment`, `trigger_pipeline`, `get_cluster_status`, `get_logs`, `acknowledge_alert`, `rollback_deployment`
- Streaming token responses via SSE
- Dynamic live system prompt injection with real cluster state
- Multi-turn conversation with full chat history
- Spring Boot `/api/ai` passthrough with Gemini fallback
- AI Confidence Indicator (High / Medium / Low)
- kubectl command detection with special copy button

#### Overview Dashboard
- Global Cluster Health Score — animated SVG arc gauge
- Live metric cards: RPS, P99 Latency, Error Rate, Active Incidents
- Mini sparklines per metric card with delta indicators
- 3 real-time SVG sparkline charts updating every 3s
- Service Health Grid with 4 service cards
- Live Incident Event Feed with slide-in animation
- AI Quick Prompts row

#### CI/CD Pipelines
- 7-stage pipeline visualizer with shimmer animation on running stages
- Pipeline table and build history with CSV export
- Re-run, cancel, view details actions

#### Kubernetes
- Node table with animated SVG arc gauges for CPU/memory
- Pod health grid (Running / Warning / OOMKilled)
- kubectl Runner modal

#### Deployments
- Service version tracking with replica counts
- Deployment timeline
- Scale, restart, rollback actions

#### Logs & Traces
- Live log streaming every 2.5s
- Filter by service and log level
- Pause/resume, export, clear

#### Alerts
- Active alerts with severity, description, age
- Alert rules engine (OOMKill, CPU, Memory, TLS Expiry)
- Acknowledge and bulk ack

#### Databases
- 4 database instances: PostgreSQL, Redis, MongoDB, InfluxDB
- Connections, size, replication lag

#### Networking
- VPC, Load Balancer (ALB/NLB), DNS Records
- RPS and latency per load balancer

#### Secrets Manager
- 18 secrets with namespace, type, age, rotation policy
- Rotate action with Spring Boot API integration

#### AI Insights (Advanced)
- Auto-analysis on page load with 3 proactive insights
- Anomaly detection on sparkline data with alert banners
- AI Runbook Generator with streaming output

#### Cost Intelligence (Advanced)
- Monthly cost estimate breakdown
- Resource waste detector
- AI Cost Optimizer with streaming kubectl/Terraform commands

#### Security Posture (Advanced)
- Security Score gauge with breakdown
- Threat feed with suspicious event detection
- CIS Kubernetes Benchmark checklist with AI Audit

#### Terminal Emulator (Advanced)
- kubectl, helm, docker, top commands
- Command history (↑/↓), tab completion
- AI Terminal — analyze last 5 commands with Gemini

#### Incident Management (Advanced)
- Kanban board: Detecting → Investigating → Resolving
- AI Incident Commander with streaming root cause + mitigation
- Incident timeline

#### Auth & Security
- JWT Bearer token auth with Spring Boot backend
- RBAC: admin / developer / viewer roles
- Session countdown timer with auto-logout
- Demo mode: offline with full mock data

#### UI/UX
- 3 themes: Deep Dark / Midnight / Charcoal
- Command Palette (Cmd+K) with fuzzy search
- Collapsible sidebar with icon-only mode
- Breadcrumb navigation
- NProgress top loading bar
- Keyboard shortcuts (G+D, G+P, G+K, /)
- Notification center with bell icon
- Count-up number animations
- Staggered table row animations
- Particle canvas on login screen
- CSV export on all tables
- Mobile responsive with hamburger drawer

---

## Upcoming

- [ ] WebSocket real-time backend events
- [ ] Prometheus metrics integration
- [ ] Multi-cluster support
- [ ] Slack / PagerDuty notifications
- [ ] OpenTelemetry trace viewer
