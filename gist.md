<!-- HACE — GitHub Gist -->
<!-- ═══════════════════════════════════════════════════════════════════════════ -->

<div align="center">

<!-- Banner: place a wide architectural/blueprint-style image here -->
<!-- Suggested: dark background, grid lines, compass rose, architectural detail -->
![HACE Banner](https://raw.githubusercontent.com/Aimergentix/stadtgaertle/main/docs/images/hace-banner.png)

<br/>

```
╔══════════════════════════════════════════════════════════╗
║        H  A  C  E                                        ║
║        Human — AI Constraint Engine                      ║
║                                                          ║
║        Observe first.                                    ║
║        Design before you act.                            ║
║        Work in small, contained plots.                   ║
╚══════════════════════════════════════════════════════════╝
```

**A structured AI-human pair-programming methodology**<br/> 
*above and beyond the slop*<br/>
*for builders who understand every line of code they ship.*<br/>

![methodology](https://img.shields.io/badge/methodology-Architecture--Driven-1a1a2e?style=flat-square&labelColor=#003B5C)
![stack](https://img.shields.io/badge/stack-React%20%C2%B7%20TypeScript%20%C2%B7%20Vite-4a9eff?style=flat-square&labelColor=#003B5C)
![hosting](https://img.shields.io/badge/hosting-EU--sovereign-2ecc71?style=flat-square&labelColor=#003B5C)
![any](https://img.shields.io/badge/any%20type-%E2%9C%97%20banned-e74c3c?style=flat-square&labelColor=#003B5C)
![status](https://img.shields.io/badge/status-Structural%20Law-f39c12?style=flat-square&labelColor=#003B5C)

</div>

---

## What is HACE?

HACE is not a library. It is not a boilerplate. It is not a prompt template.

HACE is a **discipline** — a pair-programming contract between a human architect (the *Archon*[^1]) and an AI execution engine (the *Aimergent*[^2]). The Archon designs blueprints, boundaries, and constraints **before a single line of code is written**. The Aimergent fills them in, one atomic file at a time, and may not exceed those boundaries.

> **This is the opposite of Vibe Coding** — guessing, typing, patching, hoping.  
> HACE is professional, token-efficient, and the single best workflow for a beginner  
> who needs to understand every line they ship.

### The Implementation

HACE is the methodology; **Stadtgaertle** ("little urban garden") is the living implementation.

If you want to explore the corresponding repository, study the architecture, or view the current blueprints, **[click here to visit the repository](https://github.com/Aimergentix/stadtgaertle)**.

*This project is licensed under the MIT License. See [the official MIT License text](https://opensource.org/licenses/MIT) for full details.*
### Terminology Notes

To ensure structural integrity and prevent context-window contamination, we use specific role-based designators throughout this documentation and codebase. 

These terms are chosen as unambiguous namespaces. By avoiding standard industry terms (like "user" or "agent"), we eliminate collision with reserved language keywords, variable names, or common function identifiers, providing clear syntactic anchors that keep design directives distinct from technical implementation.
We use these terms to establish a Master-Tool hierarchy. The Archon provides the vision, and the Aimergent provides the execution, ensuring that the human remains in control and fully understands the code.

[^1]: **Archon**: The human architect. An **unambiguous namespace** designator used to prevent collision with reserved keywords and to signify sole structural authority. Etymology: Derived from the Greek arkhōn, meaning "ruler" or "chief magistrate."

[^2]: **Aimergent**: The AI execution engine. An **unambiguous namespace** designator used to ensure semantic separation between design intent and implementation logic. Etymology: A portmanteau of "AI" and "Emergent" (with a nod to "Agent").

---

## The Two Roles

```
┌─────────────────────────────────┐   ┌─────────────────────────────────┐
│           ARCHON                │   │          AIMERGENT              │
│         (the human)             │   │           (the AI)              │
├─────────────────────────────────┤   ├─────────────────────────────────┤
│  ✦ Owns intent & blueprint      │   │  ✦ Executes within bounds       │
│  ✦ Designs schemas first        │   │  ✦ Generates one file/function  │
│  ✦ Holds final commit gate      │   │  ✦ Never expands scope          │
│  ✦ Reads every line before      │   │  ✦ Halts & asks when in doubt   │
│    merging                      │   │  ✦ Model-agnostic: Claude, Grok,│
│  ✦ EU infra · GDPR sovereign    │   │    Gemini, or any successor     │
└─────────────────────────────────┘   └─────────────────────────────────┘
         First to convene.                    First to be bounded.
         Last gate before merge.              Last to write autonomously.
```

---

## The Small World Doctrine

Every piece of code lives inside a **Small World** — a tightly sealed micro-module whose  
boundaries are declared by the Archon *before* the Aimergent writes a single character.

```
 ╭─────────────────────────────────────────────────────────╮
 │  SMALL WORLD CONSTRAINTS                                │
 │                                                         │
 │  4.1  Hyper-Modularity    one isolated component/fn     │
 │  4.2  Atomic Files        one responsibility · ≤200 ln  │
 │  4.3  Context-Fasting     fresh chat · only @-mentions  │
 │  4.4  Manual Gating       Archon reads, then commits    │
 │  4.5  Explicit Types      strict TS · any is banned     │
 ╰─────────────────────────────────────────────────────────╯
```

A mistake inside a Small World is contained inside that Small World.  
Clear the chat. Re-read the spec. Regenerate one file. No spiral. No waste.

---

## The Four-Phase Workflow

```
┌──────────────────────────────────────────────────────────┐
│           THE ARCHON'S INTENT-DRIVEN WORKFLOW            │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  (1) DESIGN       Side-Chat + High-Reasoning Model       │
│                 Refine specifications in /docs/*.md      │
│                                                          │
│  (2) ISOLATE      Context-Fasting                        │
│                 Open a clean chat · @-mention 1 doc      │
│                 + 1 target · nothing else                │
│                                                          │
│  (3) EXECUTE      Atomic Generation                      │
│                 Aimergent writes exactly one file        │
│                 or one function · nothing more           │
│                                                          │
│  (4) VERIFY       Line-by-Line Audit                     │
│                 Archon reads · learns · asks · commits   │
│                                                          │
└──────────────────────────────────────────────────────────┘
            One Small World per commit.
            The commit message names the step.
```

---

## Architecture Principles at a Glance

### The Content Display Shell

The public website is a **stateless shell** with a single responsibility: display text, images, and outbound links. It has zero coupling to authentication, databases, or business logic.

| Layer | Rule |
|---|---|
| Visible text | Lives in `site-content.json` only — never inlined in components |
| Outbound URLs | Live in `services-router.ts` only — never hardcoded elsewhere |
| Member actions | Plain `<a>` tags pointing to a separate `portal.yourdomain.eu` |
| Application state | None — the shell holds zero runtime state |

**What this means in practice:** A non-developer can update the entire site by editing two JSON files. No `.tsx` files are touched. No build pipeline knowledge is required.

### The Language Convention

| Context | Language |
|---|---|
| `/docs/*.md` documentation | English |
| Source code, variable names, types | English |
| Git commits and branch names | English |
| Visible content in JSON files | Target audience language (e.g. German) |
| Archon ↔ Aimergent conversation | Either — no constraint |

### The Tech Stack (Frozen)

```
Frontend  →  React 18 · TypeScript strict · Vite · Tailwind CSS · shadcn/ui
Routing   →  React Router v6
Data      →  Declarative JSON + Markdown, each paired with a TS interface
Scripting →  Python (isolated asset/AI scripts — never inside website code)
Hosting   →  EU-sovereign bare-metal / VPS (Hetzner, OVH) — no US cloud lock-in
```

---

## Absolute Bans (Aimergent Output Discipline)

The Aimergent **must not** produce any of the following — ever, under any framing:

```
✗  any  in TypeScript, or untyped Python signatures
✗  // TODO  ·  // FIXME  ·  // placeholder  ·  partial implementations
✗  @ts-ignore  or  @ts-expect-error
✗  Conversational prose around code blocks ("Here's the code…")
✗  Hardcoded outbound URLs inside components
✗  Inline visible copy inside components
```

The Aimergent **must halt and ask the Archon** whenever a request conflicts  
with this document or any file under `/docs/`.

---

## Why It Works (Token Sanity)

Agentic workflows collapse like this:

```
mistake in file A
  → patch in file B
    → break in file C
      → context window exhausted
        → quota gone
          → repository uncompilable
```

HACE workflows contain mistakes like this:

```
mistake in file A
  → clear the chat
    → re-read the specification
      → restate the constraint
        → regenerate one file
          → done
```

No spiral. No wasted quota. No code the Archon hasn't read.

---

## Walkthrough

> **Note:** GitHub Gists don't support inline video playback.  
> Click the thumbnail below to watch the walkthrough on your video platform of choice.

<!-- Video thumbnail — link the image to your video URL -->
<!-- Place a 16:9 screenshot of the video's opening frame in docs/images/          -->
<!-- and the video file itself in docs/videos/ (or link to YouTube / Vimeo)        -->

[![HACE Walkthrough — click to watch](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/docs/images/hace-walkthrough-thumb.png)](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/docs/videos/hace-walkthrough.mp4)

*A short screencast walking through one full four-phase cycle, from blank spec to committed file.*

---

## Repository Map

```
YOUR_REPO/
├── docs/
│   ├── HACE.md                  ← this document (structural law)
│   ├── 01_website_spec.md
│   ├── images/
│   │   ├── hace-banner.png      ← wide banner (1400×400 recommended)
│   │   └── hace-walkthrough-thumb.png
│   └── videos/
│       └── hace-walkthrough.mp4
└── frontend/
    └── src/
        ├── config/
        │   ├── site-content.json     ← all visible text lives here
        │   ├── navigation.json       ← all nav labels live here
        │   └── services-router.ts    ← all outbound URLs live here
        └── components/              ← zero hardcoded copy, zero hardcoded URLs
```

---

## Adopting HACE

1. **Read HACE.md top to bottom.** Every section is load-bearing.  
2. **Open `/docs/` and write your spec first.** No code before the blueprint.  
3. **Open a fresh chat per task.** Invite only the files you need via `@`-mention.  
4. **Ask until you understand every line.** The Aimergent teaches in chat, not in comments.  
5. **Commit one Small World at a time.** The diff should be readable in one sitting.

---

<div align="center">

```
The Archon designs the garden.
The Aimergent plants exactly where told.
Neither digs outside the plot.
```

*HACE is a structural law, not a suggestion.*  
*The Archon reads it first. The Aimergent executes against it.*  
*Everything else follows.*

<br/>

![made with HACE](https://img.shields.io/badge/made%20with-HACE-1a1a2e?style=for-the-badge&labelColor=0f0f1a)
     
</div>