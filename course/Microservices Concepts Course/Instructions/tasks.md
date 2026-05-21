# Progressive Programming Challenges for Spring Boot Microservices (E-Commerce Example)

This file defines **lesson-by-lesson coding challenges**, where each lesson **builds on the previous one**. The example project is an e-commerce system with `UserService`, `OrderService`, `InventoryService`, and `PaymentService`.

---
## Instructions:
1. For each microservice, create **Spring Boot code**: main class, controller, service, repository if needed, configuration (application.yml).
2. Implement endpoints exactly as described in the lesson tasks (e.g., GET /users/{id}, POST /orders, POST /inventory/check, etc.).
3. Include **service registration and discovery** if mentioned (Eureka/Consul), with minimal working configuration.
4. Include **sync or async calls**, Kafka/RabbitMQ consumers/producers, and Resilience4j patterns as introduced in lessons.
5. Include **example data responses** in JSON where applicable.
6. Use **progressive approach**: do not add functionality that hasn’t been introduced yet. Implement lesson by lesson.
7. Provide all code in **separate files/sections** clearly labeled by service and class name.
8. Output should be **copy-paste ready** Spring Boot code, with minimal necessary dependencies.

## Lesson 01: Microservices Fundamentals

**Objective:** Split a monolith into basic microservices.

**Tasks:**
1. Create **UserService**
    - Endpoint: `GET /users/{id}` → returns user details `{id, name, email}`
    - Endpoint: `POST /users` → creates a user, returns created user
2. Create **OrderService**
    - Endpoint: `GET /orders/{id}` → returns order details `{id, userId, items[]}`
    - Endpoint: `POST /orders` → creates an order, returns created order
3. Ensure services run on **different ports** (e.g., UserService: 8081, OrderService: 8082)
4. Test REST endpoints using Postman or curl

---

## Lesson 02: Service Discovery & Registration

**Objective:** Add service discovery using Eureka or Consul.

**Tasks:**
1. Set up **Eureka Server**
    - Register both `UserService` and `OrderService`
2. Modify services to be **Eureka Clients**
3. Add **DiscoveryClient usage in OrderService**
    - `GET /orders/users/{userId}` → fetch user details from UserService dynamically using `DiscoveryClient`
4. Test service registration and discovery
    - UserService and OrderService should appear in Eureka dashboard

---

## Lesson 03: API Gateway & Edge Routing

**Objective:** Introduce Spring Cloud Gateway for routing.

**Tasks:**
1. Create **Gateway Service**
    - Routes:
        - `/users/**` → `UserService`
        - `/orders/**` → `OrderService`
2. Test API routing via gateway
    - `GET /users/1` via gateway returns user details
    - `POST /orders` via gateway creates order
3. Add **simple filter** to log all requests passing through the gateway

---

## Lesson 04: Sync vs Async Communication

**Objective:** Implement synchronous and asynchronous calls.

**Tasks:**
1. In OrderService, make **synchronous call** to UserService to validate `userId` on order creation
    - Use **RestTemplate** or **OpenFeign**
2. Create **InventoryService**
    - Endpoint: `POST /inventory/check` → accepts `items[]`, returns `{inStock: true/false}`
3. Modify OrderService to **call InventoryService synchronously** before saving order
4. Introduce **asynchronous messaging with Kafka**
    - When order is created, publish `OrderCreatedEvent` to Kafka
    - InventoryService consumes this event and logs it (later we will deduct stock)

---

## Lesson 05: Fault Tolerance with Resilience4j

**Objective:** Add fault tolerance to service calls.

**Tasks:**
1. Wrap **UserService and InventoryService calls** in Resilience4j circuit breakers
2. Add **retry** policy for InventoryService call
3. Add **fallback** method for UserService (return placeholder user if service is down)
4. Demonstrate **timeout** for slow InventoryService responses
5. Verify circuit breaker and fallback behavior by temporarily stopping services

---

## Lesson 06: Configuration Management

**Objective:** Centralize configuration.

**Tasks:**
1. Create **Spring Cloud Config Server**
    - Store configs in Git repository
2. Externalize service ports, Kafka topics, inventory thresholds
3. Modify all services to be **Config Clients**
4. Use **@RefreshScope** to allow runtime refresh
5. Test config update by changing a value in Git and refreshing services

---

## Lesson 07: Messaging with Kafka/RabbitMQ

**Objective:** Enhance async messaging and reliability.

**Tasks:**
1. When OrderService creates an order:
    - Publish `OrderCreatedEvent` to Kafka
2. InventoryService consumes the event
    - Deduct stock and publish `InventoryUpdatedEvent`
3. PaymentService consumes `OrderCreatedEvent`
    - Processes payment and publishes `PaymentProcessedEvent`
4. Add **Dead Letter Queue (DLQ)** for failed messages
5. Implement **retry logic** for consumers

---

## Lesson 08: Observability & Tracing

**Objective:** Add metrics, logs, and distributed tracing.

**Tasks:**
1. Integrate **Micrometer** in all services
    - Track order creation count, inventory updates, payment success
2. Centralized logging: log requests/responses in JSON format
3. Implement **OpenTelemetry tracing**
    - Trace full order flow: OrderService → InventoryService → PaymentService
4. Visualize metrics and traces in Grafana/Jaeger (optional)

---

## Lesson 09: Data Consistency Patterns

**Objective:** Ensure eventual consistency for distributed transactions.

**Tasks:**
1. Implement **Saga Pattern** (Choreography)
    - OrderService creates order → InventoryService deducts stock → PaymentService processes payment
    - If payment fails, InventoryService should rollback stock deduction
2. Use **outbox/inbox pattern** for reliable event delivery
3. Ensure **idempotency** in event consumers
4. Demonstrate eventual consistency with a test scenario:
    - Fail PaymentService temporarily and ensure stock deduction is rolled back or corrected

---

# Notes:

- Each lesson builds on the **previous microservices** and their endpoints.
- Student ends up with a **complete e-commerce microservices system**.
- Copilot can expand each lesson into **HTML teaching slides + code snippets**.
