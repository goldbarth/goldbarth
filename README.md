<div align="right">
  <a href="https://www.linkedin.com/in/felix-wahl-6763791b9/">
    <img src="https://custom-icon-badges.demolab.com/badge/LinkedIn-0A66C2?logo=linkedin-white&logoColor=fff" align="absmiddle" alt="LinkedIn Badge">
  </a>
  <a href="mailto:felix.wahl@live.de">
    <img src="https://custom-icon-badges.demolab.com/badge/Email-333333?logo=mail&logoColor=fff" align="absmiddle" alt="Email Badge">
  </a>
</div>

<h1 align="center">Felix Wahl</h1>

<p align="center">
  Backend-Focused .NET Engineer<br/>
  Clean Architecture · Explicit Domain Logic · Predictable Systems
</p>

<p align="center">📍 Hamburg, Germany</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/ASP.NET%20Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/EF%20Core-512BD4?logo=dotnet&logoColor=fff"/>
</p>

---

## About

I’m a backend-focused .NET engineer with a strong interest in structured application design and explicit domain logic.

My background in game development shaped how I think about system behavior, state consistency and performance.  
Today, I focus on backend systems that remain understandable and changeable over time — with clear boundaries, explicit error handling and testable business logic.

---

# Primary Engineering Case

## ServiceDeskLite – Structured Architecture & Domain Design

<p>
  <img src="https://img.shields.io/github/v/release/goldbarth/ServiceDeskLite"/>
  <a href="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/ci.yml">
    <img src="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/ci.yml/badge.svg" alt="CI" />
  </a>
  <a href="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/docs.yml">
    <img src="https://github.com/goldbarth/ServiceDeskLite/actions/workflows/docs.yml/badge.svg" alt="Docs" />
  </a>
</p>

A deliberately structured .NET 10 backend application demonstrating clean layering, explicit domain rules and consistent error handling in a realistic setup.

The goal is not feature breadth, but structural clarity, explicit boundaries, and reviewable design decisions.

### Architectural Focus

- Strict layered architecture (Domain / Application / Infrastructure / API / Web)
- Explicit domain workflow rules
- Result-based error handling (no exceptions crossing application boundaries)
- RFC 9457 ProblemDetails strategy
- Strongly-typed IDs
- Deterministic paging & sorting (CreatedAt + Id tie-breaker)
- Explicit UnitOfWork commit boundary
- EF Core (PostgreSQL) + InMemory provider switch
- End-to-end tests with provider matrix

• **Repository:**
https://github.com/goldbarth/ServiceDeskLite

• **Documentation:**
https://goldbarth.github.io/ServiceDeskLite

---

# Selected Engineering Work

## BlazorStore (Blazor – Unidirectional State Architecture)

An experimental Blazor application exploring explicit and centralized state management using a reduced Redux-inspired pattern.

The playlist domain serves mainly as a vehicle to exercise architectural patterns — not as a product showcase.

Core concepts:

• Centralized store with explicit state transitions  
• Unidirectional data flow  
• Action → Reducer → State pipeline  
• Immutable state updates  
• Snapshot-based undo/redo  
• Side effects isolated from reducers  
• Component dispatch-only design

The project demonstrates how predictable state handling reduces hidden UI behavior and simplifies debugging.

Repository: 
https://github.com/goldbarth/BlazorStore

---

# Systems Engineering Background

Beyond backend architecture, I have built system-oriented projects in C++ and Unreal Engine.

These projects shaped how I approach backend systems today:  
explicit state handling, clear ownership and predictable behavior.

## Solar System Simulation (Unreal Engine C++)

A physics-driven N-body simulation with fixed time-step integration.

• GameMode as composition root  
• Explicit CelestialBody registry  
• Separation between simulation logic and visualization  
• Real mass–velocity–distance interaction

Repository:
https://github.com/goldbarth/SolarSystem

---

## 3D Model Viewer (C++ / OpenGL)

A minimal rendering engine capable of loading and displaying OBJ models.

• Explicit resource and state handling  
• Camera system (pan, rotate, zoom)  
• Shader-based lighting

Repository:
https://github.com/goldbarth/3DModelViewer

---

## Working Approach

I value clarity, ownership and deliberate system design.

• Boundaries are intentional  
• Domain logic is visible  
• Trade-offs are discussed  
• Quality is a shared responsibility

---

Open to backend-oriented engineering roles in Hamburg.
