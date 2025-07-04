## 📝 2️⃣ High-Level Requirements

I am structuring this in **functional**, **non-functional**, and **integration** categories.

------

### 🟢 2.1 Functional Requirements

1. **Product Catalog**
   - Category and product hierarchy
   - SKU management
   - Inventory quantities
   - Pricing, including:
     - Base price
     - Discounted price
     - Campaign price
2. **Shopping Cart**
   - Persistent carts per user
   - Guest carts
   - Real-time inventory reservation
3. **Order Management**
   - Checkout workflow
   - Payment processing
   - Order state machine (e.g., created, paid, shipped, delivered, returned)
4. **Campaign and Promotions**
   - Percentage or fixed discounts
   - Early delivery incentives
   - Coupons and voucher support
   - Time-bound promotions
   - Campaign prioritization rules
5. **Stock Management**
   - Real-time inventory updates
   - Multi-warehouse support
   - Stock threshold alerts
6. **Delivery Tracking**
   - Carrier integration APIs
   - Customer notifications (SMS/Email)
   - ETA calculations
   - Proof of delivery recording
7. **User Management**
   - Customer registration
   - Profile management
   - Role-based access for internal staff
8. **Reporting**
   - Sales reports
   - Inventory movement reports
   - Campaign effectiveness reports
   - Delivery performance reports

------

### 🟡 2.2 Non-Functional Requirements

1. **Scalability**
   - Must handle seasonal peaks
   - Kubernetes auto-scaling ready
2. **Performance**
   - Page load under 2 seconds
   - API latency below 300 ms p99
3. **Availability**
   - 99.9% uptime target
4. **Observability**
   - Centralized logging (e.g., ELK or OpenTelemetry)
   - Metrics and alerts
5. **Security**
   - OWASP Top 10 compliance
   - PCI-DSS readiness for payments
   - Secure API authentication (OAuth2/JWT)
6. **Data Consistency**
   - Strong consistency for inventory and orders
   - Eventual consistency acceptable for reporting
7. **Extensibility**
   - Plugin mechanisms for future campaigns
   - Modular microservices architecture

------

### 🟣 2.3 Integration Requirements

1. **Payment Gateways**
   - Support for at least 2 major gateways
2. **Carrier APIs**
   - For delivery tracking
3. **ERP/CRM**
   - Sync stock and customer data
4. **Notification Providers**
   - SMS
   - Email
   - Push notifications
5. **Analytics Platform**
   - Data export to data warehouse

------

### 🔵 2.4 Deployment & Operations

1. **Containerization**
   - All custom Java services in Docker
2. **Kubernetes**
   - Helm charts for deployment
   - ConfigMap/Secret management
3. **CI/CD**
   - Git-based workflows
   - Automated testing pipelines
4. **Environment Strategy**
   - DEV, QA, STAGING, PROD
   - Blue/Green or Canary deployments