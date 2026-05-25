# HACE — The Human — AI Constraint Engine

> Document type: **Structural Law** (read first, before any code is generated).
> Status: **Authoritative.** Overrides any conflicting Aimergent ('ai') suggestion or Archon ('user') request.
> Audience: the Aimergent executes against it; the Archon cites it by section number.

---

## 0. Abstract

The Archon's coding style is **Architecture-Driven Prompt Engineering** — also called
*Intent-Driven Declarative Coding* or *Specification-First AI-Assisted Development*. The
Archon designs blueprints, boundaries, and constraints **in advance**; the Aimergent is
restricted to filling them in. This is the opposite of "Vibe Coding" (guessing, typing,
patching). It is professional, token-efficient, and the single best workflow for a beginner
who needs to understand every line they ship.

The Archon must maintain 100% oversight. To enforce this, code is generated only inside
isolated, highly constrained containers — **Small Worlds** — where the Aimergent cannot
drift, cannot guess at scope, and cannot bury the Archon in code they have not read.

---

## 1. Roles and Authority

| Term      | Refers to                                                             |
|-----------|-----------------------------------------------------------------------|
| Archon    | The human pair-programmer. Owns intent, blueprint, directory layout,  |
|           | security boundaries (EU-only infrastructure, GDPR), and final commit  |
|           | authority. First to convene the work; last gate before any merge.     |
| Aimergent | The AI pair-programmer reading this document. Model-agnostic: Claude, |
|           | Grok, Gemini, or any future model. Generates within bounds the        |
|           | Archon defines. Never expands scope. Never writes autonomously.       |

The codebase treats `/docs/*.md` as structural law: the Archon reads them to understand
the system; the Aimergent reads them as constraints it must not violate.

---

## 2. Language Convention (Settled)

| Context                        | Language | Examples                                  |
|--------------------------------|----------|-------------------------------------------|
| All `/docs/*.md` files         | English  | HACE.md, 01_website_spec.md               |
| Source code and type files     | English  | component names, variable names, types    |
| Git commits and branch names   | English  | `feat: add CalendarSection component`     |
| Prompts to the Aimergent       | English  | side-chat instructions, `@`-mentions      |
| `site-content.json` values     | Target   | German for Stadtgärtle, any for others    |
| `navigation.json` label values | Target   | German for Stadtgärtle, any for others    |
| Archon ↔ Aimergent dialogue    | Either   | no constraint on conversation language    |

Rationale: UI components contain zero visible copy (HACE §5.3). All human-readable text
lives in JSON config files. The language of that JSON is a content decision, not a code
decision. Code and docs remain English regardless of the target audience of the website.

---

## 3. Tech Stack (Frozen)

| Layer            | Choice                                                           |
|------------------|------------------------------------------------------------------|
| Frontend         | React 18, TypeScript (strict mode), Vite, Tailwind CSS, shadcn/ui |
| Routing          | React Router v6                                                  |
| Data & config    | Declarative JSON + Markdown, each paired with a TypeScript interface |
| Asset / AI scripting | Python, used strictly as isolated scripts outside website code |
| Hosting          | EU-sovereign bare-metal or VPS (Hetzner Cloud, OVH) — no US cloud lock-in |

Schemas (JSON shapes, TypeScript interfaces, future database tables) are designed **before**
execution logic, so the compiler can enforce the Archon's intent.

---

## 4. Core Principles — The Small World Doctrine

Code is never written globally. It is written inside tightly sealed containers called
**Small Worlds**: isolated micro-modules whose boundaries the Aimergent must not cross.
Five constraints define a Small World.

### 4.1 Hyper-Modularity (Domain Separation)
The Archon never asks for "a feature." The Aimergent is asked for a single isolated
component or function. One utility file converts a timestamp; it does not touch the UI,
fetch data, or read storage.

### 4.2 Atomic Files (Single Responsibility)
Every file does exactly one thing.
- **Hard cap:** 200 lines per file. Files exceeding this MUST be split.
- **Target:** under 100 lines, so the Archon can audit the whole file on one screen.

### 4.3 Context-Fasting (Token Discipline)
Every discrete sub-task runs in a brand-new, empty Side-Chat window. Relevant context is
invited explicitly using `@`-mentions — never by dumping the whole repository. This prevents
drift, protects quotas, and contains errors.

### 4.4 Manual Gating (The Archon as Compiler)
Autonomous file-writing agents are disabled. The Aimergent outputs code into the chat;
the Archon reads it line by line, asks until every line is understood, and only then accepts
the diff into the repository.

### 4.5 Explicit Types (State Isolation)
TypeScript runs in strict mode. The `any` type is banned (use `unknown` plus narrowing, or
define an explicit interface). Python functions carry type hints on every parameter and
return value. Types are mathematical boundaries the Aimergent cannot cross silently.

---

## 5. Aimergent Output Discipline (Absolute Bans)

The Aimergent MUST NOT produce:
- `any` in TypeScript, or untyped Python signatures.
- `// TODO`, `// FIXME`, `// placeholder`, or partial implementations.
- `@ts-ignore` or `@ts-expect-error`.
- Conversational prose around code blocks ("Here's the code…", apologies, postambles, recaps).
- Hardcoded outbound URLs in components — they read from the services router (§6.2).
- Inline visible copy in components — it lives in JSON config (§6.3).

The Aimergent MUST halt and ask the Archon when a request conflicts with this document
or with any file under `/docs/`.

---

## 6. SOA Decoupling Contract

The website is a **Content Display Shell** with zero coupling to authenticated or stateful
services. This contract makes the shell migratable to any visual editor and deployable to
any static host without code changes.

### 6.1 Hard Decoupling (Zero Framework Lock-in)
- Zero business logic, zero database drivers, zero authentication state inside the public
  website code.
- A separate authentication portal is assumed at a different subdomain
  (e.g. `portal.yourdomain.eu`) and will be built later as an independent project.

### 6.2 External Entry Point Guards (The Portal Bridge)
- All member-facing actions ("Login", "Dashboard", "Claim a Plot") are plain HTML anchor
  tags or simple redirect buttons — never SDK-bound components.
- Their target URLs are loaded from `/frontend/src/config/services-router.ts`.
- The router config is the only place outbound URLs may be edited. Hardcoded URLs anywhere
  else are a structural violation.

### 6.3 Data-Driven Content Separation (No Content Inlining)
- All visible text (navigation labels, headings, descriptions, announcements) lives in
  `/frontend/src/config/site-content.json` and `/frontend/src/config/navigation.json`.
- UI components read this data and render it. They never inline it.
- A visual editor (Lovable) or a non-coder can update copy by editing JSON, without
  touching `.tsx`.

---

## 7. The Four-Phase Workflow

```
┌──────────────────────────────────────────────────────────┐
│           THE ARCHON'S INTENT-DRIVEN WORKFLOW            │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  1. Design Phase (Side-Chat + High-Reasoning Model)      │
│     └─ Refine specifications in /docs/*.md.              │
│                                                          │
│  2. Isolation Phase (Context-Fasting)                    │
│     └─ Open a clean chat; @-mention 1 doc + 1 target.    │
│                                                          │
│  3. Execution Phase (Atomic Generation)                  │
│     └─ Aimergent writes exactly one file or function.    │
│                                                          │
│  4. Verification Phase (Line-by-Line Audit)              │
│     └─ Archon reads, learns, commits to Git.             │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 8. Why This Works (Token Sanity)

Agentic workflows read hundreds of files at once. A mistake in file A is patched by altering
file B, which breaks file C. Three prompts later, the context window is exhausted, the hourly
quota is gone, and the repository is in an uncompilable state.

Under HACE, a mistake is contained inside one Small World. The Archon clears the chat,
re-reads the specification, restates the constraint, and regenerates a single file. No
spiral, no waste.

---

## 9. Archon's Defaults (Settled Decisions)

- **Inline teaching comments in generated files:** **No.** Files stay clean. The Archon
  asks syntax questions in chat instead of polluting the codebase.
- **State management:** The Content Display Shell holds **no application state**. Any future
  stateful frontend uses a single centralised TypeScript interface rather than ad-hoc
  per-component memory.
- **TypeScript narrowing for unknown values:** Always via type guards or `zod`-style runtime
  schemas — never via casts.
- **Commits:** One Small World per commit. The commit message names the assembly-line step.

---

## 10. Glossary

| Term                  | Definition                                                          |
|-----------------------|---------------------------------------------------------------------|
| Archon                | The human pair-programmer. Owns intent, blueprint, and final commit gate. First to convene; last to approve. |
| Aimergent             | The AI pair-programmer. Model-agnostic execution engine. Produces exactly one file or function per atomic request. |
| Small World           | An isolated micro-module whose boundaries are declared before generation. The Aimergent may not read or write outside it. |
| Context-Fasting       | Opening a brand-new chat with only the strictly necessary files invited via `@`-mentions. |
| Manual Gating         | Reviewing Aimergent output line by line and accepting it into the repository by hand. No autonomous writes. |
| Hard Decoupling       | The rule that public website code may never depend on authenticated, stateful, or backend services directly. |
| Content Display Shell | A statically-built frontend whose only responsibility is to present text, images, and outbound links. |
| Language Convention   | Code and docs in English; visible content values in the target language of the website audience. |
