# 📘 **Phase 6 – Observability & Monitoring Design**

This phase ensures you **can see what’s happening in your system**, catch failures early, and maintain uptime.

------

## 🟢 1️⃣ Objectives

- Achieve **full visibility** into services, infrastructure, and APIs.
- Enable **proactive alerting** on failures and slowdowns.
- Provide **traceability** for debugging.
- Ensure **logs are centralized and searchable**.

------

## 🟢 2️⃣ Core Principles

✅ **Everything emits telemetry:**

- Metrics
- Logs
- Traces

✅ **Separation of concerns:**

- Application logs ≠ system logs ≠ audit logs.

✅ **Developer and Ops accessible:**

- Dashboards and logs easily queried by the team.

------

## 🟢 3️⃣ Observability Components

Below is your observability stack (Open Source + AWS):

| Capability              | Tool/Service                               |
| ----------------------- | ------------------------------------------ |
| **Metrics**             | Prometheus                                 |
| **Dashboards**          | Grafana                                    |
| **Logs**                | Fluent Bit → Elasticsearch (ELK)           |
| **Tracing**             | OpenTelemetry → Jaeger                     |
| **Alerting**            | Prometheus Alertmanager                    |
| **K8s Events**          | Kubernetes API + Event Exporter            |
| **AWS Cloud Resources** | CloudWatch (for underlying infrastructure) |



------

## 🟢 4️⃣ Metrics Strategy

✅ **Sources:**

- Broadleaf microservices (Spring Boot Actuator /metrics endpoints).
- PostgreSQL exporter.
- Kafka exporter.
- Solr exporter.
- Node Exporter (K8s nodes).

✅ **Key Metrics by Domain:**

| Domain                   | Examples                                        |
| ------------------------ | ----------------------------------------------- |
| **API Gateway**          | Request count, error rates, latency percentiles |
| **Cart Service**         | Active sessions, checkout attempts, error rates |
| **Order Service**        | Orders created, processing times, failures      |
| **Inventory Service**    | Low-stock events, sync delays                   |
| **Notification Service** | Emails sent, failures, retries                  |
| **DB**                   | Connections, query durations                    |
| **K8s**                  | Pod restarts, CPU/memory usage                  |



✅ **Retention:**

- High-resolution metrics: 30 days
- Aggregated metrics: 1 year

------

## 🟢 5️⃣ Logging Strategy

✅ **Approach:**

- All services log to stdout in **JSON format**.
- **Fluent Bit** collects logs, enriches with metadata (pod, namespace, environment).
- Logs shipped to **Elasticsearch**.

✅ **Retention:**

- 30 days online.
- Archived to S3 after.

✅ **Log Categories:**

- Application logs
- Access logs
- Audit logs (user actions)

✅ **Masking:**

- No passwords or sensitive PII in logs.

✅ **Example log snippet:**

```json
{
  "timestamp": "2025-07-04T08:30:00Z",
  "level": "INFO",
  "service": "order-service",
  "message": "Order created",
  "orderId": "12345",
  "customerId": "67890"
}
```

------

## 🟢 6️⃣ Tracing Strategy

✅ **Approach:**

- Use **OpenTelemetry SDK** in all microservices.
- Instrument HTTP clients and Kafka producers/consumers.
- Send traces to **Jaeger** backend.

✅ **Trace Example:**

- User initiates checkout:
  - API Gateway (span #1)
  - Cart Service (span #2)
  - Pricing Service (span #3)
  - Order Service (span #4)
  - Notification Service (span #5)

✅ **Benefits:**

- You can **see end-to-end latency**.
- Quickly identify which service is slow.

------

## 🟢 7️⃣ Alerting Strategy

✅ **Tool:**

- Prometheus Alertmanager

✅ **Alert Examples:**

| Alert                        | Threshold             |
| ---------------------------- | --------------------- |
| **API Gateway 5xx Rate**     | >1% over 5 min        |
| **Order Service Latency**    | >2 sec p95            |
| **DB Connection Saturation** | >80% connections used |
| **Pod Restart Loops**        | >3 restarts in 15 min |
| **Notification Failures**    | >5% failure rate      |



✅ **Notification Channels:**

- Slack
- Email
- (Optional) PagerDuty

------

## 🟢 8️⃣ Dashboards

✅ **Grafana Dashboards:**

- System Overview
- API Gateway Health
- Microservice Health
- JVM Metrics
- Database Health
- Kafka Health
- Order Lifecycle Metrics
- Notification Delivery Stats

------

## 🟢 9️⃣ Security of Observability Data

- Access to logs/metrics restricted to Admins and DevOps.
- Elasticsearch secured with RBAC.
- Jaeger requires authentication.
- Audit logs retained separately.

------

## 🟢 10️⃣ Observability Deployment

| Component                              | Deployment                 |
| -------------------------------------- | -------------------------- |
| **Prometheus + Alertmanager**          | Kubernetes pods            |
| **Grafana**                            | Kubernetes pod             |
| **Fluent Bit**                         | DaemonSet in Kubernetes    |
| **Elasticsearch**                      | AWS-managed or self-hosted |
| **Jaeger**                             | Kubernetes pod             |
| **Node Exporter / Kube-state-metrics** | Kubernetes pods            |



------

## 🟢 11️⃣ Deliverables for Phase 6

✅ Metrics architecture & exporters
 ✅ Logging pipeline (Fluent Bit → Elasticsearch)
 ✅ Tracing configuration
 ✅ Alert definitions
 ✅ Dashboards plan
 ✅ Security & access controls