<!--
  Sync Impact Report
  ===================
  Version change: 2.0.0 -> 3.0.0 (Phase V constitution)

  Modified principles:
  - P3: "Containerized Kubernetes Architecture" -> "Production Cloud Kubernetes Architecture"
  - P4: "Technology Stack Compliance" -> expanded with Phase V stack (Kafka, Dapr, CI/CD)
  - P12: "Kubernetes Deployment Standards" -> expanded for production cloud + HPA + Ingress TLS
  - P13: "AIOps Integration" -> retained, extended with production monitoring

  Added sections:
  - P15: Event-Driven Architecture (Kafka + Dapr Pub/Sub)
  - P16: Dapr Distributed Runtime
  - P17: CI/CD Pipeline Standards (GitHub Actions)
  - P18: Production Monitoring & Observability
  - P19: Advanced Task Features

  Removed sections: None (P1-P14 retained)

  Templates requiring updates:
  - .specify/templates/plan-template.md ✅ no changes needed (generic)
  - .specify/templates/spec-template.md ✅ no changes needed (generic)
  - .specify/templates/tasks-template.md ✅ no changes needed (generic)

  Deferred items: None
-->

# Project Constitution

**Project Name:** AI-Powered Todo Chatbot (Phase V: Advanced Cloud Deployment with Event-Driven Architecture)

**Version:** 3.0.0

**Ratification Date:** 2026-01-22

**Last Amended Date:** 2026-01-31

---

## Preamble

This constitution establishes the foundational principles, technical standards, and governance
rules for the AI-Powered Todo Chatbot project. Phase V extends the application with production
cloud Kubernetes deployment (DOKS/GKE/AKS), event-driven architecture via Kafka (Redpanda Cloud),
Dapr distributed runtime for service communication, CI/CD via GitHub Actions, production
monitoring, and advanced task features (recurring tasks, due dates, priorities, search, sort).
All Phase III and Phase IV features MUST remain functional. All development activities,
architectural decisions, and implementations MUST adhere to these principles.

---

## Principles

### P1: AI-Driven Development (AIDD)

**Statement:** 100% of code MUST be generated through AI-driven development using Claude Code.

**Rules:**
- No manual code writing is permitted; all code is generated via AI specifications
- Specifications MUST be refined iteratively until correct output is achieved
- Every code change MUST be traceable to a specification or prompt
- Human developers act as architects and reviewers, not code writers

**Rationale:** AIDD ensures consistency, reduces human error, and demonstrates the viability
of AI-assisted software development at scale.

---

### P2: Spec-Driven Development

**Statement:** All features MUST be developed using Spec-Kit Plus methodology with formal
specifications preceding implementation.

**Rules:**
- Every feature MUST have a spec.md before implementation begins
- Plans (plan.md) MUST document architectural decisions
- Tasks (tasks.md) MUST be testable and traceable to specifications
- Prompt History Records (PHR) MUST capture all significant interactions
- Architectural Decision Records (ADR) MUST document significant technical choices

**Rationale:** Spec-driven development creates auditable, reproducible, and maintainable
software while enabling AI agents to operate effectively within defined boundaries.

---

### P3: Production Cloud Kubernetes Architecture

**Statement:** The application MUST be containerized with Docker and deployed to a production
cloud Kubernetes cluster (DOKS, GKE, or AKS).

**Rules:**
- Single repository with `/frontend`, `/backend`, `/k8s` (or Helm chart), and `/dapr` folders
- Core deployments: Frontend (Next.js), Backend (FastAPI + MCP server),
  Database (Neon external connection), Kafka consumers (reminder, audit, notification services)
- Services MUST provide internal communication between pods via Dapr service invocation
- ConfigMaps MUST store non-sensitive configuration
- Secrets MUST store sensitive data (API keys, JWT secret, DB credentials, Kafka credentials)
- Ingress MUST provide external access with TLS/SSL termination
- Helm chart MUST package all Kubernetes resources including Dapr components
- Horizontal Pod Autoscaler MUST be configured for all stateless services
- Rolling updates with zero downtime MUST be the default deployment strategy
- All Phase III and Phase IV features MUST work identically in cloud deployment

**Rationale:** Production cloud deployment demonstrates real-world readiness with high
availability, auto-scaling, and zero downtime capabilities.

---

### P4: Technology Stack Compliance

**Statement:** All implementations MUST use the prescribed technology stack without deviation.

**Frontend Stack:**
- Next.js 16+ with App Router (required)
- Tailwind CSS + custom CSS for styling
- TypeScript for type safety

**Backend Stack:**
- Python FastAPI for API services
- SQLModel as ORM layer
- Neon Serverless PostgreSQL for persistence
- OpenAI Agents SDK for AI capabilities
- Official MCP SDK (Python) for tool integration

**Deployment Stack (Phase IV, retained):**
- Docker with multi-stage builds for containerization
- Docker AI (Gordon) via Docker Desktop 4.53+
- Helm Charts for package management
- AIOps: kubectl-ai, kagent

**Phase V Cloud & Event Stack:**
- Cloud Platform: DigitalOcean Kubernetes (DOKS) OR Google Cloud (GKE) OR Azure (AKS)
- Event Streaming: Kafka via Redpanda Cloud (serverless free tier)
- Distributed Runtime: Dapr (Pub/Sub, State, Bindings, Secrets, Service Invocation)
- CI/CD: GitHub Actions for automated deployments
- Monitoring: Prometheus + Grafana (recommended)
- Logging: EFK Stack or cloud provider logging (recommended)

**Rules:**
- No alternative frameworks or libraries without ADR approval
- Version constraints MUST be respected
- All dependencies MUST be explicitly declared
- Free/trial cloud tiers MUST be used (DigitalOcean $200, GCP $300, Azure $200 credits)

**Rationale:** Stack consistency ensures predictable behavior, simplified debugging, and
cohesive AI code generation.

---

### P5: Authentication & Security

**Statement:** All user interactions MUST be authenticated and all API endpoints MUST be
secured with JWT tokens.

**Authentication Requirements:**
- Better Auth integration with email verification
- Signup requires: email, name, password
- Email verification system (accept fake emails, verify format)
- Verified email stored in Neon DB for reminder notifications
- JWT token-based API security for all protected endpoints

**Security Rules:**
- MUST prevent SQL injection via parameterized queries (SQLModel)
- MUST validate and sanitize all user inputs
- MUST use HTTPS in production (TLS via Ingress)
- Secrets MUST be stored in environment variables, never in code
- JWT tokens MUST have appropriate expiration times
- Kubernetes Secrets MUST be used for sensitive data in cluster
- Dapr Secrets Management MUST be used for API keys and credentials
- Network policies MUST restrict inter-pod communication to required paths
- RBAC MUST be configured for cluster access control

**Rationale:** Security is non-negotiable; authentication protects user data and enables
personalized features like reminders.

---

### P6: AI Chat Interface Standards

**Statement:** The AI assistant MUST be implemented as a separate page with persistent
conversation history.

**Interface Requirements:**
- Dedicated AI Assistant page (not embedded in dashboard)
- OpenAI ChatKit integration with Agents SDK
- Left side: Active chat interface
- Right sidebar: Chat history from Neon DB
- Conversation state persisted across sessions in database

**Technical Requirements:**
- Stateless backend architecture
- Database-persisted conversation state
- Real-time message updates
- Graceful error handling for AI failures

**Rationale:** Separation of chat from dashboard improves UX focus; persistence enables
continuity and context retention.

---

### P7: MCP Server Implementation

**Statement:** Exactly 5 MCP tools MUST be implemented using the official MCP SDK for task
operations.

**Required Tools:**
1. `add_task` - Create task with title, description, reminder, date, day
2. `delete_task` - Remove task by ID
3. `update_task` - Modify existing task details
4. `mark_as_completed_task` - Toggle task completion status
5. `view_task` - Retrieve tasks with filtering support

**Rules:**
- All tools MUST be implemented using official MCP SDK (Python)
- Tools MUST validate inputs before database operations
- Tools MUST return structured responses for AI consumption
- Error handling MUST provide actionable feedback
- Task mutations MUST publish events to Kafka via Dapr Pub/Sub

**Rationale:** MCP tools enable the AI agent to perform task management operations on behalf
of users, creating a natural language interface.

---

### P8: Visual Design Excellence

**Statement:** The application MUST deliver a visually stunning dark theme with purple
accents and exceptional animations.

**Design Requirements:**
- Black and purple color theme throughout
- Landing page: Left-aligned header text, right side high-quality image
- High-quality thematic images throughout the application
- Exceptional CSS animations on landing page
- Dashboard: Purple accents complementing black theme
- Smooth, complementary animations on dashboard

**Rules:**
- Animations MUST not impact performance (60fps target)
- Images MUST be optimized for web delivery
- Design MUST maintain visual hierarchy and readability
- Animations MUST enhance UX, not distract

**Rationale:** Visual excellence differentiates the product and demonstrates attention to
detail in AI-generated applications.

---

### P9: Task Reminder System

**Statement:** The application MUST support email-based task reminders with user-defined
scheduling, driven by event-based architecture.

**Requirements:**
- Task model MUST include reminder fields: date, day, time
- UI MUST provide intuitive reminder input controls
- Reminder service MUST consume events from Kafka `reminders` topic
- Reminders MUST be sent via email to users verified address
- Dapr cron bindings MUST trigger scheduled reminder checks

**Rules:**
- Reminder metadata MUST be stored in the task model
- Reminder service MUST be a separate Kafka consumer
- Failed reminder attempts MUST be logged, retried, and dead-lettered
- Users MUST be able to modify or cancel reminders

**Rationale:** Reminders increase task completion rates and provide value beyond basic
todo functionality. Event-driven reminders decouple scheduling from the main application.

---

### P10: Quality & Accessibility

**Statement:** All code MUST meet quality standards and accessibility requirements.

**Quality Standards:**
- Clean, maintainable code structure
- Comprehensive error handling with user-friendly messages
- Type safety via TypeScript (frontend) and type hints (backend)
- No dead code or unused dependencies

**Accessibility Requirements:**
- WCAG 2.1 AA compliance required
- Keyboard navigation support
- Screen reader compatibility
- Sufficient color contrast ratios
- Focus indicators on interactive elements

**Responsive Design:**
- Mobile-first approach
- Breakpoints: mobile, tablet, desktop
- Touch-friendly targets (minimum 44x44px)

**Rationale:** Quality and accessibility ensure the application serves all users effectively
and maintains long-term maintainability.

---

### P11: Container Image Standards

**Statement:** All Docker images MUST use multi-stage builds and meet size and security
constraints.

**Rules:**
- Frontend image MUST be < 500MB
- Backend image MUST be < 300MB
- Multi-stage builds MUST separate build dependencies from runtime
- Images MUST NOT contain secrets, credentials, or `.env` files
- Images MUST use non-root users for runtime
- `.dockerignore` MUST exclude unnecessary files (node_modules, .git, etc.)
- Each image MUST include a health check endpoint
- Images MUST be pushed to a container registry (GHCR, Docker Hub, or cloud registry)

**Rationale:** Optimized images reduce deployment time, minimize attack surface, and
improve resource utilization in the cluster.

---

### P12: Kubernetes Deployment Standards

**Statement:** All Kubernetes resources MUST follow production-ready cloud deployment patterns.

**Rules:**
- Every deployment MUST define liveness and readiness probes
- Every deployment MUST specify CPU and memory resource requests and limits
- Rolling update strategy MUST be used (maxSurge: 1, maxUnavailable: 0)
  to enable zero downtime deployments
- Horizontal Pod Autoscaler MUST be configured for stateless services
  (min 2 replicas, max based on load)
- Graceful shutdown MUST be handled (preStop hooks, SIGTERM handling)
- All pods MUST log to stdout/stderr for cluster log aggregation
- ConfigMaps MUST be used for non-sensitive environment configuration
- Secrets MUST be used for API keys, JWT secrets, and database credentials
- Ingress MUST be configured with TLS/SSL certificates
- Network policies MUST restrict traffic to required paths
- Dapr sidecar annotations MUST be applied to all application pods

**Cloud Infrastructure Requirements:**
- Production Kubernetes cluster (DOKS, GKE, or AKS)
- Free/trial tier cloud credits MUST be used
- All kubectl commands MUST be documented for manual fallback

**Rationale:** Production-ready patterns ensure reliability, observability, and the ability
to perform zero downtime deployments at scale.

---

### P13: AIOps Integration

**Statement:** AI-powered operations tools MUST be integrated for intelligent cluster
management.

**Required Tools:**
- Docker AI (Gordon) for intelligent Docker image building and troubleshooting
- kubectl-ai for natural language Kubernetes operations
- kagent for cluster analysis and optimization recommendations

**Rules:**
- AIOps tools MUST be used where applicable for troubleshooting
- Manual kubectl commands MUST be documented as fallback
- AI-generated recommendations MUST be reviewed before applying
- All AIOps interactions SHOULD be recorded in PHRs when significant

**Rationale:** AIOps tools reduce operational complexity and demonstrate AI-driven
infrastructure management alongside AI-driven development.

---

### P14: Helm Chart Management

**Statement:** A Helm chart MUST package all Kubernetes resources for the application.

**Rules:**
- Chart MUST include templates for all deployments, services, ConfigMaps, Secrets,
  Ingress, Dapr components, and HPA resources
- Values file MUST externalize all configurable parameters
- Chart MUST support environment-specific value overrides (dev, staging, prod)
- Chart MUST include NOTES.txt with post-install instructions
- Chart version MUST follow semantic versioning

**Rationale:** Helm charts provide reproducible, version-controlled deployment packaging
that simplifies installation and upgrades.

---

### P15: Event-Driven Architecture

**Statement:** The application MUST implement event-driven architecture using Kafka via
Redpanda Cloud with Dapr Pub/Sub as the abstraction layer.

**Kafka Topics:**
- `task-events` — All task CRUD operations (created, updated, deleted, completed)
- `reminders` — Reminder scheduling and trigger events
- `task-updates` — Real-time task state change notifications

**Producers:**
- Backend service MUST publish events for all task mutations
- Events MUST include: event type, task ID, user ID, timestamp, payload

**Consumers:**
- Reminder service: Processes scheduled reminders from `reminders` topic
- Audit service: Logs all task operations from `task-events` topic
- Notification service: Sends browser/email notifications from `task-updates` topic

**Rules:**
- All event publishing MUST go through Dapr Pub/Sub (not direct Kafka client)
- Events MUST follow a consistent schema with versioning
- Consumer failures MUST be handled with retries and dead-letter queues
- Event ordering MUST be maintained per user (partition by user ID)
- Redpanda Cloud serverless free tier MUST be used

**Rationale:** Event-driven architecture decouples services, enables independent scaling,
and provides an audit trail of all task operations.

---

### P16: Dapr Distributed Runtime

**Statement:** Dapr MUST be used as the distributed application runtime for all inter-service
communication and infrastructure abstraction.

**Required Dapr Building Blocks:**
- **Pub/Sub:** Kafka integration for publishing and subscribing to task events
- **State Management:** Conversation state storage as alternative/supplement to direct DB
- **Service Invocation:** Frontend-to-Backend communication with automatic retries and mTLS
- **Bindings:** Cron triggers for scheduled reminder checks
- **Secrets Management:** Secure storage and retrieval of API keys and credentials

**Rules:**
- All services MUST run with Dapr sidecars in Kubernetes
- Dapr component definitions MUST be stored in `/dapr/components/`
- Service-to-service calls MUST use Dapr service invocation (not direct HTTP)
- Secret references MUST use Dapr secrets API
- Dapr dashboard MUST be accessible for debugging

**Rationale:** Dapr provides portable, cloud-agnostic abstractions for distributed systems
patterns, reducing boilerplate and enabling provider-agnostic infrastructure.

---

### P17: CI/CD Pipeline Standards

**Statement:** GitHub Actions MUST automate the build, test, and deployment pipeline for
all services.

**Pipeline Stages:**
1. **Lint & Test:** Run linters and tests on every push/PR
2. **Build:** Build Docker images with multi-stage builds
3. **Push:** Push images to container registry with semantic version tags
4. **Deploy:** Deploy to cloud Kubernetes cluster via Helm upgrade
5. **Verify:** Run smoke tests against deployed services

**Rules:**
- Pipeline MUST trigger on push to main and on pull requests
- Docker images MUST be tagged with git SHA and semantic version
- Secrets (cloud credentials, registry tokens) MUST use GitHub Actions secrets
- Deployment MUST use rolling updates for zero downtime
- Pipeline MUST fail fast on lint/test failures
- Manual approval gate SHOULD be configured for production deployments

**Rationale:** Automated CI/CD ensures consistent, repeatable deployments and reduces
human error in the release process.

---

### P18: Production Monitoring & Observability

**Statement:** Production deployments MUST include monitoring, logging, and alerting
capabilities.

**Monitoring (Recommended):**
- Prometheus for metrics collection from all services
- Grafana dashboards for visualization
- Kubernetes metrics (CPU, memory, pod health)
- Application metrics (request latency, error rates, task operations)

**Logging:**
- All services MUST log to stdout/stderr in structured JSON format
- Cloud provider logging or EFK stack for log aggregation
- Log levels MUST be configurable via environment variables

**Alerting:**
- Pod crash/restart alerts
- High error rate alerts (>5% 5xx responses)
- Resource utilization alerts (>80% CPU/memory)

**Rules:**
- Health check endpoints MUST be exposed by all services
- Metrics endpoints MUST be exposed for Prometheus scraping
- Dapr observability features MUST be enabled (tracing, metrics)

**Rationale:** Observability is essential for operating production systems reliably and
diagnosing issues quickly.

---

### P19: Advanced Task Features

**Statement:** The application MUST implement advanced task management features beyond
basic CRUD operations.

**Required Features:**
- **Recurring Tasks:** Auto-reschedule repeating tasks (daily, weekly, monthly, custom)
- **Due Dates & Time Reminders:** Date/time pickers in UI, browser notifications
- **Priorities:** High, Medium, Low levels with visual indicators
- **Tags/Categories:** User-defined labels (e.g., work, home, urgent)
- **Search & Filter:** Keyword search across task name and description;
  filter by status, priority, date range, tags
- **Sort Tasks:** By due date, priority, creation date, alphabetically

**Rules:**
- All advanced features MUST be accessible via both UI and AI chat interface
- MCP tools MUST support advanced fields (priority, tags, due date, recurrence)
- Task events MUST include advanced field changes
- Filters and sorts MUST be combinable (e.g., high priority + due this week)

**Rationale:** Advanced features transform the application from a basic todo list into a
production-grade task management system.

---

## Governance

### Amendment Procedure

1. Propose amendment via ADR with rationale
2. Review impact on existing specifications and implementations
3. Update constitution with version increment
4. Propagate changes to dependent templates
5. Document in Sync Impact Report

### Versioning Policy

Constitution follows semantic versioning:
- **MAJOR (X.0.0):** Backward-incompatible principle changes or removals
- **MINOR (X.Y.0):** New principles or significant expansions
- **PATCH (X.Y.Z):** Clarifications, wording fixes, non-semantic updates

### Compliance Review

- All specifications MUST reference relevant principles
- Code reviews MUST verify principle adherence
- Deviations require explicit ADR with justification
- Quarterly reviews assess principle effectiveness

---

## Constraints

### Phase V Scope

The following are IN SCOPE for Phase V:
- Production cloud Kubernetes deployment (DOKS/GKE/AKS)
- Event-driven architecture with Kafka via Redpanda Cloud
- Dapr distributed runtime integration (all 5 building blocks)
- CI/CD pipeline with GitHub Actions
- Production monitoring and observability
- Advanced task features (recurring, due dates, priorities, tags, search, sort)
- Zero downtime deployments with rolling updates and HPA
- Ingress with TLS/SSL
- Security hardening (network policies, RBAC, secrets management)

### Explicit Exclusions

The following are OUT OF SCOPE:
- Multi-cluster or federation
- Service mesh beyond Dapr (no Istio, Linkerd)
- Custom Kafka cluster management (use Redpanda Cloud managed service)
- Mobile native applications
- Payment or billing features
- GraphQL API (REST + Dapr service invocation only)

### Development Constraints

- All code generated via Claude Code (no manual coding)
- Specifications MUST be refined until correct output
- No external API integrations beyond prescribed stack
- Free/trial cloud tiers MUST be used (no paid services without approval)
- All Phase III and Phase IV features MUST pass verification before Phase V work begins
- Kafka MUST use Redpanda Cloud serverless free tier
- All inter-service communication MUST use Dapr

---

## Appendix A: File Structure

```
/
├── frontend/           # Next.js application
│   └── Dockerfile      # Multi-stage build
├── backend/            # FastAPI application
│   └── Dockerfile      # Multi-stage build
├── dapr/               # Dapr configuration
│   └── components/     # Pub/Sub, state, bindings, secrets
├── k8s/                # Raw Kubernetes manifests (optional)
├── helm/               # Helm chart
│   └── obsidianlist/
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── values-dev.yaml
│       ├── values-prod.yaml
│       └── templates/
├── .github/
│   └── workflows/      # GitHub Actions CI/CD pipelines
├── services/           # Kafka consumer services
│   ├── reminder/       # Reminder consumer
│   ├── audit/          # Audit log consumer
│   └── notification/   # Notification consumer
├── specs/              # Feature specifications
│   └── <feature>/
│       ├── spec.md
│       ├── plan.md
│       └── tasks.md
├── history/
│   ├── prompts/        # PHR records
│   │   ├── constitution/
│   │   ├── general/
│   │   └── <feature>/
│   └── adr/            # Architecture Decision Records
├── .specify/
│   ├── memory/
│   │   └── constitution.md
│   └── templates/
└── .env                # Environment variables (NOT in images)
```

---

## Appendix B: Success Metrics

| Metric | Target |
|--------|--------|
| AI Code Generation | 100% |
| WCAG Compliance | AA Level |
| Animation Performance | 60fps |
| API Response Time | < 500ms p95 |
| Test Coverage | > 80% |
| Frontend Image Size | < 500MB |
| Backend Image Size | < 300MB |
| Pod Startup Time | < 30s |
| Zero Downtime Deploy | Yes |
| Health Check Response | < 5s |
| CI/CD Pipeline Duration | < 10 min |
| Event Processing Latency | < 2s p95 |
| Kafka Consumer Lag | < 100 messages |
| Auto-scaling Response | < 60s |
| TLS/SSL Coverage | 100% of external endpoints |
| Uptime SLO | 99.5% |

---

*This constitution is the authoritative source for project governance. All team members,
AI agents, and automated systems MUST operate within these boundaries.*
