# 📘 **Phase 5 – Data & Reporting Design**

This phase defines **how your platform organizes, accesses, and exposes data for operational and reporting needs**.
 Because Broadleaf already provides a robust data model, our focus is on:

- Understanding **what is out-of-the-box** vs. **custom reporting**.
- Deciding **how to access data safely for reporting**.
- Specifying **API contracts for your reporting UI**.

Let’s proceed step by step.

------

## 🟢 1️⃣ Objectives

- Provide **accurate, timely reports** for business and operations.
- Avoid performance impact on transactional workloads.
- Ensure reports are **secure and role-restricted**.
- Enable future **extensibility** (more reports later).

------

## 🟢 2️⃣ Data Model Overview

✅ **Broadleaf Data Model (simplified):**

| Domain         | Key Tables                                             |
| -------------- | ------------------------------------------------------ |
| **Catalog**    | `blc_product`, `blc_category`, `blc_sku`               |
| **Inventory**  | `blc_inventory_location`, `blc_inventory`              |
| **Orders**     | `blc_order`, `blc_order_item`, `blc_fulfillment_group` |
| **Customers**  | `blc_customer`, `blc_customer_address`                 |
| **Pricing**    | `blc_offer`, `blc_offer_code`, `blc_price_list`        |
| **Promotions** | `blc_offer`, `blc_offer_audit`                         |
| **Audit**      | `blc_audit`                                            |



✅ **Notes:**

- Broadleaf uses **Hibernate/JPA**, so tables are mapped to entities.
- You can **extend these entities** if you need custom fields.
- For most reports, **read-only queries** are sufficient.

------

## 🟢 3️⃣ Reporting Data Architecture

✅ **Approach:**

| Aspect            | Decision                                                     |
| ----------------- | ------------------------------------------------------------ |
| **Data Source**   | Direct PostgreSQL read replica or same DB (with caution)     |
| **Access Method** | Reporting Service queries data via JPA / SQL                 |
| **Consistency**   | Near-real-time acceptable (slight lag if using replicas)     |
| **Security**      | Read-only DB user credentials, limited to SELECT permissions |



------

✅ **Why Not ETL?**
 Since you only need basic operational reports, full ETL/data warehouse is **out of scope** for now.

------

## 🟢 4️⃣ Reporting Service Responsibilities

**This microservice:**

- Exposes REST APIs for reporting.
- Queries PostgreSQL directly.
- Transforms results into JSON.
- Enforces **OAuth2 role-based access** (`ROLE_ADMIN`).
- Provides **pagination and filtering**.

------

## 🟢 5️⃣ Example Reporting APIs

Below are **example endpoints**, each with purpose and parameters:

| Endpoint                               | Purpose                             | Query Parameters                     |
| -------------------------------------- | ----------------------------------- | ------------------------------------ |
| `GET /api/reports/sales`               | Show total sales and orders per day | `startDate`, `endDate`               |
| `GET /api/reports/inventory/low-stock` | List SKUs below threshold           | `threshold` (optional)               |
| `GET /api/reports/orders/status`       | Count orders by status              | `startDate`, `endDate`               |
| `GET /api/reports/campaigns`           | Show coupon redemptions             | `campaignId`, `startDate`, `endDate` |



------

### Example API Response

**Sales Report**

```json
{
  "startDate": "2024-06-01",
  "endDate": "2024-06-30",
  "results": [
    {"date": "2024-06-01", "orderCount": 15, "totalRevenue": 3200.00},
    {"date": "2024-06-02", "orderCount": 12, "totalRevenue": 2800.50}
  ]
}
```

------

## 🟢 6️⃣ Data Consistency Considerations

✅ **Approach:**

- For inventory and orders, **read committed** isolation is sufficient.
- For sales aggregation, expect minor timing gaps (e.g., in-progress orders).
- Clear disclaimers in reports: *"Data updated every 5 minutes."*

------

## 🟢 7️⃣ Reporting Data Security

✅ **Principles:**

- All APIs require `ROLE_ADMIN`.
- No customer PII exposed unless necessary.
- Sensitive fields (payment info) **excluded**.

✅ **DB Credentials:**

- Use **read-only DB user**.
- Stored in Kubernetes Secrets.

------

## 🟢 8️⃣ Data Retention & Archival

| Data Type           | Retention Policy                                             |
| ------------------- | ------------------------------------------------------------ |
| Order Data          | Minimum 3 years                                              |
| Audit Logs          | Minimum 1 year                                               |
| Inventory Snapshots | 1 year                                                       |
| Reports             | Generated on-demand (no long-term storage in reporting service) |



------

## 🟢 9️⃣ Reporting UI Design

**Frontend:**

- **ReactJS Admin UI** consuming your reporting APIs.
- Authentication via OAuth2.

✅ **Features:**

- Date range filters.
- Table views with export (CSV/Excel).
- Charts for trends.

------

## 🟢 10️⃣ Observability & Audit

✅ All API access logged:

- User identity.
- Timestamps.
- Parameters.

✅ Metrics:

- Request counts and latencies.
- Error rates.

✅ Traces:

- Query execution time per report request.

------

## 🟢 11️⃣ Deliverables for Phase 5

✅ Data model mapping (Broadleaf tables)
 ✅ Reporting Service API specs
 ✅ Data consistency approach
 ✅ Security and retention policies
 ✅ Reporting UI integration plan