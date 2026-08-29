# WhatsApp Campaign Scheduler

> A production-ready messaging automation platform for creating, scheduling, and managing WhatsApp campaigns at scale.

WhatsApp Campaign Scheduler is a developer-focused platform designed to simplify campaign management, scheduled messaging, contact imports, delivery tracking, and messaging automation.

The system combines a REST API, background job processing, Redis-based queues, PostgreSQL persistence, and a web dashboard into a modular architecture that can be extended into a multi-tenant SaaS platform.

## ✨ Features

### Campaign Management

* Create and manage campaigns
* Campaign scheduling
* Draft, scheduled, running, completed, and failed states
* Campaign history
* Campaign execution logs

### Contact Management

* Import contacts using CSV
* Contact validation
* Contact groups
* Duplicate detection
* Campaign-specific recipient lists

### Message Processing

* Queue-based message processing
* Scheduled message execution
* Retry failed jobs
* Configurable rate limiting
* Delivery status tracking

### Developer API

* REST API
* API key authentication
* Campaign management endpoints
* Contact management endpoints
* Webhook support
* OpenAPI documentation

### Reliability & Security

* Input validation
* Authentication and authorization
* Rate limiting
* Structured logging
* Secure environment configuration
* Background job isolation
* Error handling and retry mechanisms

### Infrastructure

* PostgreSQL
* Redis
* BullMQ
* Docker
* Docker Compose
* GitHub Actions CI

---

## 🏗️ Architecture

```text
                    ┌──────────────────────┐
                    │      Web Dashboard   │
                    │   React + TypeScript │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       REST API       │
                    │   Node.js + Express  │
                    └──────────┬───────────┘
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
       ┌───────────┐     ┌───────────┐     ┌───────────┐
       │ PostgreSQL│     │   Redis   │     │  Webhooks │
       └───────────┘     └─────┬─────┘     └───────────┘
                               │
                               ▼
                        ┌──────────────┐
                        │    BullMQ    │
                        │ Job Queue     │
                        └──────┬───────┘
                               │
                               ▼
                        ┌──────────────┐
                        │ Message      │
                        │ Worker       │
                        └──────┬───────┘
                               │
                               ▼
                        WhatsApp Provider
```

---

## 🛠️ Tech Stack

| Layer            | Technology         |
| ---------------- | ------------------ |
| Frontend         | React + TypeScript |
| Backend          | Node.js + Express  |
| Database         | PostgreSQL         |
| Queue            | Redis + BullMQ     |
| API              | REST + OpenAPI     |
| Authentication   | JWT / API Keys     |
| Containerization | Docker             |
| CI/CD            | GitHub Actions     |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have:

* Node.js 20+
* Docker
* Docker Compose
* PostgreSQL
* Redis

### Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/whatsapp-campaign-scheduler.git

cd whatsapp-campaign-scheduler
```

### Configure environment variables

```bash
cp .env.example .env
```

Configure the required variables:

```env
DATABASE_URL=
REDIS_URL=

JWT_SECRET=

WHATSAPP_API_URL=
WHATSAPP_API_TOKEN=

PORT=3000
```

### Start infrastructure

```bash
docker compose up -d
```

### Install dependencies

```bash
npm install
```

### Run development environment

```bash
npm run dev
```

The application will be available at:

```text
http://localhost:3000
```

---

## 📡 API

The platform exposes a REST API for programmatic campaign management.

### Create Campaign

```http
POST /api/v1/campaigns
```

Example:

```json
{
  "name": "Product Launch",
  "scheduledAt": "2026-09-01T10:00:00Z",
  "recipients": [
    "+910000000000"
  ],
  "message": "Our new product is now available."
}
```

### Campaign Status

```http
GET /api/v1/campaigns/:id
```

### Schedule Campaign

```http
POST /api/v1/campaigns/:id/schedule
```

### Cancel Campaign

```http
POST /api/v1/campaigns/:id/cancel
```

Full API documentation is available through the OpenAPI/Swagger interface when running the application locally.

---

## ⚙️ Message Processing

Campaign execution is handled asynchronously.

```text
Campaign Created
       │
       ▼
Campaign Scheduled
       │
       ▼
BullMQ Job Created
       │
       ▼
Redis Queue
       │
       ▼
Worker Picks Job
       │
       ▼
WhatsApp Provider
       │
       ├── Success → Delivered
       │
       └── Failure → Retry
                         │
                         ▼
                    Retry Limit
                         │
                    ┌────┴────┐
                    ▼         ▼
                 Success    Failed
```

This architecture prevents long-running messaging operations from blocking API requests and provides a foundation for reliable campaign processing.

---

## 📊 Campaign Lifecycle

```text
DRAFT
  │
  ▼
SCHEDULED
  │
  ▼
RUNNING
  │
  ├──────────────► COMPLETED
  │
  └──────────────► FAILED
```

Individual messages maintain their own delivery state, allowing campaign-level and recipient-level monitoring.

---

## 🔐 Security

Security is treated as a first-class concern.

The project includes:

* Authentication
* API key protection
* Request validation
* Rate limiting
* Secure environment variables
* Authorization checks
* Sanitized input
* Error handling without leaking secrets
* Audit-friendly execution logs

**Never commit production credentials or API tokens to the repository.**

---

## 🧪 Testing

Run unit tests:

```bash
npm run test
```

Run integration tests:

```bash
npm run test:integration
```

Run linting:

```bash
npm run lint
```

Build the project:

```bash
npm run build
```

---

## 🐳 Docker

Run the complete local infrastructure using:

```bash
docker compose up -d
```

The Docker environment provides the services required for local development.

```text
┌─────────────────────────────┐
│        Docker Compose       │
│                             │
│  ┌────────┐  ┌──────────┐  │
│  │   API  │  │ Frontend │  │
│  └────────┘  └──────────┘  │
│                             │
│  ┌────────┐  ┌──────────┐  │
│  │ Redis  │  │PostgreSQL│  │
│  └────────┘  └──────────┘  │
│                             │
└─────────────────────────────┘
```

---

## 📸 Screenshots

Screenshots and product demonstrations will be added as the interface evolves.

Recommended screenshots:

* Dashboard
* Campaign creation
* Contact management
* Campaign analytics
* Message execution logs
* API documentation

---

## 🗺️ Roadmap

### v0.1 — Foundation

* [x] Project architecture
* [x] Database setup
* [x] Authentication foundation
* [x] Docker environment
* [x] CI pipeline

### v0.2 — Campaigns

* [ ] Campaign creation
* [ ] Campaign scheduling
* [ ] Contact import
* [ ] Message queue
* [ ] Worker processing

### v0.3 — Reliability

* [ ] Retry strategy
* [ ] Rate limiting
* [ ] Delivery tracking
* [ ] Webhooks
* [ ] Execution logs

### v0.4 — Platform

* [ ] Multi-tenant workspaces
* [ ] Team members
* [ ] Role-based access control
* [ ] Analytics
* [ ] API key management

### v1.0

* [ ] Production deployment
* [ ] Comprehensive test coverage
* [ ] Production monitoring
* [ ] Provider integrations
* [ ] Complete API documentation

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome.

Before submitting a pull request:

1. Create a feature branch.
2. Add or update tests.
3. Run linting.
4. Run the test suite.
5. Update documentation where necessary.
6. Submit a pull request with a clear description.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for detailed guidelines.

---

## 📄 License

This project is licensed under the MIT License.

See [`LICENSE`](LICENSE) for details.

---

## ⚠️ Disclaimer

This project is intended as a software engineering and automation platform.

Any WhatsApp integration must use an authorized WhatsApp Business API/provider and comply with applicable WhatsApp policies, messaging regulations, consent requirements, and anti-spam rules.

Do not use the platform for unsolicited or abusive messaging.

---

## 👨‍💻 Project Status

**Status:** Active Development

The project is being developed as an open-source reference implementation for reliable messaging automation, asynchronous job processing, and API-driven campaign management.

---

⭐ If you find the project useful, consider giving it a star and contributing to its development.
