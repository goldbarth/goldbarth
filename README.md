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
  .NET Engineer, moving into AI Engineering<br/>
  Clean Architecture · Reliable Systems · LLM Integration
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

I came into software through game development, and my interest has kept moving ever since.

Gameplay design first, until I noticed it was not mine. Then engines and systems, which is where the questions got interesting: state consistency, explicit behavior, performance under constraints. Those questions led into backend engineering, where the same concerns show up with different names. Growing from junior to mid-level with ownership of three projects, I learned to care about systems that stay understandable and changeable over time.

Right now that path leads into AI Engineering. Not away from the backend but built on it: an LLM is an unpredictable external service that wants to change your domain state, which turns out to be an architecture problem more than a prompt problem. I have been building agents that reach the domain only through the same command handlers as every other caller, that get fenced in by a guard pipeline rather than trusted, and that hand a decision back to a person when it matters.

The wandering used to look like a lack of focus to me. I have come to see it as how I work: the constant is the curiosity, and it has been the most durable thing I have. Based in Hamburg, open to hybrid roles.

---

# Featured Projects

Four backend projects that highlight complementary engineering concerns:  
**ServiceDeskLite** focuses on architecture and domain boundaries - and ships an autonomous LLM agent (twelve tools, hybrid RAG, guard pipeline) inside them.  
**port-tidewatch** focuses on water-level ingestion and storm-surge alerting for port infrastructure.  
**MetricGate** focuses on distributed rate limiting, multi-tenant hierarchy and low-latency enforcement.  
**Ingestor** focuses on reliability, fault handling and operational resilience.

---

## ServiceDeskLite – An Autonomous AI Agent on a Clean Architecture

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
  <img src="https://img.shields.io/badge/Claude-tool%20calling-D97757?logo=claude&logoColor=white"/>
  <img src="https://img.shields.io/badge/Voyage%20AI-embeddings-1a1a1a"/>
  <img src="https://img.shields.io/badge/PostgreSQL%20%2B%20pgvector-4169E1?logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/OpenTelemetry-425CC7?logo=opentelemetry&logoColor=white"/>
</p>

A deliberately structured .NET 10 backend with a shipped LLM feature: users describe an issue in free text, a Claude model decides via tool calling what to do across twelve tools, and the response streams token-by-token to the browser (SSE). Since v1.6.0 the agent also works unprompted - a background worker reviews open tickets on a schedule and refers every high-impact decision to a person. The agent lives as an edge adapter: it acts on the system exclusively through the same command handlers as the REST API, so validation, audit trail and outbox apply to AI-created tickets unchanged.

The surface has grown across releases. What has not changed is the structure underneath: explicit boundaries, reviewable design decisions, and every feature reaching the domain through the same handlers. The agent is their hardest stress test, and it is fenced in rather than trusted.

### AI / LLM Focus

- Twelve tools against the Anthropic Messages API: create, update, search, comment, change status, assign, auto-route, ticket and knowledge-base retrieval, grounding check, remember/recall - every tool executes through existing command handlers, no special path into the domain
- Autonomous ticket worker: the tool-calling loop was extracted from the chat endpoint, so worker and assistant share one loop, one set of tools and one set of guards; a review guard refuses high-impact writes and the model posts its reasoning as a comment for a human to approve (ADR 0037)
- Agent sandbox: a guard pipeline at the single point where model intent becomes execution - unknown tool names refused, argument size capped, writes budgeted per turn, per-owner rate limits; a refusal returns as an ordinary error result the model can act on (ADR 0035)
- Hybrid retrieval: semantic (Voyage embeddings in pgvector) and keyword signals fused via Reciprocal Rank Fusion, cross-lingual, computed async by a background worker off the write path (ADR 0030)
- Knowledge-base RAG with cited sources, plus a deterministic grounding check the model runs against its own draft, so a weak answer is hedged or re-retrieved rather than asserted (ADR 0029, ADR 0031)
- Tool output treated as untrusted input: parsed and guarded before touching the domain; rejected inputs return as error `tool_result`s so the model self-corrects in a bounded loop
- Server-side conversation state and long-term memory (pgvector), with date/timezone injected per request so relative deadlines ("by Friday morning") resolve correctly (ADR 0026)
- Observability: Prometheus metrics and OpenTelemetry traces for tool latency, error rates, token usage and retrieval confidence; an unmeasurable rate reports as unknown, not as zero (ADR 0034, ADR 0036)
- Offline agent evaluation suite: the real endpoint, loop, guards and tools run against a scripted model, so tool calling, RAG grounding and streaming are pinned in CI without a network call
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

A focused ingestion service for port water-level telemetry, with threshold-based storm-surge alerting and a read-only monitoring dashboard. Since v1.2.0 it runs on real data: the public WSV/PEGELONLINE feed for the Hamburg Elbe gauges (St. Pauli, Zollenspieker, Over, Bunthaus), with a scripted simulator selectable at startup. The domain is modelled on the Hamburg storm-surge warning service (WADI): a warning is raised when an expected surge peak can exceed 4.50 m above sea level (NHN) / 2.40 m above mean high water (MThw), escalating to severe at 5.50 m NHN (BSH *sehr schwere Sturmflut*).

A reliable, observable ingestion pipeline end to end: one domain, one ingestion path, no write operations from the UI.

### Technical Focus

- Three-component architecture: reading source (.NET, live feed or simulator) → ingestion service (.NET) → read-only Angular dashboard
- Message-driven ingestion via RabbitMQ with a dead-letter path for poison messages
- Staged WADI threshold evaluation with hysteresis and trend awareness, emitting per-gauge alert state (normal / warning / severe)
- Honest freshness (v1.3.0): liveness is keyed on measurement age and staleness on data arrival, so a fresh poll of a stale gauge no longer reads as live
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

The focus is distributed correctness: consistent enforcement under concurrent load, cache coherence across a tenant hierarchy, and a hot path with measured p95 latency of 109 µs.

### Technical Focus

- Three-service architecture (Plans / Enforcement / Usage) with strict per-service database ownership
- Sub-10 ms enforcement API: fixed-window quotas + token bucket rate limits via atomic Redis Lua scripts
- Multi-level tenant hierarchy with plan inheritance and constraint validation across the tree
- Tag-based cache cascade invalidation: a single hierarchy change evicts plan resolutions, counters and auth decisions across an entire sub-tree
- Event-driven usage persistence via Kafka (Redpanda) with idempotent ingest and dedup window
- Resource-based authorization: custom `IAuthorizationHandler` resolves tenant subtree ownership at request time
- OIDC authentication (Keycloak) for admin surfaces; API-key authentication on the enforcement hot path
- Service-to-service auth on cache-miss path (Enforcement → Plans) via internal JWT
- Clean Architecture per service (Domain / Application / Infrastructure / API), no mediator, application services injected directly
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

The focus is technical reliability: retry logic, dead-letter handling, idempotent processing and full audit trails.
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

<p align="center">
  Open to hybrid backend and AI engineering roles in Hamburg.<br/>
  <sub>Currently building LLM agents that stay inside their architectural boundaries.</sub>
</p>

<p align="center">
  <a href="https://www.goldbarth.dev/">Blog</a> ·
  <a href="https://www.linkedin.com/in/felix-wahl-6763791b9/">LinkedIn</a> ·
  <a href="mailto:hello@goldbarth.dev">hello@goldbarth.dev</a>
</p>
