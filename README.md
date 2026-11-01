# 🍔 Online Food Delivery App: Software Engineering Case Study

> A complete software engineering case study of a Zomato/Swiggy-style food delivery platform, covering the full SDLC from requirements to maintenance.

**Author:** Nirvair Singh
**Institution:** Chandigarh University
**Subject:** Software Engineering

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)
2. [Problem Statement](#2-problem-statement)
3. [Feasibility Study](#3-feasibility-study)
4. [SDLC Model Selection](#4-sdlc-model-selection)
5. [Requirements Analysis (SRS)](#5-requirements-analysis-srs)
6. [System Design](#6-system-design)
7. [Implementation](#7-implementation)
8. [Testing](#8-testing)
9. [Risk Management](#9-risk-management)
10. [Deployment and Maintenance](#10-deployment-and-maintenance)
11. [Project Estimation and Schedule](#11-project-estimation-and-schedule)
12. [Conclusion](#12-conclusion)
13. [References](#13-references)

---

## 1. Introduction

Online food delivery platforms connect **customers**, **restaurants**, and **delivery partners** through a single mobile/web application. Customers browse menus, place orders, pay online, and track delivery in real time.

This case study applies software engineering principles to plan, design, and validate such a system.

**Objectives**
- Identify and document functional and non-functional requirements
- Select a suitable SDLC model with justification
- Design the system using UML diagrams
- Define a testing and risk management strategy
- Plan deployment and maintenance

---

## 2. Problem Statement

Traditional food ordering relies on phone calls, which leads to:

| Problem | Impact |
|---|---|
| Order miscommunication | Wrong or missing items |
| No order tracking | Customer uncertainty |
| Limited restaurant reach | Lost revenue |
| Manual payment handling | Delays and errors |
| No feedback mechanism | Poor quality control |

**Proposed solution:** a centralized platform that automates ordering, payment, dispatch, and tracking for all stakeholders.

---

## 3. Feasibility Study

| Type | Assessment |
|---|---|
| **Technical** | Feasible using mature stacks (React Native, Node.js, PostgreSQL, Google Maps API, payment gateways such as Razorpay) |
| **Economic** | Revenue from commission (15 to 30%), delivery fees, and advertising; cost is mainly development, cloud hosting, and marketing |
| **Operational** | Users already familiar with app-based ordering; low training needed |
| **Schedule** | An MVP is achievable in 4 to 6 months with a small team |
| **Legal** | Must comply with food licensing (FSSAI in India), data protection, and payment regulations |

---

## 4. SDLC Model Selection

### Chosen model: **Agile (Scrum)**

| Criteria | Waterfall | Agile (Scrum) |
|---|---|---|
| Requirement changes | Hard to handle | Easily handled |
| Customer feedback | Only at the end | Every sprint |
| Time to first release | Long | Short (MVP early) |
| Risk | Discovered late | Discovered early |
| Fit for this project | ❌ | ✅ |

**Justification:** Food delivery requirements evolve quickly (new payment methods, offers, features). Agile allows incremental delivery, continuous feedback, and quick adaptation.

**Sprint plan (2-week sprints)**

```mermaid
gantt
    title Sprint Roadmap
    dateFormat  YYYY-MM-DD
    section Foundation
    Sprint 1 - Auth and User Management     :s1, 2026-01-05, 14d
    Sprint 2 - Restaurant and Menu Module   :s2, after s1, 14d
    section Core
    Sprint 3 - Cart and Order Placement     :s3, after s2, 14d
    Sprint 4 - Payment Integration          :s4, after s3, 14d
    Sprint 5 - Delivery Assignment          :s5, after s4, 14d
    Sprint 6 - Live Tracking                :s6, after s5, 14d
    section Release
    Sprint 7 - Ratings and Notifications    :s7, after s6, 14d
    Sprint 8 - Testing and Deployment       :s8, after s7, 14d
```

---

## 5. Requirements Analysis (SRS)

### 5.1 Stakeholders

| Actor | Role |
|---|---|
| **Customer** | Browses, orders, pays, tracks, reviews |
| **Restaurant Owner** | Manages menu, accepts/rejects orders |
| **Delivery Partner** | Accepts deliveries, updates status |
| **Admin** | Manages users, restaurants, disputes, analytics |

### 5.2 Functional Requirements

| ID | Requirement |
|---|---|
| FR1 | Users shall register and log in using email/phone with OTP verification |
| FR2 | Customers shall search restaurants by name, cuisine, or location |
| FR3 | Customers shall view menus, add items to the cart, and apply coupons |
| FR4 | The system shall support online payments (UPI, cards, wallets) and cash on delivery |
| FR5 | Restaurants shall accept or reject incoming orders and update preparation status |
| FR6 | The system shall automatically assign the nearest available delivery partner |
| FR7 | Customers shall track the order in real time on a map |
| FR8 | Customers shall rate and review restaurants and delivery partners |
| FR9 | The system shall send push/SMS notifications on order status changes |
| FR10 | Admin shall manage users, restaurants, offers, and view reports |

### 5.3 Non-Functional Requirements

| Category | Requirement |
|---|---|
| **Performance** | Page/API response under 2 seconds for 95% of requests |
| **Scalability** | Support 10,000+ concurrent users during peak hours |
| **Availability** | 99.9% uptime |
| **Security** | HTTPS, encrypted passwords (bcrypt), PCI-DSS compliant payments, JWT authentication |
| **Usability** | Order placement within 5 taps/clicks from the home screen |
| **Reliability** | No order or payment data loss on system failure |
| **Compatibility** | Android, iOS, and modern web browsers |
| **Maintainability** | Modular architecture with at least 80% unit test coverage |

### 5.4 User Stories (Sample)

- As a **customer**, I want to track my order live so that I know when to expect it.
- As a **restaurant owner**, I want to mark items out of stock so that customers can't order them.
- As a **delivery partner**, I want to see the optimal route so that I can deliver faster.
- As an **admin**, I want to view daily revenue reports so that I can monitor business performance.

---

## 6. System Design

### 6.1 Use Case Diagram

```mermaid
flowchart LR
    C([Customer])
    R([Restaurant])
    D([Delivery Partner])
    A([Admin])

    subgraph System["Food Delivery System"]
        U1(Register / Login)
        U2(Browse Restaurants)
        U3(Place Order)
        U4(Make Payment)
        U5(Track Order)
        U6(Rate and Review)
        U7(Manage Menu)
        U8(Accept / Reject Order)
        U9(Accept Delivery)
        U10(Update Delivery Status)
        U11(Manage Users and Restaurants)
        U12(View Reports)
    end

    C --> U1 & U2 & U3 & U4 & U5 & U6
    R --> U1 & U7 & U8
    D --> U1 & U9 & U10
    A --> U11 & U12
```

### 6.2 System Architecture

A **microservices-based, layered architecture** is used for scalability and independent deployment.

```mermaid
flowchart TB
    subgraph Clients
        MA[Customer App]
        RA[Restaurant App]
        DA[Delivery App]
        AD[Admin Web Panel]
    end

    GW[API Gateway<br/>Auth, Rate Limiting, Routing]

    subgraph Services["Microservices"]
        US[User Service]
        RS[Restaurant Service]
        OS[Order Service]
        PS[Payment Service]
        DS[Delivery Service]
        NS[Notification Service]
    end

    subgraph Data
        DB[(PostgreSQL)]
        CA[(Redis Cache)]
        MQ[[Message Queue]]
    end

    EXT1[Payment Gateway]
    EXT2[Maps API]
    EXT3[SMS / Push Provider]

    Clients --> GW --> Services
    Services --> DB
    Services --> CA
    Services <--> MQ
    PS --> EXT1
    DS --> EXT2
    NS --> EXT3
```

### 6.3 Data Flow Diagram (Level 1)

```mermaid
flowchart LR
    Customer -->|Order Request| P1[1.0 Order Management]
    P1 -->|Order Details| Restaurant
    Restaurant -->|Confirmation| P1
    P1 -->|Payment Request| P2[2.0 Payment Processing]
    P2 -->|Transaction Status| P1
    P1 -->|Delivery Request| P3[3.0 Delivery Management]
    P3 -->|Assignment| DeliveryPartner[Delivery Partner]
    DeliveryPartner -->|Location Updates| P3
    P3 -->|Live Status| Customer
    P1 <--> D1[(Orders DB)]
    P2 <--> D2[(Transactions DB)]
    P3 <--> D3[(Delivery DB)]
```

### 6.4 Class Diagram

```mermaid
classDiagram
    class User {
        +int userId
        +string name
        +string email
        +string phone
        +login()
        +logout()
    }
    class Customer {
        +string address
        +placeOrder()
        +trackOrder()
        +rateOrder()
    }
    class Restaurant {
        +int restaurantId
        +string name
        +string cuisine
        +updateMenu()
        +acceptOrder()
    }
    class DeliveryPartner {
        +int partnerId
        +string vehicleNo
        +bool isAvailable
        +acceptDelivery()
        +updateLocation()
    }
    class Order {
        +int orderId
        +datetime orderTime
        +float totalAmount
        +string status
        +calculateTotal()
        +updateStatus()
    }
    class OrderItem {
        +int quantity
        +float price
    }
    class MenuItem {
        +int itemId
        +string name
        +float price
        +bool available
    }
    class Payment {
        +int paymentId
        +string method
        +string status
        +processPayment()
        +refund()
    }

    User <|-- Customer
    User <|-- DeliveryPartner
    Customer "1" --> "*" Order : places
    Restaurant "1" --> "*" Order : receives
    Restaurant "1" --> "*" MenuItem : offers
    Order "1" *-- "*" OrderItem : contains
    OrderItem "*" --> "1" MenuItem
    Order "1" --> "1" Payment
    DeliveryPartner "1" --> "*" Order : delivers
```

### 6.5 Sequence Diagram: Placing an Order

```mermaid
sequenceDiagram
    actor C as Customer
    participant App
    participant OS as Order Service
    participant PS as Payment Service
    participant R as Restaurant
    participant DS as Delivery Service
    actor D as Delivery Partner

    C->>App: Select items and checkout
    App->>OS: createOrder()
    OS->>PS: initiatePayment()
    PS-->>OS: Payment successful
    OS->>R: Notify new order
    R-->>OS: Order accepted
    OS->>DS: Request delivery partner
    DS->>D: Assign delivery
    D-->>DS: Accepted
    DS-->>C: Live tracking and status updates
    D->>C: Delivers order
    C->>App: Rate order
```

### 6.5 State Diagram: Order Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Placed
    Placed --> PaymentPending
    PaymentPending --> Confirmed : Payment success
    PaymentPending --> Cancelled : Payment failed
    Confirmed --> Preparing : Restaurant accepts
    Confirmed --> Cancelled : Restaurant rejects
    Preparing --> ReadyForPickup
    ReadyForPickup --> OutForDelivery
    OutForDelivery --> Delivered
    Delivered --> [*]
    Cancelled --> [*]
```

### 6.6 Database Design (ER Diagram)

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    RESTAURANT ||--o{ ORDER : receives
    RESTAURANT ||--o{ MENU_ITEM : offers
    ORDER ||--|{ ORDER_ITEM : contains
    MENU_ITEM ||--o{ ORDER_ITEM : "included in"
    ORDER ||--|| PAYMENT : "paid by"
    DELIVERY_PARTNER ||--o{ ORDER : delivers
    CUSTOMER ||--o{ REVIEW : writes
    ORDER ||--o| REVIEW : has

    CUSTOMER {
        int customer_id PK
        string name
        string email
        string phone
        string address
    }
    RESTAURANT {
        int restaurant_id PK
        string name
        string cuisine
        string location
        float rating
    }
    MENU_ITEM {
        int item_id PK
        int restaurant_id FK
        string name
        float price
        boolean available
    }
    ORDER {
        int order_id PK
        int customer_id FK
        int restaurant_id FK
        int partner_id FK
        float total_amount
        string status
        datetime created_at
    }
    ORDER_ITEM {
        int order_id FK
        int item_id FK
        int quantity
        float price
    }
    PAYMENT {
        int payment_id PK
        int order_id FK
        string method
        string status
        datetime paid_at
    }
    DELIVERY_PARTNER {
        int partner_id PK
        string name
        string vehicle_no
        boolean available
    }
    REVIEW {
        int review_id PK
        int order_id FK
        int rating
        string comment
    }
```

---

## 7. Implementation

### 7.1 Technology Stack

| Layer | Technology |
|---|---|
| Mobile App | React Native / Flutter |
| Web Admin Panel | React.js |
| Backend | Node.js (Express) |
| Database | PostgreSQL, Redis (caching) |
| Messaging | RabbitMQ / Kafka |
| Maps and Tracking | Google Maps API, WebSockets |
| Payments | Razorpay / Stripe |
| Notifications | Firebase Cloud Messaging |
| Cloud | AWS / GCP |
| CI/CD | GitHub Actions, Docker |

### 7.2 Suggested Project Structure

```
food-delivery-app/
├── client/
│   ├── customer-app/
│   ├── restaurant-app/
│   └── delivery-app/
├── services/
│   ├── user-service/
│   ├── restaurant-service/
│   ├── order-service/
│   ├── payment-service/
│   ├── delivery-service/
│   └── notification-service/
├── api-gateway/
├── docs/
├── docker-compose.yml
└── README.md
```

### 7.3 Key Design Principles Applied

- **Separation of concerns:** each microservice owns one business domain
- **SOLID principles** in the service layer
- **Design patterns:** Observer (order status notifications), Strategy (payment methods), Factory (user types)
- **Version control:** Git with feature-branch workflow and pull request reviews

---

## 8. Testing

### 8.1 Testing Levels

| Level | Description | Tools |
|---|---|---|
| Unit | Individual functions/classes | Jest, JUnit |
| Integration | Interaction between services | Postman, Supertest |
| System | End-to-end workflows | Selenium, Cypress |
| Performance | Load and stress | JMeter, k6 |
| Security | Vulnerability scanning | OWASP ZAP |
| UAT | Real-user validation | Beta testers |

### 8.2 Sample Test Cases

| ID | Scenario | Input | Expected Result | Status |
|---|---|---|---|---|
| TC01 | Valid login | Correct email + OTP | User logged in | Pass |
| TC02 | Invalid OTP | Wrong OTP | Error message shown | Pass |
| TC03 | Add item to cart | Select menu item | Cart total updated | Pass |
| TC04 | Apply valid coupon | Valid code | Discount applied | Pass |
| TC05 | Apply expired coupon | Expired code | "Coupon expired" message | Pass |
| TC06 | Payment failure | Insufficient funds | Order not confirmed, retry offered | Pass |
| TC07 | Restaurant rejects order | Reject action | Customer notified, refund initiated | Pass |
| TC08 | Live tracking | Order out for delivery | Map shows partner location | Pass |
| TC09 | Peak load | 10,000 concurrent users | Response time under 2 s | Pass |
| TC10 | SQL injection attempt | Malicious input in search | Input sanitized, request blocked | Pass |

---

## 9. Risk Management

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Payment gateway downtime | Medium | High | Support multiple gateways with fallback |
| Server overload at peak hours | High | High | Auto-scaling, caching, load balancing |
| Data breach | Low | Very High | Encryption, regular audits, access control |
| Poor GPS accuracy | Medium | Medium | Combine GPS with network location, manual correction |
| Scope creep | High | Medium | Sprint planning, backlog prioritization |
| Delivery partner shortage | Medium | High | Incentives, dynamic pricing, demand forecasting |
| Third-party API changes | Low | Medium | Abstraction layer around external APIs |

---

## 10. Deployment and Maintenance

### Deployment
- Containerized with **Docker**, orchestrated using **Kubernetes**
- **CI/CD pipeline:** commit → build → automated tests → staging → production
- Blue-green deployment for zero-downtime releases

### Maintenance Types

| Type | Example |
|---|---|
| **Corrective** | Fixing a bug in coupon calculation |
| **Adaptive** | Supporting a new OS version or payment method |
| **Perfective** | Improving search speed and UI |
| **Preventive** | Refactoring code, upgrading dependencies |

### Monitoring
Prometheus + Grafana for metrics, ELK stack for logs, and alerting for downtime or error spikes.

---

## 11. Project Estimation and Schedule

### COCOMO (Basic, Semi-detached mode)

Assume the estimated size is **30 KLOC**.

- Effort = a × (KLOC)^b = 3.0 × (30)^1.12 ≈ **134 person-months**
- Development time = c × (Effort)^d = 2.5 × (134)^0.35 ≈ **14 months**
- Average team size = Effort / Time ≈ **10 people**

> For an MVP with reduced scope, this shrinks to roughly 4 to 6 months with a team of 5 to 6.

### Team Structure

| Role | Count |
|---|---|
| Product Owner / Project Manager | 1 |
| Backend Developers | 2 |
| Mobile/Frontend Developers | 2 |
| QA Engineer | 1 |
| DevOps Engineer | 1 |
| UI/UX Designer | 1 |

---

## 12. Conclusion

This case study shows how software engineering practices can be applied to build a food delivery platform: choosing **Agile** to handle changing requirements, documenting clear **functional and non-functional requirements**, modelling the system with **UML**, planning multi-level **testing**, and managing **risks** proactively.

**Key learnings**
- Early requirement analysis prevents costly rework
- A modular microservices design improves scalability and maintainability
- Continuous testing and CI/CD improve quality and release speed
- Risk planning is essential for systems that depend on third-party services

**Future enhancements:** AI-based recommendations, drone/robot delivery, subscription plans, voice ordering, and demand prediction.

---

## 13. References

1. Pressman, R. S., *Software Engineering: A Practitioner's Approach*, McGraw-Hill
2. Sommerville, I., *Software Engineering*, Pearson
3. Schwaber, K. and Sutherland, J., *The Scrum Guide*
4. IEEE Std 830-1998, *Recommended Practice for Software Requirements Specifications*
5. Fowler, M., *UML Distilled*, Addison-Wesley

---

⭐ If you found this case study helpful, consider starring the repository!
