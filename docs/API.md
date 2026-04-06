OpsAI — AI Agent Architecture
Overview
OpsAI uses Google Gemini 2.5 Flash as its LLM backbone. The AI is not a simple chatbot — it is a tool-calling agent that can read live cluster state and execute DevOps actions autonomously.
---
Function Calling Flow
```
User types: "Restart the crashed api-gateway pod"
      │
      ▼
1. BUILD LIVE SYSTEM PROMPT
   ┌─────────────────────────────────────────────────────┐
   │ You are OpsAI, a DevOps AI agent.                   │
   │ Current cluster state:                              │
   │  - 142/144 pods healthy                             │
   │  - 2 OOMKilled: api-gateway-5d8c9b-mn4r1           │
   │  - Pipeline #247 running at stage 4/7               │
   │  - worker-node-03 at 87% CPU                        │
   │  - 3 active alerts (1 critical)                     │
   └─────────────────────────────────────────────────────┘
      │
      ▼
2. SEND TO GEMINI with tools array
   POST https://generativelanguage.googleapis.com/v1beta/
        models/gemini-2.5-flash:generateContent
   {
     messages: [...history, { role: "user", content: "Restart..." }],
     tools: [{ function_declarations: [...] }]
   }
      │
      ▼
3. GEMINI RETURNS functionCall
   {
     "functionCall": {
       "name": "restart_pod",
       "args": {
         "pod_name": "api-gateway-5d8c9b-mn4r1",
         "namespace": "api-gateway"
       }
     }
   }
      │
      ▼
4. FRONTEND EXECUTES the JS function
   restartPod("api-gateway-5d8c9b-mn4r1", "api-gateway")
   → Updates pod status to "Restarting" in UI
   → Shows AI Action bubble in chat
      │
      ▼
5. SEND RESULT BACK TO GEMINI
   { role: "tool", content: "Pod restarted successfully. 
     Status: Restarting. ETA: ~30s" }
      │
      ▼
6. GEMINI STREAMS final response
   "I've restarted api-gateway-5d8c9b-mn4r1. 
    The pod is now in Restarting state and should 
    be Running within ~30 seconds. I recommend also 
    increasing the memory limit from 256Mi to 512Mi 
    to prevent future OOMKills."
```
---
Available Tools
`restart_pod(pod_name, namespace)`
Restarts a specific pod. Updates pod status in the UI to "Restarting".
`scale_deployment(service_name, replicas)`
Scales a deployment to the specified replica count. Updates the deployments table.
`trigger_pipeline(pipeline_name, branch)`
Triggers a CI/CD pipeline run. Sets pipeline status to "running" in the UI.
`get_cluster_status()`
Returns a snapshot of current cluster state: pod health, node metrics, active alerts, pipeline status.
`get_logs(service_name, level)`
Returns filtered log entries from the current log buffer for a specific service and level.
`acknowledge_alert(alert_title)`
Marks an alert as acknowledged in the alerts table.
`rollback_deployment(service_name, version)`
Triggers a deployment rollback to the specified version.
---
Streaming Responses
When the AI returns a text response (not a function call), OpsAI streams it token by token using Gemini's SSE API:
```
URL: .../gemini-2.5-flash:streamGenerateContent?alt=sse&key=KEY

Each SSE chunk:
data: {"candidates":[{"content":{"parts":[{"text":"I've "}]}}]}
data: {"candidates":[{"content":{"parts":[{"text":"restarted "}]}}]}
data: {"candidates":[{"content":{"parts":[{"text":"the pod..."}]}}]}
```
Tokens are appended to the chat bubble in real time. A blinking cursor `▌` shows while streaming is in progress.
---
Live System Prompt Injection
Before every Gemini API call, the system prompt is dynamically built from current UI state:
```javascript
function buildSystemPrompt() {
  const healthyPods = pods.filter(p => p.s === 'healthy').length;
  const totalPods = pods.length;
  const criticalAlerts = alerts.filter(a => a.sev === 'red').length;
  const avgCpu = Math.round(nodes.reduce((s,n) => s + n.c, 0) / nodes.length);
  
  return `You are OpsAI, an expert DevOps and SRE AI agent with tool-calling capabilities.
  
Current cluster: production-k8s · EKS · us-east-1
Pods: ${healthyPods}/${totalPods} healthy
Alerts: ${criticalAlerts} critical, ${alerts.length} total
Pipeline: Build #247 at stage ${stageIdx}/7
Avg CPU: ${avgCpu}% · Active context: ${activeContext}

You can call tools to take real actions. Always confirm what you did after calling a tool.
Be concise. Use code blocks for commands.`;
}
```
---
AI Confidence Levels
After each response, OpsAI shows a confidence badge:
Level	Condition
High	AI used function calling — took a real action
Medium	AI had live cluster state in context
Low	Generic response without cluster context
---
Proactive AI Features
Auto-Analysis (AI Insights page)
On page load (3s delay), OpsAI automatically calls Gemini with the full cluster state and asks for 3 insights: root cause analysis, cost optimization, and predicted next alert. No user prompt required.
Anomaly Detection
Every 30 seconds, the frontend checks sparkline data for:
Error rate > 2× 5-minute baseline
Latency > 150ms
RPS drop > 30% from average
When triggered, a flashing banner appears and an auto-analysis is sent to Gemini.
AI Runbook Generator
Sends current alert details to Gemini and streams back a structured incident response runbook with numbered steps, kubectl commands, and ETA.
