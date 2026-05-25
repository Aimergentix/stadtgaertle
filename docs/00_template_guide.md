# 00 — Template Extraction Guide

> Document type: Meta-documentation. Not website content. Not structural law.
> Purpose: secure the reusability of this project's generic patterns for future
> Archon–Aimergent pair-programming work. Read this when starting any new HACE project.

---

## The two-layer content model

Every file in this project belongs to one of three categories. Maintaining this separation
from day one means extracting a reusable template later is mechanical, not creative work.

| Category | Portable?      | Definition                                              |
|----------|----------------|---------------------------------------------------------|
| TEMPLATE | Copy as-is     | Framework, rules, workflow patterns. Zero project refs. |
| MIXED    | Copy + adapt   | Structure is portable; values are project-specific.     |
| PROJECT  | Replace entirely | Stadtgärtle-specific content, German copy, real URLs.  |

---

## TEMPLATE files — copy unchanged to any new HACE project

| File                                    | Why portable                                        |
|-----------------------------------------|-----------------------------------------------------|
| `HACE.md`                               | Core doctrine. Model-agnostic. Zero project refs.   |
| `.cursor/rules/hace.mdc`                | Aimergent constraints. Zero project refs.           |
| `docs/01_website_spec.md`               | Stack declaration. Stack is frozen, not site-specific. |
| `docs/03_assembly_line.md`              | Workflow pattern. Replace step names; keep structure. |
| `.github/workflows/deploy-static.yml`  | rsync + SSH pattern. Secret names are already generic. |
| `prompts/layer-prompts.md`              | Prompt templates. Replace @mentions only.           |

---

## MIXED files — copy structure, replace the marked values

| File                       | Keep                          | Replace                               |
|----------------------------|-------------------------------|---------------------------------------|
| `docs/02_design_system.md` | §1 GDPR rule, §2 a11y rule, format | Color palette, shadcn theme vars  |
| `docs/04_server_setup.md`  | All nginx + Certbot config    | Domain name, deploy path              |
| `docs/00_template_guide.md`| This entire document          | Project name in the title only        |
| `project-manifest.md`      | §A structure (headings, table format) | All values — full content rewrite |

---

## PROJECT files — replace entirely for each new client

| File                                          | What to replace         |
|-----------------------------------------------|-------------------------|
| `frontend/src/config/site-content.json`       | All values              |
| `frontend/src/config/navigation.json`         | All nav items           |
| `frontend/src/config/services-router.ts`      | All external URLs       |
| `frontend/src/types/site-content.ts`          | Interface fields        |
| `frontend/src/types/navigation.ts`            | Interface fields (if nav structure changes) |
| `frontend/src/components/*.tsx`               | Keep pattern; replace content keys |

---

## The generic gems — what this project proves works

These patterns are reusable insights, independent of garden websites:

**1. Layer-prompt context-fasting.** One Side-Chat per layer, explicit @mentions only,
eliminates Aimergent drift in any AI-assisted coding project. The layer-prompts.md file
is the template for this pattern.

**2. Type-first Small Worlds.** TypeScript interfaces are written before the components that
read them. The compiler enforces the Archon's intent without the Archon having to check
every field manually. Order: type → JSON → component — never reversed.

**3. SOA config decoupling.** `services-router.ts` as the single source of external URLs,
and `site-content.json` as the single source of visible copy, make any site migratable to
a visual editor without touching component code. This pattern works for any content site.

**4. Legal pages as first-class Small Worlds.** Adding Impressum and Datenschutz to the
component table from day one prevents compliance debt. Works for any European web project.

**5. rsync + native SSH deploy.** Zero third-party GitHub Actions. The pattern in
`deploy-static.yml` is stable, auditable, and works for any static site on any Linux host.

---

## Extraction procedure (run when Stadtgärtle v1 ships)

```bash
git checkout -b template/hace-static-site
# Delete PROJECT files, replace with placeholder schemas
# Replace MIXED file values with "REPLACE_WITH_PROJECT_VALUE" strings
git add -A && git commit -m "chore: extract reusable HACE template"
git tag hace-template-v1
# Optionally: push as a separate public repository
```

The tag `hace-template-v1` becomes the starting point for every future HACE project.
