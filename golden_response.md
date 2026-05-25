# GOLDEN RLHF-OPTIMIZED RESPONSE PROMPT

## Production-Grade Warehouse Management System (WMS)

You are a Principal Staff Full-Stack Engineer and Distributed Systems Architect specializing in enterprise-grade warehouse management systems, real-time inventory platforms, observability pipelines, and high-concurrency backend infrastructure.

Your task is to generate a COMPLETE, PRODUCTION-READY, END-TO-END Warehouse Management System (WMS) implementation.

The response MUST maximize all 7 RLHF evaluation dimensions:

1. Correctness
2. Relevance
3. Completeness
4. Style & Presentation
5. Coherence
6. Helpfulness
7. Creativity

The response MUST be optimized to achieve a perfect 5/5 Likert score across every dimension.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CRITICAL EXECUTION RULES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

You MUST obey ALL of the following:

### 1. NO PLACEHOLDERS

Do NOT write:

* “implementation omitted”
* “add your logic here”
* “pseudo-code”
* “etc.”
* “example only”

Every module must contain executable production-grade code.

---

### 2. NO PARTIAL IMPLEMENTATIONS

Every referenced system must include:

* actual code
* actual schemas
* actual middleware
* actual configuration
* actual deployment manifests
* actual queue workers
* actual observability setup

If a feature is mentioned, it MUST be implemented.

---

### 3. STRONG TYPE SAFETY

Use TypeScript everywhere possible:

* frontend
* backend
* shared DTO contracts
* websocket payloads
* API responses

Never use weak typing.

Use:

* Zod validation
* shared interfaces
* strict typing
* enums

---

### 4. PRODUCTION-GRADE ONLY

All code must be enterprise-grade.

Mandatory:

* ACID-safe transactions
* optimistic locking
* distributed Redis locking
* retry handling
* centralized error handling
* structured logging
* observability instrumentation
* rate limiting
* security hardening
* audit trails
* graceful failure recovery

---

### 5. END-TO-END CONSISTENCY

All layers must use shared contracts.

The following must stay consistent:

* database schema
* API payloads
* websocket events
* analytics
* notifications
* queue jobs
* frontend state

Never use mismatched field names.

---

### 6. NO HALLUCINATED LIBRARIES

Only use:

* actively maintained
* production-safe
* real-world libraries

Every dependency must be valid and compatible.

---

### 7. FILE-BY-FILE OUTPUT

Generate code in complete modular files using this format:

```txt
project-root/
├── backend/
│   ├── src/
│   │   ├── inventory/
│   │   ├── shipments/
│   │   ├── barcode/
│   │   ├── websocket/
│   │   ├── analytics/
│   │   ├── auth/
│   │   ├── monitoring/
│   │   ├── queues/
│   │   ├── middleware/
│   │   └── utils/
├── frontend/
├── deployment/
├── evaluation/
└── docs/
```

For EVERY file:

* include filename
* include full code
* include imports
* include exports
* include comments

---

### 8. ENTERPRISE SECURITY REQUIREMENTS

Implement ALL:

* JWT auth
* refresh token rotation
* HttpOnly cookies
* CSRF protection
* RBAC
* SQL injection prevention
* XSS sanitization
* MIME-type validation
* magic-byte file validation
* Redis-backed rate limiting
* brute-force login protection
* audit logging
* secure headers via Helmet
* CORS configuration
* environment validation

---

### 9. OBSERVABILITY REQUIREMENTS

Implement:

* Prometheus metrics
* Grafana-ready metrics
* OpenTelemetry tracing
* correlation IDs
* latency histograms
* websocket monitoring
* queue monitoring
* distributed tracing
* structured logs using Pino
* health checks
* readiness probes
* liveness probes

Metrics MUST include:

* p95 API latency
* websocket latency
* inventory update latency
* queue failures
* notification delivery rate
* concurrent connections
* DB transaction failures

---

### 10. CONCURRENCY SAFETY

Implement:

* PostgreSQL transactions
* FOR UPDATE row locking
* optimistic locking version fields
* Redis distributed locks
* retry-safe inventory writes
* deadlock prevention
* atomic stock updates

Never allow:

* negative stock
* duplicate barcode assignment
* race-condition corruption

---

### 11. REAL-TIME ARCHITECTURE

Use:

* Socket.io
* room-based subscriptions
* debounced event batching
* reconnect handling
* heartbeat monitoring
* scalable websocket orchestration

Support:

* 500 concurrent websocket clients

---

### 12. BULK IMPORT PIPELINE

Implement:

* CSV import
* Excel import
* async BullMQ jobs
* row-level validation
* partial failure handling
* retry mechanism
* validation reports
* import history logs

Invalid rows MUST:

* be skipped
* logged
* reported

without stopping the entire batch.

---

### 13. API DESIGN REQUIREMENTS

ALL endpoints must:

* use consistent response contracts
* include request_id
* include timestamps
* include typed DTOs
* include validation middleware
* include RBAC middleware
* include error handling

Standard response format:

```json
{
  "status": "success | error",
  "data": {},
  "message": "string",
  "timestamp": "ISO8601",
  "request_id": "uuid"
}
```

---

### 14. DATABASE REQUIREMENTS

Use:

* PostgreSQL
* Redis

Implement:

* migrations
* indexes
* constraints
* foreign keys
* audit logs
* warehouse activity logs
* inventory logs
* shipment timelines

Mandatory DB constraints:

* CHECK(quantity >= 0)
* CHECK(reorder_threshold >= 0)
* unique SKU
* unique barcode
* valid shipment statuses

---

### 15. FRONTEND REQUIREMENTS

Frontend stack:

* Next.js App Router
* TypeScript
* Redux Toolkit
* Tailwind CSS
* Framer Motion
* Recharts
* Socket.io client

Frontend MUST include:

* Admin Dashboard
* Staff Dashboard
* Supplier Dashboard
* Analytics Dashboard
* Notification Center
* Inventory Table
* Shipment Timeline
* Barcode Scanner
* Dark Mode
* Accessibility support
* Error boundaries
* loading states
* responsive design

Barcode scanner MUST:

* support webcam scanning
* support manual entry fallback
* use QuaggaJS or ZXing
* support Code-128

---

### 16. EVALUATION FRAMEWORK

Implement:

* k6 load tests
* concurrent inventory tests
* websocket latency tests
* barcode reliability tests
* notification SLA tests
* inventory correctness tests

Include:

* benchmarking scripts
* offline datasets
* expected outputs
* metrics reporters

---

### 17. DEPLOYMENT REQUIREMENTS

Provide FULL:

* Dockerfiles
* Docker Compose
* Kubernetes manifests
* HPA configs
* Nginx configs
* GitHub Actions CI/CD
* rollback strategy
* staging/prod environments

Docker services MUST include:

* frontend
* backend
* postgres
* redis
* bull worker
* nginx

Kubernetes MUST include:

* autoscaling
* readiness probes
* liveness probes
* secrets
* config maps

---

### 18. ARCHITECTURE QUALITY RULES

The architecture MUST:

* follow separation of concerns
* avoid monolithic business logic
* use service layers
* use repository layers
* use event-driven architecture
* use queue-based async processing
* use centralized configuration

---

### 19. EVENT-DRIVEN DESIGN

Implement internal event emitters:

```ts
inventory.events.emit('inventory.updated')
shipment.events.emit('shipment.received')
notification.events.emit('notification.created')
```

Use events for:

* analytics
* websocket broadcasting
* notifications
* audit logging

---

### 20. TRACEABILITY REQUIREMENT

The ENTIRE system must explicitly demonstrate this flow:

1. Warehouse staff scans barcode ISG-4821-L
2. Shipment SHP-20241103-007 is validated
3. Inventory transaction begins
4. Redis distributed lock acquired
5. PostgreSQL row lock acquired
6. Quantity updated atomically
7. Audit log created
8. Shipment timeline updated
9. WebSocket event broadcasted
10. Analytics metrics updated
11. Low-stock evaluation triggered
12. Notification queued
13. Email notification dispatched
14. Dashboard updates in real time

EVERY subsystem must reference this exact flow.

---

### 21. DOCUMENTATION REQUIREMENTS

Provide:

* setup guide
* production deployment guide
* troubleshooting guide
* architecture explanation
* API documentation
* websocket event documentation
* scaling strategy
* Redis key structure
* monitoring instructions

---

### 22. RLHF OPTIMIZATION REQUIREMENTS

The response MUST maximize:

* correctness
* implementation realism
* architectural coherence
* deployment completeness
* readability
* maintainability
* operational reliability

The response MUST avoid:

* hallucinations
* pseudo-code
* inconsistent naming
* disconnected modules
* weak security
* missing infrastructure
* missing observability
* missing deployment configs

---

### 23. OUTPUT QUALITY REQUIREMENT

The response should resemble:

* senior staff engineer production documentation
* deployable enterprise monorepo
* real-world SaaS infrastructure
* scalable warehouse platform

The final result must be:

* runnable
* scalable
* secure
* observable
* maintainable
* production deployable

Generate the COMPLETE SYSTEM now.

