<div align="right">
  <a href="https://www.linkedin.com/in/felix-wahl-6763791b9/">
    <img src="https://custom-icon-badges.demolab.com/badge/LinkedIn-0A66C2?logo=linkedin-white&logoColor=fff" align="absmiddle" alt="LinkedIn Badge">
  </a>
  <a href="https://www.goldbarth.dev/">
    <img src="https://custom-icon-badges.demolab.com/badge/Blog-222222?logo=globe&logoColor=fff" align="absmiddle" alt="Blog Badge">
  </a>
  <a href="mailto:hello@goldbarth.dev">
    <img src="https://custom-icon-badges.demolab.com/badge/Email-333333?logo=mail&logoColor=fff" align="absmiddle" alt="Email Badge">
  </a>
</div>

<h1 align="center">Felix Wahl</h1>

<p align="center">
  Backend-Focused .NET Engineer<br/>
  Clean Architecture · Reliable Processing · Explicit Domain Logic
</p>

<p align="center">📍 Hamburg, Germany</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/ASP.NET%20Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/EF%20Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=fff"/>
</p>

---

## About

I'm a backend-focused .NET engineer who came into software through game development.

Studying game programming taught me to care about state consistency, explicit system behavior, and performance under constraints. Those concerns turned out to matter just as much in backend systems. After two years of professional .NET work, growing from junior to mid-level with ownership of three projects, I now focus on systems that stay understandable and changeable over time. Based in Hamburg, open to hybrid roles.

---

# Featured Projects

Four backend projects that highlight complementary engineering concerns:  
**ServiceDeskLite** focuses on architecture and domain boundaries — and ships an LLM intake assistant (tool calling, SSE streaming) inside them.  
**port-tidewatch** focuses on water-level ingestion and storm-surge alerting for port infrastructure.  
**MetricGate** focuses on distributed rate limiting, multi-tenant hierarchy and low-latency enforcement.  
**Ingestor** focuses on reliability, fault handling and operational resilience.

---

## ServiceDeskLite – AI Ticket Intake on a Clean Architecture

<p>
  <img src="https://img.shields.io/github/v/release/goldbarth/ServiceDeskLite"/>
  <a href="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/ci.yml">
    <img src="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/ci.yml/badge.svg" alt="CI" />
  </a>
  <a href="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/docs.yml">
    <img src="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/docs.yml/badge.svg" alt="Docs" />
  </a>
</p>
<p>
  <img src="https://img.shields.io/badge/.NET-10-512BD4?logo=dotnet&logoColor=white"/>
  <img src="https://img.shields.io/badge/Anthropic%20API-191919?logo=anthropic&logoColor=white"/>
  <img src="https://img.shields.io/badge/Voyage%20AI-embeddings-191919"/>
  <img src="https://img.shields.io/badge/PostgreSQL%20%2B%20pgvector-4169E1?logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Blazor-512BD4?logo=blazor&logoColor=white"/>
</p>

A deliberately structured .NET 10 backend with a shipped LLM feature: users describe an issue in free text, a Claude model decides via tool calling whether to create, update, search, or route a ticket, and the response streams token-by-token to the browser (SSE). The assistant lives as an edge adapter — the model acts on the system exclusively through the same command handlers as the REST API, so validation, audit trail and outbox apply to AI-created tickets unchanged.

The goal is not feature breadth, but structural clarity, explicit boundaries, and reviewable design decisions — with the LLM integration as their latest stress test.

### AI / LLM Focus

- Tool calling against the Anthropic Messages API: create, update, search, change status and assign tickets to roster agents — every tool executes through existing command handlers, no special path into the domain
- Token streaming via SSE; text deltas and partial tool-call JSON handled in a single pass over the stream
- Tool output treated as untrusted input: parsed and guarded before touching the domain; rejected inputs return as error `tool_result`s so the model self-corrects in a bounded loop
- Stateless chat with injected date/timezone so relative deadlines ("by Friday morning") resolve correctly
- Semantic ticket search (RAG): `find_similar_tickets` retrieves duplicates by meaning, not keywords — Voyage AI embeddings stored in PostgreSQL via pgvector, computed async by a background worker off the write path
- Model-controlled retrieval: the model decides when to search and gets honest feedback when the tool is unavailable, instead of forced pre-generation retrieval
- Domain and Application compile without any Anthropic reference (ADR 0023)

### Architectural Focus

- Strict layered architecture (Domain / Application / Infrastructure / API / Web)
- Explicit domain workflow rules
- Result-based error handling (no exceptions crossing application boundaries)
- RFC 9457 ProblemDetails strategy
- Strongly-typed IDs
- Explicit UnitOfWork commit boundary
- EF Core (PostgreSQL) + InMemory provider switch
- End-to-end tests with provider matrix

**Repository:** 
https://github.com/goldbarth/ServiceDeskLite  
**Documentation:**
https://goldbarth.github.io/ServiceDeskLite  
**Blog (German):**
https://www.goldbarth.dev/projects/service-desk-lite  
**Edge-adapter decision (German):**
https://www.goldbarth.dev/decisions/ai-assistant-edge-adapter  
**Semantic ticket search / RAG decision (German):**
https://www.goldbarth.dev/decisions/semantic-ticket-search-rag

---

## port-tidewatch – Water-Level Ingestion & Storm-Surge Alerting

<p>
<img src="https://img.shields.io/github/v/release/goldbarth/port-tidewatch"/>
<a href="https://github.com/goldbarth/port-tidewatch/actions/workflows/ci.yml">
    <img src="https://github.com/goldbarth/port-tidewatch/actions/workflows/ci.yml/badge.svg" alt="CI" />
</a>
</p>
<p>
  <img src="https://img.shields.io/badge/.NET-10-512BD4?logo=dotnet&logoColor=white"/>
  <img src="https://img.shields.io/badge/RabbitMQ-FF6600?logo=rabbitmq&logoColor=white"/>
  <img src="https://img.shields.io/badge/Angular-DD0031?logo=angular&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white"/>
</p>

A focused ingestion service for port water-level telemetry, with threshold-based storm-surge alerting and a read-only monitoring dashboard. The domain is modelled on the Hamburg storm-surge warning service (WADI): a warning is raised when an expected surge peak can exceed 4.50 m above sea level (NHN) / 2.40 m above mean high water (MThw), escalating to severe at 5.50 m NHN (BSH *sehr schwere Sturmflut*).

The goal is not feature breadth, but a reliable, observable ingestion pipeline end to end — one domain, one ingestion path, no write operations from the UI.

### Technical Focus

- Three-component architecture: simulator (.NET) → ingestion service (.NET) → read-only Angular dashboard
- Message-driven ingestion via RabbitMQ with a dead-letter path for poison messages
- Staged WADI threshold evaluation, emitting per-gauge alert state (normal / warning / severe)
- Read-only dashboard: current levels, per-gauge alert status, recent-history trend
- OpenTelemetry observability across the pipeline
- Integration tests with Testcontainers
- Architecture decisions recorded as four ADRs (threshold placement, dashboard state, deploy target, surge-evaluator algorithm)
- Kubernetes + Argo CD (GitOps) deployment; Azure Container Apps baseline

**Repository:**
https://github.com/goldbarth/port-tidewatch  
**Blog (German):**
https://www.goldbarth.dev/projects/port-tidewatch  
**Surge-evaluator decision (German):**
https://www.goldbarth.dev/decisions/surge-evaluator-decisions

---

## MetricGate – Multi-Tenant Rate Limiting & Distributed Microservices

<p>
  <img src="https://img.shields.io/github/v/release/goldbarth/MetricGate"/>
  <a href="https://github.com/goldbarth/MetricGate/actions/workflows/ci.yml">
    <img src="https://github.com/goldbarth/MetricGate/actions/workflows/ci.yml/badge.svg" alt="CI" />
  </a>
  <a href="https://dev.azure.com/goldbarth/MetricGate/_build/latest?definitionId=1">
    <img src="https://dev.azure.com/goldbarth/MetricGate/_apis/build/status%2Fgoldbarth.MetricGate" alt="Azure Pipelines" />
  </a>
</p>

A tenant-aware quota and rate-limiting backend for SaaS APIs. Three independently deployable .NET 10 services enforce plan limits in real time, persist usage events for billing and audit, and support multi-level tenant hierarchies (root, reseller, customer).

The goal is not feature breadth, but distributed correctness — consistent enforcement under concurrent load, cache coherence across a tenant hierarchy, and a hot path with measured p95 latency of 109 µs.

### Technical Focus

- Three-service architecture (Plans / Enforcement / Usage) with strict per-service database ownership
- Sub-10 ms enforcement API: fixed-window quotas + token bucket rate limits via atomic Redis Lua scripts
- Multi-level tenant hierarchy with plan inheritance and constraint validation across the tree
- Tag-based cache cascade invalidation — a single hierarchy change evicts plan resolutions, counters and auth decisions across an entire sub-tree
- Event-driven usage persistence via Kafka (Redpanda) with idempotent ingest and dedup window
- Resource-based authorization: custom `IAuthorizationHandler` resolves tenant subtree ownership at request time
- OIDC authentication (Keycloak) for admin surfaces; API-key authentication on the enforcement hot path
- Service-to-service auth on cache-miss path (Enforcement → Plans) via internal JWT
- Clean Architecture per service (Domain / Application / Infrastructure / API) — no mediator, application services injected directly
- BenchmarkDotNet hot-path benchmarks: p95 cache hit 109 µs, per-request at 50 concurrent 4 µs
- Integration tests with Testcontainers

**Repository:**
https://github.com/goldbarth/MetricGate

---

## Ingestor – Backend Reliability & Resilient Processing

  <p>
    <img src="https://img.shields.io/github/v/release/goldbarth/Ingestor"/>
    <a href="https://github.com/goldbarth/Ingestor/actions/workflows/ci.yml">
      <img src="https://github.com/goldbarth/Ingestor/actions/workflows/ci.yml/badge.svg" alt="CI" />
    </a>
  </p>

A reliable import system for processing delivery data from multiple suppliers. Built for a fictional furnishing logistics company (Fleetholm Logistics).

The goal is not domain complexity, but technical reliability — retry logic, dead-letter handling, idempotent processing and full audit trails.
Extended with a Blazor Server web UI.

### Technical Focus

- Two-process architecture (API Host + Worker Host)
- Database-backed job orchestration (Outbox pattern)
- Configurable dispatcher abstraction (DB queue or RabbitMQ, config-switch)
- Message-driven processing via RabbitMQ with dead-letter exchange
- Automatic retries with exponential backoff
- Dead-letter handling with manual requeue
- Idempotent file ingestion (content hash + supplier)
- Explicit status model with strict state transitions
- Structured error classification (transient vs. permanent)
- Batch import with chunk processing and partial success handling
- Full audit trail for every job lifecycle event
- OpenTelemetry tracing and structured logging
- Integration tests with Testcontainers
- Blazor Server web UI (Dashboard, Imports, Dead Letters)

**Repository:**
https://github.com/goldbarth/Ingestor

---

# Systems Engineering Background

Beyond backend architecture, I have built system-oriented projects in C++ and Unreal Engine.

These projects shaped how I approach backend systems today:  
explicit state handling, clear ownership and predictable behavior.

## Solar System Simulation (Unreal Engine C++)

A physics-driven N-body simulation with fixed time-step integration.

- GameMode as composition root  
- Explicit CelestialBody registry  
- Separation between simulation logic and visualization  
- Real mass–velocity–distance interaction

Repository:
https://github.com/goldbarth/SolarSystem

---

## 3D Model Viewer (C++ / OpenGL)

A minimal rendering engine capable of loading and displaying OBJ models.

- Explicit resource and state handling  
- Camera system (pan, rotate, zoom)  
- Shader-based lighting

Repository:
https://github.com/goldbarth/3DModelViewer

---

Open to backend-oriented engineering roles in Hamburg.
