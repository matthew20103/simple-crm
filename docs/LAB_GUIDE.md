# Lab Guide – Vibe-Code a Local CRM with Claude Code

**Goal:** by the end of this lab you will have a real, working CRM running on your own laptop at
**http://127.0.0.1:8000**. You won't write any code. You'll steer Claude Code the way you'd brief
a capable new assistant: clear instructions, a plan, and careful checks.

**Time:** about 2.5–3 hours · **No programming experience needed** · **Installs needed:** none beyond Python + Claude Code

---

## What's in this starter kit

| File | What it is, in plain words | Your job |
|------|----------------------------|----------|
| `CLAUDE.md` | The briefing note Claude reads every time it starts | Answer the questions in **Part B** (Step 1) |
| `.claude/settings.json` | Safety rules: what Claude may and may not do on your laptop | Just look (Step 1) |
| `.claude/commands/run-crm.md` | A ready-made shortcut: type `/run-crm` to start your CRM | Use it |
| `.claude/commands/check-spec.md` | A shortcut that checks your CRM against the checklist | Finish it (Step 7) |
| `.claude/commands/add-feature.md` | A shortcut for adding new features safely | Write it (Step 8) |
| `.claude/agents/spec-reviewer.md` | A "helper Claude" that reviews your project | Finish its checklist (Step 7) |
| `docs/SPEC.md` | What the CRM must do. Written for Claude, but section 4 is your test checklist | Read section 4 |
| `docs/ARCHITECTURE.md` | How the CRM is built. Written for Claude | Read sections 1–2 |

> **Blanks** look like this: `(write your answer here)`. Replace them with your own words, in plain
> English. There are no "programming" answers in this lab. If you can explain it to a colleague,
> you can write it here.

**How to edit a file:** open the project folder in any text editor (for example VS Code, Notepad
or TextEdit), change the text and save. Or ask Claude: *"Open CLAUDE.md and help me answer
question B2."*

---

## Handy Claude Code controls

| Action | How |
|--------|-----|
| Start Claude Code in this folder | Open a terminal in the project folder and type `claude` |
| Switch to **Plan Mode** (Claude plans but doesn't change anything) | Press **Shift+Tab** until the bottom line says plan mode |
| Stop Claude mid-answer | **Esc** |
| See the safety rules | `/permissions` |
| See your helper Claudes | `/agents` |
| Start a fresh conversation (your files are kept) | `/clear` |
| Run a shortcut | Type `/` and pick it, for example `/run-crm` |
| Help | `/help` |

---

## Step 0 – Check your laptop (10 min)

Open a terminal (macOS: **Terminal** · Windows: **PowerShell** or **Windows Terminal**) and type:

| Check | macOS | Windows |
|-------|-------|---------|
| Python 3.10+ | `python3 --version` | `python --version` **or** `py --version` |
| Claude Code | `claude --version` | `claude --version` |

- Python older than 3.10, or missing? Ask your instructor or IT. **Don't** install anything yourself.
- **Windows tip:** if typing `python` opens the Microsoft Store, use `py` instead.

Get the starter kit, if you haven't already:
- With git: `git clone https://github.com/matthew20103/simple-crm.git` then `cd simple-crm`
- Without git: open https://github.com/matthew20103/simple-crm, click **Code → Download ZIP**, and unzip it

> **Done when:** both commands print a version number and your terminal is in the project folder.

---

## Step 1 – Brief Claude: fill in CLAUDE.md (30 min)

1. **Skim the big picture.** Read the introduction and **section 4 (the checklist)** of
   `docs/SPEC.md`. That's what you're going to build. Skip the technical tables.

2. **Look at the safety rules.** Open `.claude/settings.json`. The `deny` list blocks Claude from
   installing software (`pip install`) and downloading from the internet (`curl`).
   Think about it: *why would that matter on a company laptop?*

3. **Answer Part B of `CLAUDE.md`** (questions B1–B7), in your own words. Part A is technical and
   already done by your instructor. Tips:
   - Short, specific answers beat long ones. "Show a red message saying which field is missing"
     is better than "handle errors well".
   - Think like a user of the CRM, not like a programmer.
   - Stuck? Ask Claude: *"Explain question B3 in CLAUDE.md to me with an everyday example."*

4. **Let Claude translate your answers.** Start Claude Code (`claude`) and type:

   > Read CLAUDE.md. Turn my answers in Part B into clear rules for yourself and write them in
   > Part C. Don't change my original answers. Then tell me, in plain English, if anything in my
   > answers is unclear or missing. Don't build anything yet.

   Read Part C. Does it match what you meant? If not, tell Claude what to change.

5. **Mini exercise: add your own safety rule, in plain words.** Type:

   > Add a personal rule to .claude/settings.local.json so that you can never delete folders or
   > upload anything to GitHub. Explain the rule to me in plain English.

   Then type `/permissions` and check that your new rule appears in the **Deny** list.
   (`settings.local.json` is only for you. It doesn't get shared.)

> **Done when:** no `(write your answer here)` is left in Part B, Part C is filled in by Claude,
> and your personal rule appears under `/permissions`.

---

## Step 2 – Plan before you build (15 min)

Switch to **Plan Mode** (Shift+Tab) and type:

> Using docs/SPEC.md and docs/ARCHITECTURE.md, plan how to build SimpleCRM in 4 milestones:
> (1) the web server and a first page, (2) Companies, (3) Contacts and search,
> (4) Activities and the Dashboard. For each milestone, explain in plain English what I will be
> able to see and do in the browser afterwards, and how we'll check that it works.
> Remember all the rules in CLAUDE.md.

Read the plan. Ask questions about anything you don't understand. That's part of the job!
**Push back** if the plan mentions installing anything (for example "pip install" or "Flask") or
loading things from the internet (for example a "CDN"). Those break the rules. When you're happy,
approve the plan.

> **Done when:** you understand what you'll be able to do after each milestone.

---

## Steps 3–6 – Build it, one milestone at a time

For each milestone: **copy the prompt into Claude**, wait for it to finish, then **try it
yourself in the browser**. You don't need to understand every word in the prompts. They're
written for Claude.

### Step 3 – Milestone 1: the web server and a first page (20 min)

> Build milestone 1: app.py with a ThreadingHTTPServer on 127.0.0.1, a simple router, the database
> setup in db.py (all three tables from SPEC.md), a base page layout in views.py with the navigation
> bar, static/style.css, and a Dashboard page that shows zero counts. Add a first test.

Then type `/run-crm` and open **http://127.0.0.1:8000** in your browser.

> **Done when:** you see a Dashboard page with Dashboard · Contacts · Companies at the top.

**Troubleshooting**
- *"Address already in use"*: something else is using the same "door" (port 8000). Type `/run-crm 8080`, then open http://127.0.0.1:8080.
- *The browser says it can't connect*: the CRM isn't running. Type `/run-crm` again.
- *Anything else*: copy the error message into Claude and ask *"What does this mean, in plain English, and how do we fix it?"*

### Step 4 – Milestone 2: Companies (20 min)

> Build milestone 2: Companies list with search, create, detail, edit and delete, exactly as in
> SPEC.md. Validate that the name is required and unique. Add tests. Run them.

**Try it:** add 2–3 companies, edit one, delete one. Try saving a company with no name. Does it
behave the way you described in your answer to **B2**?

> **Done when:** checklist item AC3 works in your browser.

### Step 5 – Milestone 3: Contacts and search (25 min)

> Build milestone 3: Contacts CRUD with the company drop-down and status drop-down, search (q) and
> status filter on the list page, and all validation rules from SPEC.md. Add tests. Run them.

**Try it:** add contacts at different companies. Search for part of a company name. Type a wrong
email address. Add a contact named `<script>alert(1)</script>`. It must show up as plain text,
not do anything strange (this is question **B3** in action).

> **Done when:** checklist items AC4–AC8 and AC12 work.

### Step 6 – Milestone 4: Activities and the Dashboard (20 min)

> Build milestone 4: logging activities on the contact detail page, deleting activities, and the full
> Dashboard (counts, contacts per status, 10 most recent activities). Add tests. Run them.

**Try it:** log a call on a contact. Check that it shows on the Dashboard. Delete a company and
check what happens to its contacts (question **B4**). Then stop the CRM (Ctrl+C, or ask Claude),
start it again with `/run-crm`, and check that your data is still there.

> **Done when:** checklist items AC9–AC11 and AC14 work.

---

## Step 7 – Check your work like a pro (25 min)

1. **Finish the `/check-spec` shortcut.** Open `.claude/commands/check-spec.md` and answer the two
   questions in plain English: *how should a careful colleague check the CRM*, and *how should
   they report back to you?* Save the file, then type `/check-spec`.

2. **Finish the reviewer.** Open `.claude/agents/spec-reviewer.md` and write at least 6 things a
   careful colleague should make sure of before your manager sees the CRM. Save it, then type:

   > Use the spec-reviewer agent to review the whole project. Explain the findings to me in plain English.

3. **Fix the problems.** For each item under **Must fix** and **Should fix**, ask Claude to fix it
   **one at a time**, then try it again in the browser.

> **Done when:** `/check-spec` shows every checklist item passing, and the reviewer finds nothing under "Must fix".

---

## Step 8 – Write your own shortcut and add a feature (20+ min)

1. Open `.claude/commands/add-feature.md` and write, step by step, how you'd brief a new
   assistant before they add a feature to your CRM. The questions in the file will guide you.

2. Try your shortcut:

   > /add-feature a button on the Contacts page that downloads all contacts as a spreadsheet (CSV) file

3. More ideas: a **Deals** page to track sales opportunities, sorting the contact list, or a dark colour theme.

> **Done when:** Claude followed *your* steps and the new feature works in your browser.

---

## Final checklist

- [ ] My CRM opens at http://127.0.0.1:8000, and every item AC1–AC16 in SPEC.md section 4 passes
- [ ] I asked Claude to run the automated tests, and they all pass
- [ ] Nothing extra was installed on my laptop
- [ ] Part B of CLAUDE.md, `/check-spec`, the reviewer and `/add-feature` are all completed in my own words

## Reflection questions (for the class discussion)

1. Which of your answers in Part B made the biggest difference to what Claude built?
2. When did Plan Mode, or asking a question, change what Claude was going to do?
3. What did the reviewer catch that you missed?
4. What makes a *good* instruction to an AI assistant? How is it like briefing a person?

## Reset / recover

- **Start again with an empty CRM:** stop the CRM and delete the file `crm.db`. A new, empty one is created next time.
- **Claude went off track:** press **Esc** and explain what's wrong, or type `/clear` and paste the milestone prompt again.
- **Stop the CRM:** press **Ctrl+C** in the terminal where it runs, or ask Claude: *"Stop the CRM server."*

## Mini glossary

| Word | Meaning |
|------|---------|
| **CRM** | Customer Relationship Management: an app for keeping track of customers and your contact with them |
| **Terminal** | A window where you type commands instead of clicking |
| **127.0.0.1** | A special address that always means "this computer". Nobody else can open your CRM |
| **Port (8000)** | Like a door number on your computer. The CRM listens at door 8000 |
| **Server** | The part of the CRM that runs in the background and sends pages to your browser |
| **Database (`crm.db`)** | The file where all your companies, contacts and activities are saved |
| **Test** | A small automatic check that Claude writes to prove a feature still works |
| **Slash command** | A shortcut starting with `/`. It's simply saved instructions for Claude |
| **Subagent** | A "helper Claude" with one specific job, like reviewing |
| **Plan Mode** | Claude thinks and plans, but doesn't change any files |
