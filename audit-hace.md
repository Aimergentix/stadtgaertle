# MISSION: Scaffold an Open-Source Website Environment for a Coding Beginner

Audit and generate the scaffolding for a Vite static website built per the **HACE** Small World
principle. The project is developed locally in Cursor + GitHub, with a **zero-modification**
migration path to visual editors (Lovable, v0, bolt.new) and direct deployment to EU-sovereign
hosting (Hetzner Cloud / OVH).

**Authority:** `HACE.md` is structural law. Where this brief and `HACE.md` disagree, `HACE.md` wins.

**Roles:** The Archon reads this document as an instruction set. The Aimergent reads it as a
constraint set. Both terms are model-agnostic; see `HACE.md §1` for definitions.

---

## STEP 0 — PRE-FLIGHT (Run in a context-fasted Side-Chat)

Before executing, the Aimergent must confirm three things in its first response:

1. `@HACE.md` is in the chat context. If absent: halt and request it from the Archon.
2. Output mode is **Manual Gating** (HACE §4.4): blueprints are rendered in chat as
   reviewable code blocks; nothing is written autonomously to disk.
3. Every generated file obeys: zero `any`, zero `// TODO` / `// FIXME`, zero `@ts-ignore` /
   `@ts-expect-error`, zero conversational prose, hard cap 200 lines (target < 100), one
   responsibility per file.

---

## STEP 1 — WEBSITE MIGRATION AUDIT

As an expert Systems Archon, produce a short assessment (max 2 paragraphs per sub-point) of:

1. **Visual-Editor Readability.** How to structure a React + TypeScript + Vite + Tailwind layout
   so a visual AI design platform can import the repository and render it without modification.
   Cover: standard Vite directory convention, one-component-per-file naming, Tailwind-only
   styling (no CSS-in-JS), React Router v6.

2. **Configuration Isolation.** How to keep text, navigation, image paths, and outbound URLs in
   dedicated asset files so a non-coder can edit content without touching `.tsx`. Cover the
   three mandated config files from `HACE.md §6.2 / §6.3` plus their paired TypeScript types.

---

## STEP 2 — METICULOUS SKELETON GENERATION

Generate the full text of each file below. No placeholders, no ellipses, no `// rest of code here`.

### 2.1 `.cursorrules` (project root)

Enforce HACE as structural law:
- Ban `any` — use `unknown` plus narrowing, or define an explicit interface / type.
- Ban `// TODO`, `// FIXME`, `@ts-ignore`, `@ts-expect-error`, and partial implementations.
- Ban conversational fluff in Aimergent output (no "Here's the code…", no apologies, no postambles).
- Hard cap 200 lines per file; target under 100. Split when exceeded.
- One responsibility per file. One React component per file. File name equals component name.
- Forbid hardcoded outbound URLs — read from `/frontend/src/config/services-router.ts`.
- Forbid inline visible copy — read from `/frontend/src/config/site-content.json`.
- Treat `/docs/*.md` as structural law; halt if a request conflicts with them.
- Roles: the human is the Archon; the AI session is the Aimergent. Both are model-agnostic.
- Language: all code and doc files in English; visible content values in the website's target language.

### 2.2 `docs/01_website_spec.md`

Define the structural boundaries:
- Fixed tech stack: Vite, React 18, TypeScript strict, Tailwind CSS, shadcn/ui, React Router v6.
- Frozen directory tree rooted at `/frontend/` with `public/` and `src/{components,pages,config,types}`.
- Asset rules: images in `public/images/` (referenced via absolute paths, never imports);
  copy in `site-content.json`; menus in `navigation.json`; outbound URLs in `services-router.ts`;
  paired interfaces in `src/types/`.
- SOA Decoupling Contract (HACE §6.1): zero backend logic, zero auth state, zero database
  drivers. All member actions are plain `<a>` redirects through `services-router.ts`.
- Explicit non-goals: SSR, API routes, database access, authentication, sessions.
- Language convention (HACE §2): all source files and docs in English; JSON content values
  in the target language of the website audience.

### 2.3 `docs/03_assembly_line.md`

A beginner workflow checklist. **Execution order is fixed; do not start step N+1 until N is
committed.** Each step lists files touched, acceptance criteria, and the exact side-chat prompt.

- **Step 1.1 — Environment Lockdown.** `/.gitignore` covering Node, Vite, TypeScript artefacts,
  OS files, IDE files, and all `.env*` variants.
- **Step 1.2 — Content + Types.** `/frontend/src/config/site-content.json` and
  `/frontend/src/types/site-content.ts` (interface matching the JSON shape exactly).
- **Step 1.3 — Navigation + Services Router.** `/frontend/src/config/navigation.json`,
  `/frontend/src/types/navigation.ts`, and `/frontend/src/config/services-router.ts`
  (typed object exporting every outbound URL).
- **Step 1.4 — Base Layout Wrapper.** `/frontend/src/components/Layout.tsx` reads brand text
  from `site-content.json` and menu from `navigation.json`. Under 100 lines. Zero `any`.

Phase 1 gate: `npm run build` inside `/frontend/` exits with zero TypeScript errors before commit.

### 2.4 `.github/workflows/deploy-static.yml`

A clean GitHub Actions workflow with **no third-party action dependencies**:
- Triggers: push to `main` and `workflow_dispatch`.
- Use only `actions/checkout@v4` and `actions/setup-node@v4` (Node 20, npm cache pointed at
  `frontend/package-lock.json`).
- Working directory `./frontend`. Run `npm ci`, then `npx tsc --noEmit` as a type-check gate,
  then `npm run build`.
- Deploy `./frontend/dist/` via native `rsync` over `ssh` — no `appleboy/*`, no
  `SamKirkland/*`, no other community actions.
- Use an ed25519 SSH key from secrets. Populate `known_hosts` via `ssh-keyscan`. Remove the
  key in a final `if: always()` step.
- Document the five required GitHub Secrets: `DEPLOY_SSH_PRIVATE_KEY`, `DEPLOY_SSH_HOST`,
  `DEPLOY_SSH_PORT`, `DEPLOY_SSH_USER`, `DEPLOY_SSH_PATH`.

---

## STEP 3 — NEXT ATOMIC INSTRUCTION

Conclude with the exact prompt the Archon should paste into a brand-new, context-fasted
Cursor Side-Chat to begin **Step 1.1** of the assembly line. The prompt must:
- Include the precise `@`-mentions: `@docs/01_website_spec.md` and `@docs/03_assembly_line.md`.
- Reference HACE roles: address the Aimergent, confirm the Archon's Manual Gating mode.
- End with "Output only the file content. No commentary. No code fences."

---

## ARCHON'S ACCEPTANCE CRITERIA (for the audit run itself)

The audit run is accepted if all of the following hold:
- All four blueprints are output in full, copy-paste ready, each under 200 lines.
- `.cursorrules` covers every ban listed in §2.1 plus the SOA decoupling rule and the
  language convention.
- The assembly line orders dependencies correctly: content + types **before** the components
  that read them.
- The workflow uses zero third-party actions and lists every required secret.
- No file references libraries or services beyond the fixed stack declared in §2.2.
- Roles throughout use Archon / Aimergent — no occurrence of "user", "human", "AI", or
  any model product name.
