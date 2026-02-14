## How would you monitor and troubleshoot a Lambda microservice using CloudWatch Logs, metrics, and X-Ray tracing?

I use a layered approach: **CloudWatch metrics for detection**, **CloudWatch Logs for evidence**, and **X-Ray for root-cause isolation across services**.

### 1. Detect issues early with CloudWatch metrics and alarms

I monitor these core Lambda metrics:
- `Errors` and error rate (`Errors / Invocations`)
- `Duration` (average + p95/p99)
- `Throttles`
- `ConcurrentExecutions`
- `IteratorAge` (for stream/event consumers)
- `DeadLetterErrors` and queue depth (if SQS/DLQ is used)

Then I set alarms (for example):
- Error rate above 1-2% for 5 minutes
- p95 duration near timeout threshold
- Any throttles in production
- Growing DLQ or retry backlog

This gives fast signal before users report problems.

### 2. Use CloudWatch Logs for request-level debugging

I keep logs structured (JSON) and include:
- `correlationId` / request ID
- function name and version
- key business identifiers (orderId, userId, tenantId)
- external dependency outcomes (DB/API status, latency)
- error type + stack trace

With CloudWatch Logs Insights, I can quickly:
- find top error types
- filter by correlation ID for one failing request
- compare duration/error spikes to deployment windows

### 3. Use X-Ray to pinpoint latency and dependency failures

I enable active tracing and propagate correlation IDs. In X-Ray I use:
- **Service map** to see which dependency is failing
- **Trace timeline** to find where latency is spent (Lambda init, DB, HTTP call)
- **Annotations** (tenant, endpoint, environment) to filter traces fast

This is especially useful when logs show a symptom but not the bottleneck.

### 4. Practical troubleshooting workflow (interview-ready)

1. Alarm fires (high error rate or latency).
2. Check CloudWatch metrics to confirm scope and timing.
3. Query Logs Insights for impacted requests and dominant error pattern.
4. Pull trace samples in X-Ray for same time range/correlation IDs.
5. Identify root cause: code regression, timeout, downstream API, DB saturation, or throttling.
6. Mitigate: rollback via alias, increase memory/concurrency, fix dependency issue, or adjust retry/DLQ behavior.
7. Add/adjust alarms and dashboards so the same failure is caught earlier next time.

### 5. What I’d mention for full-stack context

- Tie frontend/API errors to backend traces using a shared correlation ID.
- Build one dashboard with API Gateway + Lambda + downstream dependency metrics.
- Use canary/weighted alias deployments and monitor error/latency deltas before full rollout.

This approach gives both **fast detection** and **clear root-cause analysis** in production.
