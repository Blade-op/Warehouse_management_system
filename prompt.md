# Warehouse Management System (WMS)

```text
Warehouse Management System (WMS)
Complete System Prompt — Production Grade

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Context and Role
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

As a Full-Stack Engineer specializing in enterprise-grade supply chain systems and
real-time inventory platforms, you are responsible for designing and implementing a
production-grade Warehouse Management System (WMS). The system must provide
accurate, real-time inventory tracking, role-based access, barcode/QR operations,
shipment management, and analytics dashboards while ensuring scalability, security,
and observability.

The system should help warehouse administrators, managers, staff, and suppliers
manage inventory, track shipments, receive alerts, and generate actionable analytics
through a modern, responsive interface.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Objective
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Design and implement a complete, production-ready WMS that:

• Ingests and manages inventory records (products, SKUs, barcodes, quantities)
• Supports real-time inventory synchronization via WebSockets
• Provides role-based dashboards for admins, managers, staff, and suppliers
• Maintains shipment lifecycle tracking with timeline and alerts
• Generates and scans barcodes and QR codes for products and shipments
• Streams live updates to connected clients without server overload
• Supports scalable deployment for high-concurrency warehouse environments
• Includes monitoring, observability, and evaluation pipelines


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Critical Output Requirement
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Generate complete, working, production-ready code organized into modular files.
Each file must serve a single responsibility. Provide every file needed to run the
system end to end. Do not provide architecture descriptions alone — actual
implementable code is required for every component described.

Structure all code output as follows:

project-root/
├── frontend/
│   ├── components/
│   │   ├── dashboard/
│   │   ├── inventory/
│   │   ├── shipments/
│   │   ├── barcode/
│   │   └── notifications/
│   ├── pages/
│   ├── hooks/
│   ├── store/
│   └── utils/
├── backend/
│   ├── api/
│   │   ├── routes/
│   │   └── middleware/
│   ├── inventory/
│   ├── shipments/
│   ├── barcode/
│   ├── notifications/
│   ├── auth/
│   └── monitoring/
├── database/
│   └── schema/
├── deployment/
│   ├── docker/
│   └── kubernetes/
├── evaluation/
└── docs/


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Test Case and Sample Data
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Use the following as a concrete test scenario throughout all implementation examples:

• Sample product: "Industrial Safety Gloves — SKU: ISG-4821-L" (Supplier: SafeGear
  Inc., Category: PPE, Stock: 142 units, Reorder threshold: 50 units)
• Sample action: Warehouse staff scans barcode ISG-4821-L during inbound shipment
  SHP-20241103-007, triggering a stock update, shipment status change to "Received",
  and a real-time dashboard update for all connected admin and manager clients
• Expected behavior: System validates the scanned barcode, updates inventory count
  atomically in the database, emits a WebSocket event to all subscribed dashboard
  clients, logs the warehouse activity, and triggers a low-stock check — if stock
  falls below 50 units after update, fires an in-app and email notification to the
  warehouse manager

All code examples, API response samples, and pipeline demonstrations must reference
this test case.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Core System Requirements
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Inventory Management Pipeline
─────────────────────────────────

The system must support:

• Product creation with fields: name, SKU, barcode, category, quantity, unit,
  reorder threshold, expiration date, product images, and supplier association
• Metadata extraction per product: creation timestamp, last updated by, warehouse
  zone, and stock history log
• Bulk import and export via CSV/Excel with validation and error reporting
• Automatic SKU and barcode generation for new products
• Duplicate detection using SKU and barcode uniqueness constraints
• Quantity update locking to prevent concurrent write conflicts (optimistic locking
  or DB-level transactions)
• Inventory history log: record every quantity change with timestamp, actor, and
  reason
• Automated low-stock alert trigger when quantity falls below reorder threshold
• Asynchronous processing of bulk imports using job queues (Bull or BullMQ)
• Graceful failure logging with retry mechanism (max 3 retries)

Provide working code for: Product model/schema, SKU and barcode generator,
Bulk import/export handler, Inventory update service with locking, Low-stock alert
trigger, Activity logger.


2. Barcode and QR Code Strategy
──────────────────────────────────

Use the following barcode/QR configuration and justify each choice in code comments:

• Barcode format: Code-128
  - Justification: Widely supported, high-density encoding, industry standard for
    warehouse SKU tracking
• QR Code: qrcode library (Node.js)
  - Justification: Mobile-device compatible, supports deep-link URLs for quick
    product lookups
• Barcode scanning: QuaggaJS or ZXing (browser-based)
  - Justification: No hardware dependency, supports webcam and mobile camera input

Provide working code for: Barcode generation module, QR code generation module,
Browser-based barcode scanner component, Shipment barcode tracker.


3. Real-Time Synchronization Pipeline
──────────────────────────────────────

Implement a WebSocket-based real-time architecture:

• WebSocket server using Socket.io (primary real-time engine)
  - Justification: Mature library, supports rooms/namespaces, automatic fallback
    to long-polling, strong Node.js integration
• Event-driven inventory sync: emit stock update events on every quantity change
• Live shipment status broadcasting to subscribed clients
• Dynamic dashboard refresh without full-page reload
• Room-based subscriptions: admin room, manager room, per-warehouse rooms
• Debounced updates: prevent server overload by batching rapid sequential events
  with a 300ms debounce window
• Concurrent user support: tested for up to 500 simultaneous connections

Provide working code for: Socket.io server setup, Event emitter service, Client-side
Socket.io hook (React), Room subscription manager, Debounce utility for event batching,
Full real-time orchestrator.


4. Shipment Management
────────────────────────

The system must:

• Support full shipment lifecycle: Draft → Scheduled → In Transit → Arrived →
  Received → Completed (or Cancelled/Delayed)
• Generate shipment records with: shipment ID, origin, destination, carrier,
  expected delivery date, associated products and quantities, assigned staff member
• Track real-time status with timestamped timeline entries per status change
• Barcode-scan-triggered status transitions during receiving
• Automated delayed shipment alerts if expected delivery date is exceeded
• Shipment analytics: on-time rate, average transit time, shipments by carrier
• Stream shipment status updates via WebSocket to all subscribed dashboard clients

Prompt Engineering Strategy for Notifications:

  System: You are a warehouse notification assistant. Generate concise,
  actionable notification messages ONLY from the provided shipment or
  inventory event data. For every alert, include the affected entity
  (product SKU or shipment ID), current status, and recommended action.
  If the event data is insufficient to generate a meaningful alert,
  respond with: "Insufficient event data to generate notification."

  Event Data: {triggered_event}
  Recipient Role: {recipient_role}
  Warehouse Context: {warehouse_metadata}

Context Window Management:
• Max notification message length: 160 characters (SMS-safe)
• For in-app notifications: max 300 characters
• For email digests: aggregate up to 10 events per email

Provide working code for: Shipment model/schema, Status transition service,
Shipment timeline builder, Delayed shipment alert job, Shipment analytics aggregator,
WebSocket broadcast handler.


5. Hallucination-Free Data Integrity and Safety
──────────────────────────────────────────────────

• Inventory write validation: Only commit stock changes when quantity delta passes
  business rule checks (no negative stock, no zero-SKU writes)
• Conflict detection: Reject concurrent writes to the same SKU within a 500ms
  window using Redis-based distributed locks
• Barcode collision prevention: Verify generated barcodes against existing index
  before assignment
• Refusal behavior: If a bulk import row fails validation, skip and log the row
  without halting the entire batch
• Input sanitization: Strip script tags, SQL injection patterns, and malformed SKU
  characters from all inventory form inputs
• File upload validation: Accept only CSV/Excel for bulk imports (max 10MB),
  PDF/image for product attachments (max 5MB each)
• Activity audit trail: Every data mutation is logged with actor identity, timestamp,
  before-value, and after-value; logs are immutable

Provide working code for: Inventory validator, Distributed lock service (Redis),
Barcode collision checker, Bulk import row error handler, Input sanitizer, Audit
logger.


6. Frontend Requirements
──────────────────────────

Technology: Next.js (App Router), Tailwind CSS, Framer Motion, Redux Toolkit

Frontend must include:

• Admin Dashboard: inventory summary cards, live shipment monitor, low-stock
  alerts panel, supplier activity feed, user management table
• Staff Dashboard: assigned task list, inventory scanner interface, shipment
  handling status, product update form
• Inventory Management UI: product table with search/filter, add/edit/delete
  modals, barcode/QR display panel, bulk import/export controls
• Shipment Tracking UI: shipment list with status badges, detail timeline view,
  carrier and delivery info panel
• Barcode Scanner Component: live webcam feed with overlay, scan result
  display, manual entry fallback
• Analytics Dashboard: interactive charts (inventory trends, shipment on-time
  rate, warehouse throughput), downloadable reports
• Notification Center: real-time in-app notification bell, notification list
  with read/unread states
• Responsive design (mobile, tablet, desktop)
• Dark mode toggle
• Accessibility support (ARIA labels, semantic HTML, keyboard navigation)
• Animated transitions between states using Framer Motion

Provide working code for: Admin dashboard component, Staff dashboard component,
Inventory table and modal components, Shipment timeline component, Barcode scanner
component, Analytics chart components (Recharts), Notification bell and list
components, Framer Motion animation config, Dark mode provider.


7. Backend Requirements
─────────────────────────

Technology: Node.js, Express.js, Socket.io, JWT authentication, Bull for job queues

Backend responsibilities:

• Inventory CRUD and bulk operations
• Shipment lifecycle management
• Barcode/QR generation and validation
• JWT authentication and RBAC middleware
• Real-time event broadcasting via Socket.io
• Background job processing (bulk imports, notification dispatch, report generation)
• Email notification dispatch via Nodemailer or SendGrid
• Rate limiting: max 20 requests per minute per user

Provide working code for: All API route handlers, Authentication middleware,
RBAC middleware, Rate limiting middleware, Socket.io event handlers, Bull job
definitions, Email notification service, Background task handler.


8. Database and Storage
─────────────────────────

Layer            Technology      Justification
─────────────────────────────────────────────────────────────────────────────
Primary DB       PostgreSQL       Relational integrity for inventory, users,
                                  shipments; ACID transactions for stock updates
Cache / Locks    Redis            Low-latency session management, distributed
                                  locking for concurrent writes, job queue backend
File Storage     Local / S3       Product images, bulk import CSVs, generated
                                  barcode/QR image files
─────────────────────────────────────────────────────────────────────────────

Provide: PostgreSQL schema for users, products, inventory_logs, shipments,
shipment_items, notifications, and warehouse_activity. Redis key structure for
sessions, distributed locks, and rate limiting. S3/local storage directory layout
for uploads.


9. API Design
───────────────

Design and implement the following endpoints:

POST   /api/auth/register                → User registration
POST   /api/auth/login                   → JWT token generation
POST   /api/auth/refresh                 → Token refresh

POST   /api/inventory                    → Create new product
GET    /api/inventory                    → List products (filter/search/paginate)
GET    /api/inventory/:id                → Get single product with stock history
PUT    /api/inventory/:id                → Update product details or quantity
DELETE /api/inventory/:id                → Remove product from system
POST   /api/inventory/bulk-import        → Bulk CSV import (async job)
GET    /api/inventory/export             → Export inventory as CSV

POST   /api/shipments                    → Create new shipment
GET    /api/shipments                    → List shipments (filter by status/date)
GET    /api/shipments/:id                → Get shipment with timeline
PUT    /api/shipments/:id/status         → Transition shipment status
DELETE /api/shipments/:id                → Cancel shipment

POST   /api/barcode/generate             → Generate barcode or QR for SKU
POST   /api/barcode/scan                 → Validate scanned barcode, return product
GET    /api/barcode/:sku/image           → Return barcode image file

GET    /api/notifications                → Get notifications for current user
PUT    /api/notifications/:id/read       → Mark notification as read
DELETE /api/notifications/clear          → Clear all read notifications

GET    /api/analytics/inventory          → Inventory analytics summary
GET    /api/analytics/shipments          → Shipment performance metrics
GET    /api/analytics/warehouse          → Warehouse throughput and activity

GET    /api/users                        → List users (admin only)
PUT    /api/users/:id/role               → Update user role (admin only)
DELETE /api/users/:id                    → Remove user (admin only)

All responses must follow this structure:

{
  "status": "success" | "error",
  "data": {},
  "message": "string",
  "timestamp": "ISO8601",
  "request_id": "uuid"
}


10. Authentication and Security
─────────────────────────────────

Implement:

• JWT authentication with refresh token rotation (access token: 15min TTL,
  refresh token: 7-day TTL stored in HttpOnly cookie)
• Rate limiting: 20 requests per minute per user (Redis-backed sliding window)
• Secure file upload validation (CSV max 10MB, images max 5MB, PDF only)
• Environment variable management via .env with .env.example provided
• RBAC with four roles:
  - admin: full system access including user management
  - manager: inventory + shipment management + analytics
  - staff: assigned task execution + barcode scanning + product updates
  - supplier: read-only access to their own product and shipment data

Protect against: SQL injection via parameterized queries, XSS via output encoding,
malicious file uploads via MIME-type and magic-byte validation, unauthorized API
access via JWT middleware on all protected routes, brute-force login via login rate
limiting (5 attempts per 15 minutes per IP).


11. Monitoring and Observability
──────────────────────────────────

Implement production observability using Prometheus, Grafana, and OpenTelemetry.
Track the following metrics:

Metric                          Target Threshold
────────────────────────────────────────────────────────────────
p95 inventory update latency    < 200ms
p95 WebSocket event delivery    < 100ms
p95 end-to-end API latency      < 500ms
Failed inventory write rate     Alert if > 1%
Concurrent WebSocket clients    Alert if > 400
Failed bulk import row rate     Alert if > 5%
Low-stock alert delivery rate   > 99%
────────────────────────────────────────────────────────────────

Provide working code for: Prometheus metrics setup, OpenTelemetry trace
instrumentation, Latency tracking middleware, WebSocket connection monitor,
Bulk import failure rate logger.


12. Evaluation Framework
─────────────────────────

Design evaluation pipelines measuring:

• Inventory update correctness: Verify final stock count matches expected delta
  across 50 concurrent write test scenarios
• Real-time sync accuracy: Measure lag between database write and WebSocket
  client delivery across 100 test events
• Barcode scan reliability: Scan success rate across 200 test barcodes under
  varied lighting and orientation conditions (simulated via image set)
• Notification delivery rate: Percentage of triggered events that result in
  delivered in-app and email notifications within SLA
• API latency: p95 response time across 200 test requests per endpoint
• Concurrent user stability: System behavior under 500 simultaneous WebSocket
  connections

Include: Offline test dataset with 50 inventory scenarios and expected outcomes.
Automated benchmarking script for API and WebSocket latency. Load test script
using k6 or Artillery for concurrent user simulation.

Provide working code for: Evaluation runner script, Metrics calculator,
Benchmark dataset loader, Load test configuration, Results reporter.


13. Deployment
────────────────

Provide deployment architecture using:

• Docker with separate containers for: frontend (Next.js), backend (Node.js/Express),
  PostgreSQL, Redis, Bull worker, Nginx reverse proxy
• Docker Compose for local development with hot-reload support
• Kubernetes manifests for production deployment with horizontal pod autoscaling
• CI/CD pipeline using GitHub Actions (lint → test → build → push image → deploy)
• Environment variable management with .env.example

Include: Staging and production environment separation. Rollback mechanism via
versioned Docker images. Horizontal pod autoscaling for backend (min 2, max 10
replicas based on CPU > 60%). Nginx configuration for WebSocket upgrade headers.

Provide: Complete docker-compose.yml, Kubernetes deployment manifests,
Kubernetes HPA configuration, GitHub Actions CI/CD workflow file,
Nginx config with WebSocket support, .env.example with all required variables.


14. Documentation
──────────────────

Provide:

• Complete folder structure with file descriptions
• Step-by-step setup instructions (local and production)
• Environment variable configuration guide
• API documentation with request/response examples for every endpoint
• Architecture diagram described textually with data flow (from barcode scan
  through to dashboard update)
• Troubleshooting section covering top 10 common issues (e.g. WebSocket
  disconnect on Nginx, Redis lock timeout, bulk import row failures, JWT
  expiry handling, barcode scanner permission errors)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Final Output Checklist
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Your response must include ALL of the following — do not skip any item:

• Complete modular folder structure
• Product ingestion and inventory CRUD pipeline code
• SKU and barcode/QR generation module code
• Browser-based barcode scanner component code
• Real-time WebSocket synchronization pipeline code (Socket.io server + client)
• Shipment lifecycle management module code
• Shipment timeline builder and status transition service code
• Delayed shipment alert job code
• Inventory distributed locking module code (Redis)
• Low-stock alert trigger code
• Bulk import/export handler code (with row-level error handling)
• Activity audit logger code
• Input sanitizer and file upload validator code
• Admin dashboard component code (Next.js)
• Staff dashboard component code (Next.js)
• Inventory management UI code (table, modals, barcode panel)
• Shipment tracking UI code (list, timeline, status badges)
• Analytics dashboard code (charts via Recharts)
• Notification center component code
• Framer Motion animation configuration
• Dark mode provider code
• FastAPI/Express backend with all route handler code
• PostgreSQL schema (users, products, inventory_logs, shipments, notifications)
• Redis key structure and session management code
• JWT authentication middleware code
• RBAC middleware code
• Rate limiting middleware code
• Prometheus and OpenTelemetry instrumentation code
• Bull job definitions and worker code
• Email notification service code
• Evaluation runner script
• Load test configuration (k6 or Artillery)
• docker-compose.yml
• Kubernetes deployment manifests
• Kubernetes HPA configuration
• GitHub Actions CI/CD workflow
• Nginx configuration with WebSocket support
• .env.example
• Complete README with setup and deployment guide
```
