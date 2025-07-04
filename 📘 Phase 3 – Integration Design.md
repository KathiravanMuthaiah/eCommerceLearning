# 📘 Phase 3 – Integration Design

------

## 🟢 1️⃣ Objectives of Phase 3

- Define **how our platform talks to other systems**.
- Specify **APIs, contracts, and data flows**.
- Clarify **responsibility boundaries** (who owns which data).
- Make sure integrations are **secure, observable, and resilient**.

------

## 🟢 2️⃣ Scope of Integrations

Since you decided to **defer payments and carrier APIs**, this phase covers only **in-scope integrations**:

| #    | Integration Type                 | Purpose                               |
| ---- | -------------------------------- | ------------------------------------- |
| 1    | Reporting APIs                   | Provide data to ReactJS Admin UI      |
| 2    | Notification Service Integration | Send emails/SMS to customers          |
| 3    | Search Service                   | Serve search queries via Solr         |
| 4    | (Optional) Webhooks              | For future ERP/CRM or analytics hooks |



------

## 🟢 3️⃣ Reporting API Design

✅ **Why?**
 Broadleaf’s built-in reports are limited; you need **custom REST APIs** to feed your reporting UI.

✅ **Features:**

- Sales reports (order totals, revenue)
- Inventory reports (current stock, low-stock alerts)
- Campaign performance (redemption rates)
- Delivery metrics (from order status)

✅ **Approach:**

- Create a **Reporting Service** microservice.
- This service **queries the Broadleaf PostgreSQL DB** (read-only).
- Exposes REST endpoints secured by OAuth2/JWT.

✅ **Example REST API Endpoints:**

| Endpoint                                           | Purpose                |
| -------------------------------------------------- | ---------------------- |
| `GET /api/reports/sales?startDate=...&endDate=...` | Sales totals by date   |
| `GET /api/reports/inventory/low-stock`             | Low-stock SKUs         |
| `GET /api/reports/campaigns?campaignId=...`        | Campaign effectiveness |
| `GET /api/reports/orders/status`                   | Orders in each state   |



✅ **Response format:**

- JSON, suitable for React data tables.
- Paging and filtering included.

✅ **Security:**

- Only authenticated admin users (role-based policy).
- JWT validation in Reporting Service.

------

## 🟢 4️⃣ Notification Service Integration

✅ **Why?**
 While Broadleaf includes email templates, you want **AWS SNS/SES and Firebase** to actually send notifications.

✅ **Approach:**

- Develop a **Notification Service** microservice.
- Other services (Order, Customer) **publish events to Kafka**, e.g.,
  - `OrderCreated`
  - `OrderShipped`
  - `PasswordResetRequested`
- Notification Service **consumes Kafka events**, then:
  - For email: uses **AWS SES SDK**
  - For SMS: uses **AWS SNS SDK**
  - (Optional) Firebase for push notifications.

✅ **Event Flow Example:**

```
mermaidCopyEditsequenceDiagram
    participant Order Service
    participant Kafka
    participant Notification Service
    participant AWS SES/SNS

    Order Service->>Kafka: Publish OrderCreated event
    Kafka->>Notification Service: Consume OrderCreated event
    Notification Service->>AWS SES: Send order confirmation email
    Notification Service->>AWS SNS: Send SMS notification
```

✅ **Fallback Handling:**

- Failed sends retried up to 3 times.
- After final failure, log error and optionally alert Ops.

✅ **Security:**

- Notification Service uses AWS IAM credentials securely stored in Kubernetes Secrets.
- No customer contact data in logs.

------

## 🟢 5️⃣ Search Service Integration (Solr)

✅ **Why?**
 Catalog and search are separate concerns—Solr index needs to stay current.

✅ **Approach:**

- Broadleaf automatically updates Solr via its microservices when products change.
- API Gateway forwards search queries to Search Service.
- No additional development needed unless you want advanced search faceting or custom ranking.

✅ **API Example:**

- `GET /api/search?q=keyword&category=shoes&sort=price`

------

## 🟢 6️⃣ (Optional) Webhooks

- Not considered

------

## 🟢 7️⃣ Integration Security

**Common Security Principles:**

- All APIs secured with **OAuth2 JWT**.
- Service-to-service traffic secured with **mTLS**.
- Customer data protected in transit (TLS 1.2+).
- Secrets stored in Kubernetes Secrets or AWS Secrets Manager.

------

## 🟢 8️⃣ Observability for Integrations

| Aspect      | Strategy                                                     |
| ----------- | ------------------------------------------------------------ |
| **Tracing** | OpenTelemetry spans for all outbound calls (SES, SNS, Solr)  |
| **Logging** | JSON logs with request IDs and correlation IDs               |
| **Metrics** | Prometheus metrics: request counts, error counts, latency histograms |



------

## 🟢 9️⃣ Failure Handling & Resilience

| Integration        | Failure Handling                                             |
| ------------------ | ------------------------------------------------------------ |
| **Reporting APIs** | Return `503` if DB unavailable; React UI retries             |
| **Notifications**  | Retry 3 times, fallback to DLQ (dead letter queue)           |
| **Search**         | Return empty results if Solr down (with user-friendly message) |



------

## 🟢 10️⃣ Deliverables for Phase 3

✅ Reporting Service API contracts
 ✅ Notification Service event flows
 ✅ Integration security model
 ✅ Observability approach
 ✅ Failure handling strategy