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
  Clean Architecture · Explicit Domain Modeling · Deterministic Systems
</p>

<p align="center">📍 Hamburg, Germany</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/ASP.NET%20Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/EF%20Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/xUnit-2C3E50"/>
  <img src="https://img.shields.io/badge/FluentAssertions-2C3E50"/>
</p>

<p align="center"><i>
In pain we grow, in joy we recognize what already is.
</i></p>

---

## About

I’m a backend-focused .NET engineer with a strong interest in clean architecture and explicit system design.

My background in game development shaped how I think about invariants, performance, and deterministic behavior.  
Today, I focus on building maintainable backend systems with clear boundaries, explicit error handling, and testable domain logic.

---

# Primary Engineering Case

## ServiceDeskLite – Explicit Architecture & Domain Modeling

<p>
  <img src="https://img.shields.io/badge/.NET_10-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/Blazor-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/EF_Core-512BD4?logo=dotnet&logoColor=fff"/>
  <img src="https://img.shields.io/badge/Minimal_API-111111"/>
  <img src="https://img.shields.io/badge/Clean_Architecture-000000"/>
</p>

A deliberately structured .NET 10 backend application focused on architectural clarity, explicit domain modeling and disciplined layering.

The goal is not feature quantity, but architectural understanding through hands-on implementation.

### Architectural Focus

• Strict layered architecture (Domain / Application / Infrastructure / API / Web)  
• Ubiquitous language reflected in use-case structure  
• Explicit domain workflow rules  
• Result-pattern based error handling (no exceptions in Application layer)  
• RFC 9457 ProblemDetails strategy  
• Strongly-typed IDs  
• Deterministic paging & sorting (CreatedAt + Id tie-breaker)  
• Explicit UnitOfWork commit boundary  
• EF Core (SQLite) + InMemory provider switch  
• End-to-End tests with provider matrix

<p>
  <a href="https://github.com/goldbarth/ServiceDeskLite">
    <img src="https://img.shields.io/badge/View_ServiceDeskLite-000000?logo=github&logoColor=white"/>
  </a>
</p>

---

# Selected Engineering Work

## BlazorStore (Blazor – Unidirectional State Architecture)

An experimental Blazor application focused on explicit state management using a reduced Redux-inspired pattern.

The YouTube playlist functionality serves primarily as a domain to exercise architectural patterns — not as the product focus.

Core concepts:

• Centralized Store with explicit state transitions  
• Unidirectional data flow  
• Action → Reducer → State pipeline  
• Immutable state updates  
• Snapshot-based undo/redo  
• Effect isolation (side effects separated from reducers)  
• Deterministic state transitions  
• Component dispatch-only architecture

The project explores predictable UI behavior through strict state ownership and separation of concerns.

The architecture intentionally minimizes implicit component state
to enforce explicit and traceable application behavior.

<p>
  <a href="https://github.com/goldbarth/BlazorStore">
    <img src="https://img.shields.io/badge/View_BlazorStore-181717?logo=github&logoColor=white"/>
  </a>
</p>

---

# Systems Engineering Background

Beyond backend architecture, I have built low-level system-oriented projects in C++ and Unreal Engine.

These projects focus on explicit state modeling, deterministic simulation and clear separation of responsibilities.

## Solar System Simulation (Unreal Engine C++)

A physics-driven N-body gravitational simulation.

• GameMode used as composition root  
• Explicit CelestialBody registry for state coordination  
• Interface + component-based debug visualization  
• Virtual body abstraction for orbit debugging  

• Real mass–velocity–distance interaction (no predefined or scripted orbits)  
• Fixed time-step integration model  
• Documented physical scaling model (AU, mass ratios, velocity calculations)

Extensive technical documentation included.

<p>
  <a href="https://github.com/goldbarth/SolarSystem">
    <img src="https://img.shields.io/badge/SolarSystem-181717?logo=github&logoColor=white"/>
  </a>
</p>


## 3D Model Viewer (C++ / OpenGL)

A minimal rendering engine capable of loading and displaying OBJ models.

• Explicit resource and state handling  
• Camera system (pan, rotate, zoom)  
• Wireframe rendering mode  

• Assimp-based model import  
• OpenGL rendering pipeline  
• Shader-based lighting (ambient / diffuse / specular)

Built with GLFW, GLAD and modern OpenGL.

<p>
  <a href="https://github.com/goldbarth/3DModelViewer">
    <img src="https://img.shields.io/badge/3DModelViewer-181717?logo=github&logoColor=white"/>
  </a>
</p>

---

## Working Approach

I value clarity, ownership and intentional system design.

For me, engineering means making decisions explicit — in code, in architecture and in collaboration.

• Boundaries are deliberate, not accidental  
• Domain logic is expressed, not hidden  
• Trade-offs are discussed explicitly  
• Quality is a responsibility, not an afterthought

---

Open to backend-oriented engineering roles in Hamburg that value architectural clarity and thoughtful collaboration.
