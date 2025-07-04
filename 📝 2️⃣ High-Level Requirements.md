## 📝 2️⃣ High-Level Requirements

I am structuring this in **functional**, **non-functional**, and **integration** categories.

------

### 🟢 2.1 Functional Requirements

1. **Product Catalog** ( below all off the shelf)
   - Category and product hierarchy
   - SKU management
   - Inventory quantities
   - Pricing, including:
     - Base price
     - Discounted price
     - Campaign price
2. **Shopping Cart** ( below all off the shelf)
   - Persistent carts per user
   - Guest carts
   - Real-time inventory reservation
3. **Order Management**  ( below all off the shelf)
   - Checkout workflow
   - Payment processing
   - Order state machine (e.g., created, paid, shipped, delivered, returned)
4. **Campaign and Promotions** ( below all off the shelf)
   - Percentage or fixed discounts
   - Early delivery incentives
   - Coupons and voucher support
   - Time-bound promotions
   - Campaign prioritization rules
5. **Stock Management** ( to integrate with external ERP)
   - Real-time inventory updates
   - Multi-warehouse support
   - Stock threshold alerts
6. **Delivery Tracking**
   - Carrier integration APIs  (to integrate with external delivery system)
   - Customer notifications (SMS/Email) ( to use AWS tools)
   - ETA calculations  (to integrate with external delivery system)
   - Proof of delivery recording  (to integrate with external delivery system)
7. **User Management**  ( to integrate with external ERP)
   - Customer registration
   - Profile management
   - Role-based access for internal staff
8. **Reporting** ( custom report through API to reactjs,  a simple admin domain to login and access these reports)
   1. Pulled live from the operational DB
   2. Sales reports
   3. Inventory movement reports
   4. Campaign effectiveness reports
   5. Delivery performance reports


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
   - **mTLS for internal service communication** (Kubernetes best practice)
   - **IAM roles for AWS resources**
6. **Data Consistency**
   - Strong consistency for inventory and orders
   - Eventual consistency acceptable for reporting
7. **Extensibility**
   - Plugin mechanisms for future campaigns
   - Modular microservices architecture

------

### 🟣 2.3 Integration Requirements

1. **Payment Gateways** ( Moving to next version of development, now focus only on ecommerce off the shelf, ERP off the shelf, reporting development, integration development)
   - Support for at least 1 major gateway preferred MTN MoMo.
2. **Carrier APIs**   ( Moving to next version of development, now focus only on ecommerce off the shelf, ERP off the shelf, reporting development, integration development)
   - For delivery tracking
3. **ERP/CRM** (we need to choose the ERP and CRM which is very minimal for our ecommerce and opensource)
   - Sync stock and customer data
4. **Notification Providers** ( Use AWS (SES/SNS) and Firebase for notifications)
   - SMS
   - Email
   - Push notifications

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
   - DEV, QA,  PROD ( removed Staging directly to PROD from QA)
   - Blue/Green or Canary deployments



🔵 2.5 Development environment and PROD environment

1. **Development and QA** most of them in docker and kubernetes
   - we are using eclipse IDE, minikube, docker, postgreSQL(docker), JDK 17, github, gitaction, open telemetry, jeager, graffana, prometheus further we will discuss and finalise 
2. **Production**
   - All AWS based platform and services. But match exact with dev model whereever possible
   - CI/CD from github through gitactions to AWS deployment. 