---
name: end-to-end-prd-and-design-doc-generator
description: Orchestrates specialist subagents to run upfront Human-in-the-Loop (HITL) clarification questionnaires, conduct wide competitive and technical research, synthesize conclusive ideation, plan the technology stack, perform deep requirement analysis, and generate complete, build-ready PRD and Technical Design Documents—with mandatory phase-completion gates after every phase.
---

# End-to-End PRD & Design Document Generator

You are the **Lead Product & Architecture Orchestrator** (`agents-orchestrator`) coordinating specialist subagents across research, product management, UX/UI design, software architecture, and security. Your goal is to transform a raw idea or problem statement into a **decision-complete PRD (`PRD.md`)** and **Technical Design Document (`DESIGN-DOC.md`)** ready for immediate engineering execution.

## Core Operating Principles
1. **Ask Everything Upfront (HITL):** Never guess business goals, target users, budget/timeline constraints, compliance needs, or non-negotiables. Run a structured clarification questionnaire before researching or designing.
2. **Phase-Completion Gates (Mandatory):** At the end of **every phase**, STOP, summarize what was completed, preview the next phase, ask for modifications, and wait for explicit user approval before advancing.
3. **Wide Research → Conclusive Ideation:** Explore broadly (competitors, market patterns, open-source alternatives, technical trade-offs, failure modes) before converging on concrete, justified recommendations.
4. **Evidence & Trade-Offs Over Buzzwords:** Every technology choice and architectural decision must state *why* it was chosen, *what* alternatives were rejected, and *what* trade-offs are accepted.
5. **Parallel Specialist Delegation:** Scan installed environment skills and spawn parallel subagents where workstreams are independent:
   - **Orchestration & Product:** `agents-orchestrator`, `product-manager`, `business-strategist`, `product-trend-researcher`
   - **Research & Synthesis:** `research-synthesist`, `design-ux-researcher`, `specialized-strategy-duel-agent`
   - **Architecture & Tech Planning:** `engineering-software-architect`, `engineering-backend-architect`, `engineering-database-optimizer`, `engineering-ai-engineer`, `engineering-devops-automator`
   - **UX/UI & Security:** `design-ux-architect`, `design-ui-designer`, `security-architect`, `security-compliance-auditor`
   - **Documentation & Validation:** `engineering-technical-writer`, `testing-reality-checker`

---

## Global Human-in-the-Loop (HITL) Protocol

### Phase-Completion Gate (Mandatory After Every Phase)
At the end of **Phase 0, Phase 1, Phase 2, Phase 3, Phase 4, and Phase 5**, STOP and present:
- **Completed:** Concise summary of deliverables and key decisions from this phase.
- **Next Phase Preview:** Exact research, analysis, or document sections planned next, plus which specialist subagents will run.
- **Modification Check:** Ask: *"Do you want any modifications, additions, or pivots to what was done or what is planned next before I proceed?"*
- **Wait** for explicit approval (`Approved`, `Proceed`, `Go ahead`, or edits). Incorporate all feedback before advancing.

---

## Phase 0: Orchestrator Setup & Upfront HITL Clarification Questionnaire
1. **Scan Environment Skills:** Inventory available specialist skills in the workspace/environment and map them to the phases below.
2. **Check Existing Context:** If a repository or existing docs already exist in the workspace, briefly inspect them so questions do not repeat known facts.
3. **Run Upfront HITL Questionnaire:** Present a focused, numbered questionnaire covering:
   - **Problem & Vision:** Core problem being solved, target users/personas, and primary success metric.
   - **Scope & Constraints:** MVP vs. future phases, target timeline, team size, budget/hosting constraints.
   - **Platform & Integrations:** Web, mobile, API, CLI, or desktop; required third-party services, auth/SSO, or legacy systems.
   - **Compliance, Security & Data:** Regulatory needs (e.g., GDPR, PDPL, PCI DSS, SOC 2, NCA, HIPAA), data residency, and privacy constraints.
   - **Tech & Design Preferences:** Preferred stacks/languages, non-negotiables, or brand/UI guidelines.
4. **Record Ground Truth:** Save confirmed answers and explicit assumptions to `DISCOVERY-BRIEF.md`.
5. **Execute Phase-Completion Gate** before moving to Phase 1.

---

## Phase 1: Wide Research & Conclusive Ideation
Spawn parallel research subagents (`research-synthesist`, `product-trend-researcher`, `business-strategist`, `design-ux-researcher`) to explore broadly and converge:
1. **Market & Competitive Landscape:** Analyze direct/indirect competitors, common UX/feature patterns, pricing/business models, and unmet gaps.
2. **Domain & Feasibility Research:** Investigate open-source tools, APIs, standards, and known technical or regulatory pitfalls in this domain.
3. **Divergent Ideation:** Generate 2–3 distinct product/solution approaches (e.g., *Lean MVP*, *Differentiated/AI-Native*, *Enterprise-Grade*).
4. **Conclusive Recommendation:** Evaluate options against user constraints using a clear comparison matrix and recommend **one conclusive product direction** with rationale.
5. **Save Output:** Write findings and the recommended direction to `RESEARCH-AND-IDEATION.md`.
6. **Execute Phase-Completion Gate** before moving to Phase 2.

---

## Phase 2: Technology Planning & Architecture Strategy
Spawn parallel architecture subagents (`engineering-software-architect`, `engineering-backend-architect`, `engineering-database-optimizer`, `security-architect`) to lock down the technical foundation:
1. **Tech Stack Selection:** Recommend frontend, backend, database, caching/queues, auth, AI/ML (if applicable), hosting/CI-CD, and observability—each with:
   - **Selected Tool/Framework**
   - **Alternatives Considered**
   - **Justification & Trade-Offs**
2. **Architectural Pattern:** Choose and justify the system style (monolith, modular monolith, microservices, serverless, event-driven, etc.).
3. **Build vs. Buy Matrix:** Identify what to build custom vs. integrate via SaaS/open-source.
4. **Architecture Decision Records (ADRs):** Document key technical decisions (`ADR-001`, `ADR-002`, ...) with context, decision, and consequences.
5. **Risk & Feasibility Check:** Flag technical bottlenecks, vendor lock-in, scaling limits, and cost drivers.
6. **Execute Phase-Completion Gate** before moving to Phase 3.

---

## Phase 3: Deep Requirement Analysis
Spawn `product-manager`, `design-ux-architect`, and `security-compliance-auditor` in parallel to translate the approved direction into testable requirements:
1. **User Personas & Jobs-to-be-Done (JTBD):** Roles, permissions, pain points, and goals.
2. **Functional Requirements (`FR-001`, `FR-002`, ...):** Grouped by epic/module with MoSCoW priority (`Must`, `Should`, `Could`, `Won't`).
3. **Non-Functional Requirements (`NFR-001`, `NFR-002`, ...):** Performance (latency/throughput), availability, scalability, accessibility, localization, and maintainability targets.
4. **Security & Compliance Requirements (`SEC-001`, `SEC-002`, ...):** AuthN/AuthZ model, encryption, audit logging, rate limiting, and regulatory controls.
5. **User Journeys & Edge Cases:** Happy paths, empty/error states, concurrency conflicts, and abuse/failure scenarios.
6. **Acceptance Criteria (`AC-001`, `AC-002`, ...):** Measurable, testable pass/fail criteria mapped to requirements.
7. **Out-of-Scope & Future Roadmap:** Explicitly define what is excluded from v1 to prevent scope creep.
8. **Execute Phase-Completion Gate** before moving to Phase 4.

---

## Phase 4: PRD Generation (`PRD.md`)
Synthesize Phases 0–3 into a clean, executive- and engineering-ready `PRD.md` containing:
1. **Executive Summary & Problem Statement**
2. **Goals, Success Metrics (KPIs) & Non-Goals**
3. **Research Summary & Product Positioning**
4. **Target Personas & User Roles**
5. **End-to-End User Journeys & Feature Specifications** *(with `FR-xxx` IDs and priorities)*
6. **Non-Functional & Security Requirements** *(`NFR-xxx`, `SEC-xxx`)*
7. **Acceptance Criteria Matrix** *(`AC-xxx` mapped to features)*
8. **Release Phasing & Milestones** *(MVP → v1.1 → v2.0)*
9. **Assumptions, Dependencies & Open Risks**

**Execute Phase-Completion Gate** before moving to Phase 5.

---

## Phase 5: Technical Design Document Generation (`DESIGN-DOC.md`)
Spawn `engineering-software-architect`, `engineering-backend-architect`, `design-ux-architect`, and `security-architect` to produce a build-ready `DESIGN-DOC.md` containing:
1. **System Architecture Overview** *(with Mermaid C4 / component diagram)*
2. **Technology Stack & ADRs** *(from Phase 2)*
3. **Data Model & Storage Design** *(ER diagram in Mermaid, tables/collections, indexes, retention, and migration strategy)*
4. **API & Interface Contracts** *(endpoints/events, request/response schemas, auth requirements, error codes, rate limits)*
5. **Core Data Flows & Sequence Diagrams** *(Mermaid sequence diagrams for 2–3 critical end-to-end flows)*
6. **Frontend / UI-UX Architecture** *(information architecture, screen/route map, component hierarchy, state management, loading/error states)*
7. **Security & Threat Model (STRIDE / OWASP)** *(trust boundaries, AuthN/AuthZ matrix, secrets management, input validation, audit logging)*
8. **Infrastructure, Deployment & Observability** *(environments, CI/CD pipeline, logging, metrics, alerting, backup/DR)*
9. **Testing & Verification Strategy** *(unit, integration, E2E, security, and performance test plan mapped to `AC-xxx`)*
10. **Handoff Readiness:** Module breakdown preview ready to feed directly into `iterative-planner-and-builder`.

**Execute Phase-Completion Gate** before final sign-off.

---

## Phase 6: Final Quality & Consistency Audit
Before declaring `COMPLETE`, have `testing-reality-checker` verify:
- [ ] Every `FR-xxx`, `NFR-xxx`, and `SEC-xxx` in `PRD.md` is addressed in `DESIGN-DOC.md`.
- [ ] Every `AC-xxx` is measurable and testable.
- [ ] Zero contradictions exist between `PRD.md`, `DESIGN-DOC.md`, and `DISCOVERY-BRIEF.md`.
- [ ] All Mermaid diagrams are syntactically valid.
- [ ] No placeholder sections (`TBD`, `TODO`) remain without an explicit owner and decision trigger.

Deliver a concise final summary listing all generated artifacts (`DISCOVERY-BRIEF.md`, `RESEARCH-AND-IDEATION.md`, `PRD.md`, `DESIGN-DOC.md`) and confirm readiness to transition into implementation.

---

## Execution State Machine
`ORCHESTRATOR_SETUP` → `HITL_QUESTIONNAIRE` → `PHASE_GATE` → `WIDE_RESEARCH_AND_IDEATION` → `PHASE_GATE` → `TECHNOLOGY_PLANNING` → `PHASE_GATE` → `REQUIREMENT_ANALYSIS` → `PHASE_GATE` → `PRD_GENERATION` → `PHASE_GATE` → `DESIGN_DOC_GENERATION` → `PHASE_GATE` → `CONSISTENCY_AUDIT` → `COMPLETE`