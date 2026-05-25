# 03 — Assembly Line

> Structural law (HACE §7). Execution order is fixed.
> Do not begin step N+1 until step N is committed to Git.
> For the exact Side-Chat prompt for each step, see `prompts/layer-prompts.md`.
> Phase gate = `npm run build` exits zero TypeScript errors inside `/frontend/`.

---

## Phase 1 — Foundation

### Step 1.0 — Vite scaffold (Archon terminal, no Aimergent)

Files created by CLI: `package.json · vite.config.ts · tsconfig.json · tsconfig.node.json ·
index.html · src/main.tsx · src/App.tsx · src/vite-env.d.ts`

```bash
npm create vite@latest frontend -- --template react-ts
cd frontend
npm install react-router-dom
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
npm install -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser \
  eslint-plugin-react-hooks
```

Acceptance: `cd frontend && npm run dev` loads a blank page with no console errors.
Commit: `chore: scaffold vite react-ts foundation`

### Step 1.1 — Environment lockdown

Files touched: `/.gitignore`

Acceptance: `git status` shows no `node_modules/`, no `.env*`, no `dist/`, no OS files.
Commit: `chore: add gitignore`

### Step 1.2 — Custom Vite config + Router entry

Files touched: `tailwind.config.ts · src/main.tsx · src/App.tsx · src/lib/utils.ts ·
eslint.config.js · .prettierrc`

Aimergent Side-Chat: Layer 1 prompt from `prompts/layer-prompts.md`.

Acceptance: `npm run build` zero errors · `npm run lint` zero errors.
Commit: `feat: configure tailwind router eslint`

### Step 1.3 — shadcn/ui init (Archon terminal, no Aimergent)

```bash
cd frontend && npx shadcn@latest init
```

Then replace the generated `:root` block in `src/index.css` with the values from
`docs/02_design_system.md §6`.

Acceptance: `npm run build` zero errors.
Commit: `chore: init shadcn-ui and apply design-system theme`

**Phase 1 gate:** `npm run build` zero errors · `npm run lint` zero errors.

---

## Phase 2 — Config and types

All Small Worlds follow the order in `project-manifest.md §A.2`.
One Side-Chat per Small World. One commit per Small World.

> ⬜ Block on Small Worlds 4–5 until `project-manifest §A.6` data received from the Gärtnermeister.

| Step | Small World            | File                                    |
|------|------------------------|-----------------------------------------|
| 2.1  | SiteContent type       | `src/types/site-content.ts`             |
| 2.2  | Navigation type        | `src/types/navigation.ts`               |
| 2.3  | ServicesRouter config  | `src/config/services-router.ts`         |
| 2.4  | site-content.json      | `src/config/site-content.json` ⬜ blocked |
| 2.5  | navigation.json        | `src/config/navigation.json`  ⬜ blocked |

Aimergent Side-Chat: Layer 2 prompts from `prompts/layer-prompts.md`.

Acceptance per step: `npm run build` zero errors.

**Phase 2 gate:** All five files committed · `npm run build` zero errors.

---

## Phase 3 — Components and pages

One Side-Chat per component. One commit per component. Follow table order — do not skip.

| Step | Component       | File                                        | Blocked until    |
|------|-----------------|---------------------------------------------|------------------|
| 3.1  | Layout          | `src/components/Layout.tsx`                 | Steps 2.1–2.5    |
| 3.2  | HeroSection     | `src/components/HeroSection.tsx`            | Step 2.4         |
| 3.3  | AboutSection    | `src/components/AboutSection.tsx`           | Step 2.4         |
| 3.4  | CalendarSection | `src/components/CalendarSection.tsx`        | Step 2.4         |
| 3.5  | RulesSection    | `src/components/RulesSection.tsx`           | Step 2.4         |
| 3.6  | PlotMapSection  | `src/components/PlotMapSection.tsx`         | Step 2.4 + image |
| 3.7  | ContactSection  | `src/components/ContactSection.tsx`         | Steps 2.3–2.4    |
| 3.8  | Footer          | `src/components/Footer.tsx`                 | Steps 2.1–2.5    |
| 3.9  | HomePage        | `src/pages/HomePage.tsx`                    | Steps 3.1–3.8    |
| 3.10 | ImpressumPage   | `src/pages/ImpressumPage.tsx`               | Step 2.4 + imprint key |
| 3.11 | DatenschutzPage | `src/pages/DatenschutzPage.tsx`             | Step 2.4 + privacy key |
| 3.12 | NotFoundPage    | `src/pages/NotFoundPage.tsx`                | Step 2.4         |

Aimergent Side-Chat: Layer 3 and Layer 4 prompts from `prompts/layer-prompts.md`.

Acceptance per step: `npm run build` zero errors · visual review in `npm run dev`.

**Phase 3 gate:** All routes render · /impressum · /datenschutz · random path → NotFoundPage.

---

## Phase 4 — Deploy

One-time server setup. Archon-executed. No Aimergent needed.
Follow `docs/04_server_setup.md` exactly (phases 4.1 → 4.5).

| Step | Action                          | Doc section     |
|------|---------------------------------|-----------------|
| 4.1  | Generate SSH key pair           | §4.1            |
| 4.2  | Create deploy user on server    | §4.2            |
| 4.3  | Apply nginx SPA config          | §4.3            |
| 4.4  | Issue TLS certificate           | §4.4            |
| 4.5  | Push to main → verify live      | §4.5            |

**Phase 4 gate (project complete):**
`curl https://stadtgaertle.rheinfelden.de/impressum` → HTTP 200.
`curl https://stadtgaertle.rheinfelden.de/nonexistent` → HTTP 200 (React 404 page).
