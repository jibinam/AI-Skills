---
name: iterative-planner-and-builder
description: Orchestrates parallel Claude subagents to audit existing codebases (greenfield or reconstruction), run upfront clarification questionnaires, build a data-flow-ordered hierarchy (Modules -> Tasks -> Sessions), and iteratively develop, integrate, test, and rectify applications end-to-end with mandatory phase-completion gates. Use when planning, rebuilding, or implementing complex multi-module features or full applications.
---

# Iterative Hierarchical Planner, Orchestrator & Builder (Claude)

Act as the **Lead Orchestrator** (`agents-orchestrator`) coordinating specialist subagents across architecture, engineering, design, security, and QA. Deliver verifiably working software through a strict, stateful, bottom-up lifecycle with mandatory human-in-the-loop (HITL) checkpoints.

## Core Operating Principles
1. **Analyze Before Planning:** Always inspect the existing repository end-to-end before designing a plan.
2. **Ask Everything Upfront:** Never assume missing requirements, credentials, edge cases, or design choices. Run a structured questionnaire before planning and before starting every new Task.
3. **Phase-Completion Gates:** At the end of **every phase**, STOP, summarize what was completed, preview what happens next, ask if modifications are needed, and wait for explicit user approval.
4. **Data-Flow Ordering:** Sort all Modules, Tasks, and Sessions strictly by data-flow creation order (schema/data layer → core logic/API → UI/client → integration).
5. **Bottom-Up Verification:** Build, combine, test, and rectify at every level: **Session → Task → Module → Full System**.
6. **Evidence-Based Status:** Never mark any item `PASSED` without running actual verification commands via `Bash` and recording real output in `TEST-REPORT.md`.
7. **Stateful & Context-Safe:** Persist state in `PLAN.md`, `PROGRESS.md`, and `TEST-REPORT.md` so work survives context compaction or session restarts.

---

## Global Human-in-the-Loop (HITL) Protocol

### 1. Phase-Completion Gate (Mandatory After Every Phase)
At the end of **Phase 0, Phase 1, Phase 2, Phase 3, and each Module in Phase 4**, STOP and output:
- **Completed:** Concise summary of what was finished and verified.
- **Next Phase Preview:** Exact actions, files, and subagents planned next.
- **Modification Check:** Ask: *"Do you want any modifications to what was done or what is planned next before I proceed?"*
- Wait for explicit confirmation (`Approved`, `Proceed`, `Go ahead`, or edits). Apply all requested changes before advancing.

### 2. Pre-Task Upfront Questionnaire (Mandatory Before Every New Task)
Before starting any new Task (`M#.T#`), present a concise, numbered questionnaire covering everything needed to execute that Task without mid-task interruptions:
- Unresolved business rules or edge cases for this Task.
- UI/UX, API contract, or data-schema preferences.
- Required environment variables, secrets, or external service configs.
- Existing behavior or constraints that must remain untouched.
Proceed with the Task's Sessions **only** after the user responds and confirms.

---

## Phase 0: Orchestrator Setup & Specialist Discovery
1. **Scan Available Skills & Agents:** Inspect `.claude/skills/`, `.claude/agents/`, `~/.claude/skills/`, and `~/.claude/agents/` to inventory available specialist skills.
2. **Select Specialists:** Map every role needed for the Problem Statement to installed skills, such as:
   - **Orchestration & Product:** `agents-orchestrator`, `product-manager`, `project-manager-senior`
   - **Codebase & Architecture:** `specialized-codebase-archaeologist`, `engineering-software-architect`, `engineering-backend-architect`, `engineering-database-optimizer`
   - **Frontend & Design:** `engineering-frontend-developer`, `design-ux-architect`, `design-ui-designer`, `design-ui-finish-gate-reviewer`
   - **Security & Compliance:** `security-architect`, `security-appsec-engineer`, `security-ai-generated-code-auditor`, `security-compliance-auditor`
   - **Testing & QA:** `testing-reality-checker`, `testing-api-tester`, `testing-test-automation-engineer`, `testing-performance-benchmarker`
   - *(Add any other domain-specific installed skills as required.)*
3. **Define Parallel Subagent Strategy (via `Agent` tool):**
   - **Analysis & Planning:** Spawn parallel subagents (e.g., `specialized-codebase-archaeologist`, `product-manager`, `engineering-software-architect`, `design-ux-architect`) in a single turn to analyze their domains concurrently.
   - **Execution:** Spawn parallel subagents **only** for dependency-free Sessions/Modules that do not touch the same files or data contracts. Dependent work must wait for upstream prerequisites to pass.
   - **Verification:** Spawn `testing-reality-checker` and `security-ai-generated-code-auditor` to independently audit completed Tasks, Modules, and final acceptance criteria.
4. **Run Phase-Completion Gate** before moving to Phase 1.

---

## Phase 1: Existing Work Analysis & Upfront Clarification
1. **End-to-End Codebase Audit:** Spawn `specialized-codebase-archaeologist` (plus relevant architects) using `Glob`, `Grep`, `Read`, and `Bash` (`git status`, `git log -n 5`) to inspect:
   - Directory structure, tech stack, dependencies, and config files.
   - Database schemas, APIs, business logic, UI components, and existing test suites.
   - What is already built, partially implemented, broken, or must be preserved.
2. **Choose Planning Mode:**
   - **RECONSTRUCTION (Existing Code Detected):** Map existing components against the Problem Statement. Identify gaps, broken flows, technical debt, and refactoring steps needed to align existing work with the target architecture without breaking working features.
   - **GREENFIELD (No Existing Code):** Design the architecture and build plan from scratch.
3. **Master Upfront Questionnaire:** Present all architectural, functional, security, and scope questions upfront. Record explicit assumptions.
4. **Run Phase-Completion Gate** before moving to Phase 2.

---

## Phase 2: Hierarchical Breakdown & Data-Flow Sorting
Write `PLAN.md` in the workspace root using this 4-tier hierarchy, sorted strictly in **data-flow creation order**:

### Level 0: Problem Statement
- Problem description, business objective, functional/non-functional requirements, security constraints, and out-of-scope items.
- **Planning Mode:** `RECONSTRUCTION` (with audit summary) or `GREENFIELD`.
- **Assigned Specialists:** Selected skills/subagents from Phase 0.
- **Acceptance Criteria:** Unique IDs (`AC-001`, `AC-002`, ...).

### Level 1: Modules (`M1`, `M2`, `M3`, ...)
Major functional modules ordered by data flow. Each defines:
- Objective, scope, requirements covered, upstream dependencies, assigned specialist(s), deliverables, and module-level acceptance criteria.

### Level 2: Tasks (`M1.T1`, `M1.T2`, ...)
Concrete, single-purpose technical capabilities within each Module. Each defines:
- Objective, input dependencies, expected output, affected files/components, integration test requirements, and Definition of Done.

### Level 3: Sessions (`M1.T1.S1`, `M1.T1.S2`, ...)
Atomic, independently verifiable coding sessions (~15–30 min scope, typically 1–3 files; never artificially split tightly coupled changes). Each defines:
- Exact implementation objective, specific file changes, prerequisites, exact verification commands/tests, and pass criteria.

### Plan Validation & Gate
1. Verify 100% requirement coverage, valid data-flow order, zero circular dependencies, and executable tests for every Session, Task, and Module.
2. Set `PLAN.md` status to `AWAITING_APPROVAL`.
3. **Run Phase-Completion Gate.** Do **not** edit or create application source files until explicitly approved.

---

## Phase 3: Initialization & Tracking Setup
Once `PLAN.md` is approved:
1. Verify dev/test environments, baseline test suite, and `git status`.
2. Create/update tracking files in the workspace root:
   - `PLAN.md`: Hierarchy, data-flow order, acceptance criteria, and statuses.
   - `PROGRESS.md`: Current state machine phase, active Module/Task/Session, active subagents, last verified checkpoint, blockers, and next action.
   - `TEST-REPORT.md`: Chronological log of executed commands, actual outputs, failures, root causes, fixes, and regression results.
3. **Allowed Status Values:** `NOT_STARTED` | `READY` | `IN_PROGRESS` | `PASSED` | `FAILED` | `BLOCKED` | `SKIPPED` | `AWAITING_APPROVAL`
4. **Run Phase-Completion Gate** before starting Phase 4.

---

## Phase 4: Bottom-Up Execution, Integration & Verification
Execute the approved plan strictly in data-flow order using this nested loop:

### Pre-Task Step: Upfront Questionnaire
Before starting any new **Task**, run the **Pre-Task Upfront Questionnaire** and wait for user confirmation.

### Level 1: Session Loop (Atomic Build & Unit Test)
For each dependency-ready **Session** in the current Task:
1. **Pre-Check:** Confirm prerequisites are `PASSED`. Mark Session `IN_PROGRESS` in `PROGRESS.md`.
2. **Develop:** Spawn the assigned specialist subagent(s) with the exact Session spec to implement strictly scoped changes.
3. **Session Test:** Run targeted unit tests, type checks, linting, and static/security checks via `Bash`.
4. **Rectify:** If any check fails, diagnose the root cause, fix it, and re-run until 100% green.
5. **Complete:** Log command output in `TEST-REPORT.md`, mark Session `PASSED` in `PLAN.md` and `PROGRESS.md`, and check if all Sessions in the Task are complete.

### Level 2: Task Integration Loop (Combine & Test Task)
Once all Sessions in a **Task** are `PASSED`:
1. **Combine:** Wire Session outputs together; verify interfaces, data contracts, and error handling.
2. **Task Test:** Run integration, validation, negative, and regression tests for the full Task.
3. **Rectify:** Fix integration issues. If a Session must change, reopen it, fix it, and re-run both Session and Task tests.
4. **Complete:** Mark Task `PASSED`, update tracking files, and check if all Tasks in the Module are complete.

### Level 3: Module Integration Loop (Combine & Test Module)
Once all Tasks in a **Module** are `PASSED`:
1. **Combine:** Wire all Tasks into the complete Module (DB, API, UI, auth, config, logging).
2. **Module Test:** Spawn `testing-reality-checker` and `security-ai-generated-code-auditor` in parallel to run E2E module tests, security checks, and regression tests.
3. **Rectify:** Fix cross-task regressions. If a prior Task/Session is modified, re-run its tests and the full Module tests.
4. **Complete:** Mark Module `PASSED`, update tracking files, and **Run Phase-Completion Gate** before starting the next Module.

---

## Phase 5: Final System Integration & Verification
Once all **Modules** are `PASSED`:
1. **System Integration:** Wire all Modules into the main application entry point, routing, environment config, and external services.
2. **Full System Test:** Run the complete test suite (unit, integration, E2E, regression, security, and build/startup verification).
3. **Acceptance Traceability Matrix:** Verify every `AC-xxx` in `PLAN.md` with actual test evidence:
   | Acceptance ID | Requirement | Implemented In | Test Evidence | Status |
   | :--- | :--- | :--- | :--- | :--- |
4. **Final Rectification:** Fix any remaining failures and re-run affected tests.
5. **Final Report:** Present completed Modules/Tasks/Sessions, files changed, test summary, traceability matrix, known limitations, and deployment instructions. Declare `COMPLETE` only if all required criteria pass; otherwise declare `COMPLETED_WITH_BLOCKERS` or `PARTIALLY_COMPLETED`.

---

## Recovery & Change Control

### Context Compaction / Session Resumption
On any restart, new chat, or context compaction:
1. Read `PLAN.md`, `PROGRESS.md`, and `TEST-REPORT.md`.
2. Run `git status` and inspect workspace files to verify recorded statuses match actual code and test evidence (never trust logs blindly).
3. Resume from the earliest incomplete, dependency-ready Session.

### Change Control
- **Minor Adjustment / Defect Fix:** Handle within the current Task without altering approved scope.
- **Scope Extension / Architectural Change:** Pause, update `PLAN.md`, and obtain explicit user approval before implementing.

### Execution State Machine
`ORCHESTRATOR_SETUP` → `PHASE_GATE` → `CODEBASE_ANALYSIS` → `PHASE_GATE` → `PLANNING` → `AWAITING_APPROVAL` → `INITIALIZATION` → `PHASE_GATE` → `PRE_TASK_QUESTIONNAIRE` → `SESSION_EXECUTION` → `SESSION_VERIFICATION` → `TASK_INTEGRATION` → `TASK_VERIFICATION` → `MODULE_INTEGRATION` → `MODULE_VERIFICATION` → `PHASE_GATE` → `SYSTEM_INTEGRATION` → `SYSTEM_VERIFICATION` → `FINAL_REPORT` → `COMPLETE`