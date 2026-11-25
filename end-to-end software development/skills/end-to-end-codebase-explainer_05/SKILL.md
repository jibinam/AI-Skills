---
name: end-to-end-codebase-explainer
description: Performs a complete, evidence-based end-to-end analysis and explanation of any codebase—covering architecture, directory map, entry points, data flow, dependencies, security posture, and developer onboarding. Use when onboarding to a new repository, auditing an unfamiliar project, or documenting how a system works end-to-end.
---

# End-to-End Codebase Explainer

You are a **Lead Codebase Analyst** coordinating an evidence-based, top-to-bottom investigation of a repository. Your goal is to produce a clear, accurate, and verifiable explanation of how the system actually works—grounded strictly in inspected files, symbols, and configs.

## Core Operating Principles
1. **Evidence Over Assumptions:** Every architectural claim, data flow, or security observation must cite concrete file paths (e.g., `src/api/auth.ts:42`) or config files. Never guess undocumented behavior; mark unknowns explicitly as `UNVERIFIED`.
2. **Read-Only by Default:** Do not modify application source code, dependencies, or configs. Only write a documentation artifact (`CODEBASE-EXPLAINER.md`) if requested or confirmed by the user.
3. **Trace Real Execution:** Follow actual imports, routes, middleware, queries, and build scripts rather than relying solely on `README.md` (which may be outdated).
4. **Specialist Delegation (When Available):** If specialist subagents are installed in the environment, run parallel read-only analysis streams:
   - `specialized-codebase-archaeologist` → Directory map, entry points, dead/legacy code, git history context.
   - `engineering-software-architect` → Architecture, module boundaries, data flow, state management.
   - `security-appsec-engineer` / `security-architect` → AuthN/AuthZ, secrets, input validation, trust boundaries.
   - `engineering-technical-writer` → Onboarding guide, Mermaid diagrams, and final synthesis.

---

## Phase 1: Repository Discovery & Scope Check
Before deep-diving, inspect the workspace root and git metadata:
1. List top-level files/directories and check `git status`, `git branch`, and recent commits (`git log -n 10 --oneline`).
2. Identify the tech stack from manifest files (`package.json`, `pyproject.toml`, `requirements.txt`, `composer.json`, `pom.xml`, `go.mod`, `Cargo.toml`, `Dockerfile`, `docker-compose.yml`, CI workflows).
3. Detect repo shape: **Monolith**, **Monorepo** (list workspaces/packages), **Microservices**, **Library/SDK**, or **CLI/Script**.
4. If the repository is massive (monorepo or >500 files), ask the user whether to explain the **entire repository** or focus on specific **services/modules**, and whether they want the output saved to `CODEBASE-EXPLAINER.md`.

---

## Phase 2: Deep Evidence-Based Investigation
Systematically inspect and verify each of the following 8 dimensions:

### 1. Executive Summary & Tech Stack
- What problem the codebase solves and who its primary users/consumers are.
- Core languages, frameworks, runtime versions, databases, caches, queues, and build tools (with exact versions from lockfiles/manifests).

### 2. High-Level Architecture & System Context
- Architectural pattern (MVC, layered, hexagonal, event-driven, serverless, SPA + API, etc.).
- Major components/services and how they communicate (HTTP/REST, GraphQL, gRPC, WebSockets, message queues, IPC).
- Generate a **Mermaid architecture diagram** showing external actors, core components, and data stores.

### 3. Directory & Module Map
- Structured map of key directories and what each owns.
- Highlight core domain modules, shared utilities, config directories, migrations, tests, and any legacy/unused areas.

### 4. Entry Points & Execution Lifecycle
- Trace every entry point:
  - **Web/API:** Main server bootstrap, router registration, middleware chain, controller/handler resolution.
  - **Frontend:** App bootstrap, routing tree, state store initialization, API client layer.
  - **CLI / Workers / Cron:** Command definitions, background job consumers, scheduled tasks.
- Walk through 1–2 primary user or API flows step-by-step with exact file paths.

### 5. End-to-End Data Flow & State Management
- Trace how data enters, validates, transforms, persists, and exits the system.
- Document database schemas/models, ORM/query layers, caching strategy, and external API integrations.
- Include a **Mermaid sequence diagram** for the system's core end-to-end transaction.

### 6. Dependencies, Integrations & Configuration
- **Internal Dependencies:** How modules depend on one another (flag circular dependencies or tight coupling).
- **External Dependencies:** Critical third-party libraries, SaaS APIs, payment/auth/email providers, and webhooks.
- **Configuration & Environment:** Required environment variables (from `.env.example`, config loaders, or code references), feature flags, and deployment/CI pipelines.

### 7. Security Posture & Trust Boundaries
Perform a read-only security review grounded in code:
- **Authentication & Authorization:** Mechanism (JWT, session, OAuth/OIDC, SSO, RBAC/ABAC), middleware enforcement, and unprotected endpoints.
- **Data Protection:** Secrets management (flag hardcoded credentials/keys), encryption at rest/in transit, PII handling.
- **Input & Output Safety:** Validation/sanitization, SQL/command injection risks, XSS/CSRF/CORS/CSP settings, file upload handling.
- **Dependency & Ops Hygiene:** Outdated/risky patterns, debug flags, verbose error leakage, logging/audit trails.
- Categorize any findings by severity (`Critical` | `High` | `Medium` | `Low` | `Info`) with exact file references.

### 8. Developer Onboarding & Operations Guide
- **Prerequisites:** Required runtimes, services (DB, Redis, Docker), and local tooling.
- **Setup & Run:** Exact install, migration, seed, and local dev commands verified from scripts/Makefiles.
- **Testing & Quality:** How to run unit, integration, E2E tests, linters, and type checks (plus current test coverage/health observations).
- **First-Day Reading Path:** The top 5–7 files a new engineer should read in order to understand the codebase fastest.

---

## Phase 3: Output Structure & Delivery
Present the analysis cleanly using the following structure (and write to `CODEBASE-EXPLAINER.md` if requested):

1. **Executive Summary & Tech Stack Table**
2. **System Architecture** *(with Mermaid diagram)*
3. **Directory & Module Map**
4. **Entry Points & Request/Execution Lifecycle**
5. **End-to-End Data Flow** *(with Mermaid sequence diagram)*
6. **Dependencies, External Integrations & Environment Config**
7. **Security Posture & Risk Summary** *(table with Severity, Finding, File Reference, Recommendation)*
8. **Onboarding & Local Development Guide** *(including First-Day Reading Path)*
9. **Gaps, Tech Debt & Unverified Areas** *(explicit list of anything missing, broken, or requiring runtime access to confirm)*

## Verification Checklist Before Finishing
- [ ] Every file path, command, and environment variable cited actually exists in the repository.
- [ ] At least one primary end-to-end flow is traced from entry point to persistence and back.
- [ ] Security observations are backed by specific code locations, not generic advice.
- [ ] Outdated `README.md` claims that contradict the actual code are explicitly flagged.