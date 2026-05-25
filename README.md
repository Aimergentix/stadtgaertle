<div align="center">

<br>

```
    ┌─────────────────────────────────────────┐
    │   🌱  S T A D T G Ä R T L E             │
    │       R H E I N F E L D E N             │
    └─────────────────────────────────────────┘
```

**Urban community gardening — rooted in Rheinfelden, grown online.**

*A static website built to free a garden master from his inbox.*

<br>

[![React 18](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=white)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-latest-000000?style=flat-square)](https://ui.shadcn.com)
[![MIT License](https://img.shields.io/badge/License-MIT-4A7C59?style=flat-square)](LICENSE)
[![Built with HACE](https://img.shields.io/badge/Built%20with-HACE-5C4033?style=flat-square)](HACE.md)

<br>

</div>

---

## The garden

Since 2015, the **Stadtgärtle** has been growing quietly in the Karl-Metzgergrube —
a former gravel pit, reclaimed, replanted, and handed to the community of Rheinfelden,
Baden. Almost 50 people, aged 3 to 83, tend raised beds built above contaminated soil,
practice permaculture together, and gather every autumn for the *Kürbissuppenfest*.

At the centre of it: **The Gärtnermeister**, permaculture designer,
and the living memory of everything the garden knows. Every question about rules,
schedules, and plots lands with him. In person. Repeatedly.

This website exists to change that. Not to replace the Gärtnermeister — to free him.

---

## What the site does

```
Stadtgärtle website
│
├── Tells visitors what the garden is and who it is for
├── Publishes the seasonal calendar so nobody has to ask
├── Lists the rules (including why raised beds are mandatory — it is a good story)
├── Shows the plot map so members can find their bed
└── Provides contact details without the Gärtnermeister having to hand them out
```

No database. No login. No server logic. A static shell, deployed to EU-sovereign
infrastructure, readable by anyone with a browser.

---

## How it was built — the HACE method

> *"Permaculture is the design of sustainable human settlements by following nature's patterns. 
> You observe first. You design before you act. You work in small, contained plots."*

> *Methodology: This codebase was built using **HACE** (the Human — AI Constraint Engine) — a structured AI-human pair-programming methodology that applies the same philosophy to software.
> You can view the full structural law here: [Human — AI Constraint Engine](https://gist.github.com/Aimergentix/16eee019135759fb982893df1fbf21f1)*


| Permaculture principle | HACE equivalent |
|------------------------|-----------------|
| Observe before you act | Design Phase: spec documents written before any code |
| Small, contained plots | Small Worlds: one file per isolated chat session |
| No waste | Context-Fasting: fresh chat per task, zero token drift |
| Design self-sustaining systems | SOA decoupling: zero backend coupling, portable to any host |
| The gardener tends; nature grows | Manual Gating: Archon reviews every line; Aimergent generates |

The human (the **Archon**[^1]) designs the blueprint and holds the commit gate.
The AI (the **Aimergent**[^2]) fills in the structure, one file at a time, within strict
constraints the Archon defines in advance.

The result: a beginner who understands every line of code they ship.

Full methodology: [`HACE.md`](HACE.md)

### Terminology Notes

To ensure structural integrity and prevent context-window contamination, we use specific role-based designators throughout this documentation and codebase. 

These terms are chosen as unambiguous namespaces. By avoiding standard industry terms (like "user" or "agent"), we eliminate collision with reserved language keywords, variable names, or common function identifiers, providing clear syntactic anchors that keep documentation and design directives distinct from technical implementation. 

We use these terms to establish a Master-Tool hierarchy. The Archon provides the vision, and the Aimergent provides the execution, ensuring that the human remains in control and fully understands every line of code.


[^1]: **Archon**: The human architect. An **unambiguous namespace** designator used to prevent collision with reserved keywords and to signify sole structural authority. Etymology: Derived from the Greek arkhōn, meaning "ruler" or "chief magistrate."

[^2]: **Aimergent**: The AI execution engine. An **unambiguous namespace** designator used to ensure semantic separation between design intent and implementation logic. Etymology: A portmanteau of "AI" and "Emergent" (with a nod to "Agent").

---

## Tech stack

| Layer | Choice | Reason |
|-------|--------|--------|
| Bundler | Vite 5 | Fast dev server, clean static output |
| UI | React 18 + TypeScript strict | Component model, compiler-enforced contracts |
| Styling | Tailwind CSS 3 + shadcn/ui | Utility-first, zero CSS-in-JS, visual-editor ready |
| Routing | React Router 6 | Standard SPA routing with history API |
| Hosting | Hetzner Cloud / OVH | EU-sovereign, no US cloud lock-in |
| Deploy | GitHub Actions + rsync over SSH | Zero third-party action dependencies |

---

## Project structure

```
frontend/
├── public/images/          # static assets — referenced by path, never imported
└── src/
    ├── components/         # one component per file, name = filename
    ├── pages/              # one page per route
    ├── config/             # site-content.json · navigation.json · services-router.ts
    └── types/              # TypeScript interfaces paired to each config file
```

All visible text lives in `site-content.json`.
All outbound URLs live in `services-router.ts`.
Components contain zero hardcoded copy and zero hardcoded URLs — ever.
This means a non-developer can update the entire site by editing two JSON files.

---

## Screenshots

> Coming in Phase 3 — add a screenshot of the live site here once deployed.

---

## Getting started

This project uses the HACE pair-programming workflow.
If you are the Archon (the human owner), start here:

**[`docs/ARCHON_START_HERE.md`](docs/ARCHON_START_HERE.md)**

If you want to contribute or understand the code standards, read:

**[`CONTRIBUTING.md`](CONTRIBUTING.md)**

---

## The dioxin story (why raised beds are not optional)

The soils beneath Rheinfelden's inner city carry traces of polychlorinated dioxins —
a legacy of the region's industrial past. The Karl-Metzgergrube was backfilled between
2010 and 2018 with lightly contaminated excavation material. German federal soil
protection law (BBodSchG) and the recommendations of the Landkreis Lörrach are clear:
no root vegetables grown directly in the ground, no tilling of the natural soil.

The raised beds filled with clean earth are not an aesthetic choice. They are a
necessary protection measure — and one of the most quietly radical things about this
garden.

The website explains this. the Gärtnermeister used to.

---

## License

[MIT](LICENSE) · Built with care in Baden-Württemberg · 2026

<div align="center">
<br>
<sub>
  🌱 &nbsp; <em>Grown one small world at a time.</em>
</sub>
<br><br>
</div>
