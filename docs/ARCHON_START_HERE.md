# Archon's Field Manual — Starting AI-Pair Programming in Cursor

> **Who this is for:** You — the Archon. The human who owns this project.
> **What this is:** A step-by-step walkthrough of everything you do from a cold start
> to your first working Aimergent session in Cursor.
> **When to read it:** Once carefully before your first session. Then keep it open
> as a reference in a second window while you work.
> **Language:** Plain English. Every technical term is explained the first time it appears.

---

## PART A — One-time setup (do this before anything else)

These steps happen once on your laptop. You never repeat them.

### A.1 — Install Node.js

Node.js is the engine that runs the JavaScript tools (Vite, TypeScript, npm) that build
your website. Think of it as the machinery behind the scenes.

1. Open your browser and go to **https://nodejs.org**
2. Download the **LTS version** (the button labelled "LTS" — Long Term Support). As of 2026
   this is Node 20 or 22. Either is fine.
3. Run the installer. Accept all defaults. Click through to Finish.
4. To verify it worked: open a terminal window (see A.2) and type:
   ```
   node --version
   ```
   You should see something like `v20.11.0`. Any version above 18 is fine.
   Then type:
   ```
   npm --version
   ```
   You should see something like `10.2.4`. If both commands show version numbers, Node.js
   is correctly installed. You will never need to touch it again.

### A.2 — Know how to open a terminal

A terminal is a text window where you type commands. In Cursor (and VS Code, which Cursor
is built on) the terminal is built in. You do not need a separate terminal application.

To open the terminal inside Cursor:
- **Mac:** Press `Ctrl` + `` ` `` (the backtick key, top-left of keyboard, under Escape)
- **Windows/Linux:** Same — `Ctrl` + `` ` ``
- Or go to the menu: **View → Terminal**

A panel appears at the bottom of the Cursor window showing a command prompt. This is where
you type npm commands, git commands, and nothing else.

### A.3 — Install Cursor (if not yet done)

Download from **https://cursor.com**. Install like any application. Open it.
You will need to create a Cursor account and choose a plan. The free tier is sufficient
to start. Log in.

---

## PART B — Opening the project in Cursor

### B.1 — Open the project folder

1. In Cursor: go to **File → Open Folder**
2. Navigate to the folder that contains `HACE.md` and the `docs/` subfolder.
   Select that folder (the project root). Click **Open**.
3. Cursor may ask "Do you trust the authors of the files in this folder?" — click **Yes, I
   trust the authors**. (You wrote these files. Trust is required for the rules to activate.)

### B.2 — What you should see in the left sidebar

The left sidebar is called the **Explorer**. It shows your project's files and folders.
After opening the project you should see (at minimum):

```
📁 .cursor/
   📁 rules/
      📄 hace.mdc          ← the Aimergent's constraint file
📁 docs/
   📄 00_template_guide.md
   📄 01_website_spec.md
   📄 02_design_system.md
   📄 03_assembly_line.md
   📄 04_server_setup.md
📁 prompts/
   📄 layer-prompts.md
📄 HACE.md
📄 project-manifest.md
📄 audit-hace.md
```

If you see these files, you are in the right folder. If the Explorer is hidden, press
`Ctrl+Shift+E` (Mac: `Cmd+Shift+E`) to show it.

### B.3 — Verify the HACE rules are active in Cursor

The file `.cursor/rules/hace.mdc` tells the Aimergent (the AI in the chat) how to behave.
Cursor loads this automatically when you open the folder. To confirm it is loaded:

1. Open Cursor Settings: go to **Cursor → Settings → Cursor Settings** (not VS Code settings)
   or press `Ctrl+Shift+J` (Mac: `Cmd+Shift+J`)
2. Look for a section called **Rules** or **Project Rules**
3. You should see `hace.mdc` listed there as an active project rule

If it does not appear, close and reopen the folder. The file must be at exactly
`.cursor/rules/hace.mdc` — not in the root, not in `docs/`.

---

## PART C — Understanding the two tools you use in every session

You will use exactly two things in Cursor throughout this project:

**Tool 1: The Chat Panel (where you talk to the Aimergent)**
**Tool 2: The integrated Terminal (where you run commands)**

Everything else — writing files, reading files — happens through the Explorer sidebar.

### C.1 — Opening a Side-Chat (the most important habit)

The HACE doctrine requires every task to run in a **fresh, empty chat**. A fresh chat has
no memory of previous conversations, which prevents the Aimergent from drifting or making
assumptions based on old context.

To open a new, empty chat:
1. Press `Ctrl+L` (Mac: `Cmd+L`) — this opens the chat panel on the right side of Cursor
2. Look for a **"New Chat"** button or icon at the top of the chat panel (it looks like a
   pencil or a `+` icon). Click it.
3. The chat input field clears. You now have a context-free chat — the Aimergent remembers
   nothing from before.

> ⚠️ **Never continue an old chat for a new task.** Always open a new chat. This is
> the single most important HACE habit. Old context causes drift and errors.

### C.2 — How to @mention a file in the chat

When you paste a prompt from `prompts/layer-prompts.md`, you will see lines like:
```
@docs/01_website_spec.md @docs/02_design_system.md @HACE.md
```

These `@mentions` invite specific files into the chat context. The Aimergent can then read
those files and obey their constraints. Here is how to use them:

1. In the chat input field, type `@`
2. A file picker dropdown appears — start typing the file name (e.g. `HACE`)
3. Click the file you want from the list
4. It appears as a tag in the input field: `@HACE.md`
5. Repeat for each file you need

> ✅ **Rule:** Only @mention the files listed in the layer prompt. Do not invite extra files.
> More context is not better — it causes the Aimergent to drift outside the Small World.

### C.3 — How to use a prompt from layer-prompts.md

1. Open `prompts/layer-prompts.md` by clicking it in the Explorer sidebar
2. Find the layer section you are working on (e.g. "Layer 1 — Vite foundation")
3. Find the block of text marked **"Side-Chat prompt"**
4. Select all the text inside that block and copy it (`Ctrl+C` / `Cmd+C`)
5. Open a new Side-Chat (see C.1)
6. First type or paste the `@mention` lines at the top of the prompt into the chat
   (use the `@` method from C.2 for each file)
7. Then paste the rest of the prompt text
8. Press `Enter` to send

The Aimergent will respond with one or more code blocks — file contents it has generated.

### C.4 — Manual Gating: how to receive the Aimergent's output

The Aimergent outputs code into the chat. It does **not** write files to your disk.
You do the writing. This is called **Manual Gating** — you are the compiler.

For each file the Aimergent generates, do this:

1. **Read the code in the chat.** Line by line. If you do not understand a line, ask in the
   same chat: *"What does line 12 do?"* Get an answer before proceeding.
2. **Only when you understand and accept it:** Create the file manually.
   - In the Explorer sidebar, right-click the correct folder → **New File**
   - Type the exact file name the Aimergent specified
3. **Paste the code** from the chat into the new file (`Ctrl+V` / `Cmd+V`)
4. **Save** the file (`Ctrl+S` / `Cmd+S`)

> ⚠️ **Never use the Aimergent's "Apply" or "Insert" button to automatically write code
> to a file without reading it first.** Read first. Apply only after you understand.

---

## PART D — Layer 0: Reading the docs (your first session)

Layer 0 has no coding. It is entirely reading. Estimate: 60–90 minutes.

Open each document by clicking it in the Explorer. Read it fully. After each one, ask
yourself: "Do I have any questions about this?" If yes, open a new Side-Chat, @mention
just that document, and ask your question. Get the answer. Then close the chat.

**Reading order:**

| # | File | What it answers |
|---|------|-----------------|
| 1 | `HACE.md` | What this whole workflow is and why it works |
| 2 | `project-manifest.md` | What the Stadtgärtle website actually is and contains |
| 3 | `docs/01_website_spec.md` | What tech we use and why (frozen — no changes) |
| 4 | `docs/02_design_system.md` | What the site looks like (colors, fonts, layout) |
| 5 | `docs/03_assembly_line.md` | The complete step-by-step build plan |
| 6 | `docs/04_server_setup.md` | How the site gets deployed live (read now, execute later) |
| 7 | `docs/00_template_guide.md` | How this project becomes a reusable template later |
| 8 | `prompts/layer-prompts.md` | The exact prompts you will paste into Side-Chats |

**Layer 0 gate:** You have read all eight documents. You can answer these four questions:
- What does SOA decoupling mean for this project?
- Where is the only place an outbound URL may be defined?
- What is a Small World?
- Why must every chat session be fresh and empty?

When you can answer all four, Layer 0 is complete. Make your first Git commit:

In the terminal (see A.2), navigate to the project root and type:
```bash
git add -A
git commit -m "docs: complete layer-0 documentation"
```

---

## PART E — The repeating rhythm (every layer from 1 onward)

Every layer follows the same rhythm. Learn it once, repeat it forever.

```
1. READ the layer section in docs/03_assembly_line.md
         ↓
2. READ the matching layer section in prompts/layer-prompts.md
         ↓
3. If the layer has Archon terminal steps → open terminal, run them first
         ↓
4. Open a NEW Side-Chat
         ↓
5. @mention the files listed in the prompt (type @ → pick file → repeat)
         ↓
6. Paste the prompt text → press Enter
         ↓
7. READ the Aimergent's output LINE BY LINE
   Ask questions in the same chat until you understand every line
         ↓
8. CREATE the file in Explorer → PASTE the code → SAVE
         ↓
9. Run the acceptance check (usually: open terminal → type npm run build)
   If errors appear → paste the error into the SAME chat → ask the Aimergent to fix
         ↓
10. When build passes: commit to Git
    git add -A
    git commit -m "feat: [step name from assembly line]"
         ↓
11. Close the chat. Open a new one for the next Small World.
```

---

## PART F — Starting Layer 1 (your first real coding session)

> **Before you start Layer 1, confirm:**
> - [ ] Node.js is installed (`node --version` shows a version number)
> - [ ] Layer 0 is complete and committed
> - [ ] You have 60–90 minutes of uninterrupted time

**Step 1:** Open the terminal inside Cursor (`Ctrl` + `` ` ``)

**Step 2:** Check you are in the project root. The terminal prompt shows the folder path.
If it shows the wrong folder, type `cd path/to/your/project` to navigate there.

**Step 3:** Run the scaffold commands from `prompts/layer-prompts.md → Layer 1`.
Copy each command block one at a time, paste into the terminal, press Enter.
Wait for each command to finish (you see the prompt return) before pasting the next one.

**Step 4:** After the npm commands finish, open a **new Side-Chat**.

**Step 5:** Follow the Layer 1 prompt from `prompts/layer-prompts.md` exactly.

**Step 6:** When the Aimergent responds, read the output. For each file:
- Does the file contain `any`? → Send back: *"This file contains `any` on line X. Remove it."*
- Does it contain `// TODO`? → Send back: *"This file contains a TODO comment. Remove it."*
- Does it inline visible German text? → Send back: *"This component inlines copy. It must read from site-content.json."*
These are your Manual Gating checks. Run them every time.

**Step 7:** Create each file in the Explorer. Paste. Save.

**Step 8:** In the terminal, type:
```bash
cd frontend
npm run build
```
You should see output ending in something like `✓ built in 1.23s` with no errors.
If you see red error text, paste the entire error block back into the chat and ask for a fix.

**Step 9:** When the build passes, commit:
```bash
git add -A
git commit -m "feat: scaffold layer-1-foundation"
```

Layer 1 is complete. Close the chat. The next session starts at Layer 2.

---

## PART G — What to do when things go wrong

**The Aimergent wrote something you do not understand.**
→ Ask in the same chat: *"Explain line 14 to me in plain language."*
→ Do not accept code you have not understood. This is the whole point of Manual Gating.

**The Aimergent ignored a rule (used `any`, wrote a TODO, etc.)**
→ Correct it in the same chat: *"You used `any` on line 8. HACE §4.5 bans this.
Replace it with an explicit interface."*
→ Do not accept the file until it is clean.

**`npm run build` shows errors.**
→ Copy the full error text from the terminal.
→ Paste it into the current chat and ask: *"Fix this TypeScript error."*
→ Read the fix. Apply it. Run the build again.

**You are not sure which chat to use or whether to open a new one.**
→ Default: always open a new chat. You lose nothing by starting fresh.
→ The only time you stay in the same chat is when you are fixing an error in code
the Aimergent just generated in that same session.

**You made changes you want to undo.**
→ In the terminal: `git status` shows what changed. `git checkout -- filename` reverts one file.
→ `git stash` temporarily sets aside all uncommitted changes.
→ Never panic. Git keeps a full history of everything you committed.

**The Aimergent starts talking about things outside the current task.**
→ Stop it: *"You are outside the current Small World. Stay within the scope of
[current file name]. Do not reference or modify any other file."*

---

## Quick-reference card (keep this visible while working)

| Action | How |
|--------|-----|
| Open Explorer (files) | `Ctrl+Shift+E` / `Cmd+Shift+E` |
| Open terminal | `Ctrl+`` ` ``​` / `Cmd+`` ` ``​` |
| New Side-Chat | `Ctrl+L` / `Cmd+L` → click New Chat |
| @mention a file | Type `@` in chat → pick from dropdown |
| Save a file | `Ctrl+S` / `Cmd+S` |
| Run build check | Terminal → `cd frontend` → `npm run build` |
| Commit changes | Terminal → `git add -A` → `git commit -m "message"` |
| Undo all uncommitted changes | Terminal → `git checkout -- .` |
| Check what has changed | Terminal → `git status` |

---

> **One last thing.** The goal of Manual Gating is not to slow you down — it is to make
> sure you understand every line of code that goes into your project. A line you do not
> understand is a line you cannot fix when it breaks. Ask questions freely.
> The Aimergent does not get bored. There are no stupid questions in a Side-Chat.
