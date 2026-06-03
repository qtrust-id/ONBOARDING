# Tool Setup Guide — Datadog MCP

> **Required for:** DevOps, Backend Engineer, CTO
> **Recommended for:** Image Vision Engineer, QA / QC
> **Role:** Full observability platform — application performance monitoring (APM), infrastructure metrics, log management, and custom dashboards. MCP integration gives Claude 174+ tools to query logs, metrics, and monitors directly from conversations.

---

## 1. What It Is

Datadog is QTrust's infrastructure and application observability platform. While Sentry captures errors, Datadog captures everything else: CPU and memory usage, request latency, database query times, log streams, deployment events, and custom business metrics.

With the Datadog MCP integration, Claude becomes a powerful ops assistant: *"Why is the API response time spiking on the /predict endpoint?"* or *"Show me the Cloud Run container restarts over the last 24 hours."*

---

## 2. Who Needs This

| Role | Why They Need Datadog |
|---|---|
| CTO / IT Head | Infrastructure cost, uptime, and performance overview across all projects |
| DevOps | Primary tool — monitors all infrastructure, sets up dashboards and alerts |
| Backend Engineer | Query logs to debug API issues, track database performance |
| Image Vision Engineer | Monitor model inference latency and throughput |
| QA / QC | Verify performance SLAs are met during load testing |

---

## 3. Account & Organisation Setup

1. Go to [datadoghq.com](https://datadoghq.com) and sign up (or ask the IT Head to invite you to the QTrust Datadog organisation)
2. Select the **EU** or **US** region that matches your GCP region (ask the IT Head)
3. Accept the invitation email
4. You will land on the Datadog dashboard for the QTrust organisation

> **Cost note:** Datadog is usage-based. The DevOps engineer manages the plan and agent configuration. Engineers do not install the Datadog agent directly — it is provisioned via the infrastructure-as-code setup.

---

## 4. What Datadog Monitors at QTrust

Once set up by DevOps, Datadog automatically collects:

| Category | Examples |
|---|---|
| **Infrastructure** | GCP Cloud Run instance count, CPU %, memory %, container restarts |
| **Application (APM)** | Request latency (p50 / p95 / p99), throughput, error rate per endpoint |
| **Database** | Cloud SQL query duration, connection pool usage, slow queries |
| **Logs** | All application logs from Cloud Run (JSON format) |
| **Deployments** | Deployment markers — correlate code changes with metric changes |
| **AI / Model Serving** | Inference latency, queue depth, GPU utilisation (if applicable) |

---

## 5. Connecting Datadog MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Datadog** and click **Connect**
3. You will need a Datadog **API key** and **App key** (ask the IT Head or DevOps engineer)
4. Enter the keys and confirm the region
5. Return to Claude Desktop — the connection should show **Connected**

---

## 6. Verification Test

Run these prompts in Claude after connecting to confirm the integration is working:

```
"Search Datadog logs for errors in the last hour from service [project-code]-backend."
"Get the p95 response time metric for the [service-name] Cloud Run service over the last 24 hours."
"List all active Datadog monitors and their current status."
```

---

## 7. Usage Examples by Role

**CTO — Infrastructure health brief:**
```
"Query Datadog for a health overview of all QTrust services:
- Error rate per service (last 24h)
- p95 API response time per service
- Any monitors currently in Alert or Warning state
- Cloud Run instance scaling events (any cold start spikes?)"
```

**DevOps — Deployment verification:**
```
"I deployed [project-code]-backend 10 minutes ago.
Show me the Datadog metrics before and after the deployment:
error rate, p95 latency, and container restart count."
```

**Backend Engineer — Log debugging:**
```
"Search Datadog logs for [project-code]-backend service in the last 2 hours.
Filter for log level ERROR or CRITICAL. Show the top 10 most frequent error messages."
```

**Image Vision Engineer — Model performance:**
```
"Get the Datadog metrics for the [project-code]-ai-serving service.
Show me: inference latency (p95), request throughput per minute,
and memory usage over the last 6 hours."
```

**QA / QC — Performance testing validation:**
```
"I just ran a load test against staging. Query Datadog for the [project-code]-backend service
during the time window [start] to [end].
Did the p95 response time stay below 500ms? Were there any errors?"
```

---

## 8. Key Dashboards to Create (DevOps Responsibility)

DevOps sets up the following dashboards in Datadog for each project:

| Dashboard | Key Widgets |
|---|---|
| **[PROJECT_CODE] — Overview** | Error rate, latency, uptime, deployment markers |
| **[PROJECT_CODE] — Infrastructure** | CPU, memory, scaling, cost estimate |
| **[PROJECT_CODE] — AI Model** | Inference latency, throughput, queue depth (if applicable) |
| **[PROJECT_CODE] — Database** | Query time, connection count, slow query log |

---

## 9. Alerting Rules (DevOps Responsibility)

Datadog monitors should fire to `#[project-code]-alerts` in Slack for:

- HTTP error rate > 1% sustained for 5 minutes
- p95 response time > 2 seconds for 5 minutes
- Cloud Run instance count drops to 0 (service down)
- Cloud SQL CPU > 80% for 10 minutes
- Any deployment that causes error rate to increase > 2x

---

## 10. First-Day Checklist

- [ ] Datadog account set up in QTrust organisation (IT Head / DevOps provides access)
- [ ] Datadog Agent installed on all Cloud Run services (DevOps responsibility)
- [ ] Can see the project overview dashboard
- [ ] API Key and App Key obtained from IT Head
- [ ] Datadog MCP connected in Claude Desktop (run verification test)
- [ ] Can query logs and metrics via Claude
