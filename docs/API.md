OpsAI — Backend API Reference
OpsAI connects to a Spring Boot backend. All endpoints below are called with a JWT Bearer token. If the backend is unreachable, the frontend falls back to offline mock data automatically.
Base URL
```
http://localhost:8080
```
Change this at runtime: click the backend URL in the login screen.
---
Authentication
POST `/api/auth/login`
```json
Request:
{
  "username": "admin",
  "password": "admin123"
}

Response 200:
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "username": "admin",
  "role": "admin"
}

Response 401:
{
  "error": "Invalid credentials"
}
```
POST `/api/auth/register`
```json
Request:
{
  "username": "newuser",
  "password": "password123",
  "email": "user@company.com"
}

Response 200:
{
  "message": "User created successfully"
}
```
---
AI
POST `/api/ai`
```json
Request:
{
  "question": "Why is api-gateway OOMKilling?",
  "context": "production-k8s",
  "history": [
    { "role": "user", "content": "previous message" },
    { "role": "assistant", "content": "previous response" }
  ]
}

Response 200:
{
  "answer": "The api-gateway pods are OOMKilling because..."
}
```
---
Metrics
GET `/api/metrics`
```json
Response 200:
{
  "nodeCount": "6",
  "podCount": "142/144",
  "namespaces": "8",
  "issues": "2",
  "services": "4",
  "replicas": "28",
  "uptime": "99.7%",
  "pending": "1",
  "nodes": [
    {
      "name": "worker-node-01",
      "role": "worker",
      "cpu": 42,
      "memory": 61,
      "pods": "24/30",
      "status": "healthy"
    }
  ]
}
```
---
Pipelines
GET `/api/pipelines`
```json
Response 200:
[
  {
    "name": "api-service",
    "branch": "main",
    "lastRun": "4m ago",
    "duration": "6m 52s",
    "status": "running"
  }
]
```
POST `/api/pipelines/{name}/run`
Triggers a new pipeline run.
```json
Response 200:
{ "message": "Pipeline triggered" }
```
---
Deployments
POST `/api/deployments/{name}/scale`
```json
Request:
{ "replicas": 8 }

Response 200:
{ "message": "Scaling initiated" }
```
POST `/api/deployments/{name}/restart`
```json
Response 200:
{ "message": "Restart initiated" }
```
---
Logs
GET `/api/logs`
Returns an array of recent log entries.
```json
Response 200:
[
  {
    "timestamp": "14:24:01",
    "service": "api-gateway",
    "level": "ERROR",
    "message": "OOMKilled: container exceeded memory limit 256Mi"
  }
]
```
GET `/api/logs/stream`
Returns the single latest log entry (polled every 2.5s).
```json
Response 200:
{
  "timestamp": "14:24:05",
  "service": "worker",
  "level": "INFO",
  "message": "Job completed in 120ms"
}
```
---
Secrets
POST `/api/secrets/{name}/rotate`
```json
Response 200:
{ "message": "Rotation initiated for DATABASE_URL" }
```
---
Auth Headers
Every request (except `/api/auth/*`) must include:
```
Authorization: Bearer <jwt_token>
Content-Type: application/json
```
The JWT token is stored in `localStorage` as `opsai_token` after login.
---
Error Responses
Status	Meaning
401	Token missing or expired — frontend auto-logs out
403	Insufficient role permissions
404	Resource not found
500	Backend error — frontend falls back to mock data
