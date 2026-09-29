# Lab Guide – Vibe-Code a Local CRM with Claude Code

**Goal:** by the end of this lab you will have a real, working CRM running on your own laptop at
**http://127.0.0.1:8000**. You won't write the code by hand. You'll steer Claude Code with good
instructions, plans, and checks.

**Time:** about 2.5–3 hours · **Level:** beginner-friendly · **Installs needed:** none beyond Python + Claude Code

---

## What's in this starter kit

| File | What it is | Your job |
|------|------------|----------|
| `CLAUDE.md` | Project memory. Claude reads it at the start of every session. | Fill in the `STUDENT TODO` blanks (Step 1) |
| `.claude/settings.json` | Permission rules: what Claude may and may not run | Read it (Step 1) |
| `.claude/commands/run-crm.md` | Custom slash command `/run-crm` | Use it |
| `.claude/commands/check-spec.md` | Custom slash command `/check-spec` | Finish it (Step 7) |
| `.claude/commands/add-feature.md` | Custom slash command `/add-feature` | Write it (Step 8) |
| `.claude/agents/spec-reviewer.md` | A review **subagent** | Finish its checklist (Step 7) |
| `docs/SPEC.md` | What the CRM must do, plus acceptance criteria | Read it |
| `docs/ARCHITECTURE.md` | How the CRM is built, plus security rules | Read it |

> **Blanks** look like this: `<!-- STUDENT TODO: ... -->`. Replace the whole comment with your answer.

---

## Handy Claude Code controls

| Action | How |
|--------|-----|
| Start Claude Code in this folder | Open a terminal in the project folder and run `claude` |
| Switch to **Plan Mode** (Claude plans, doesn't edit) | Press **Shift+Tab** until the status line says plan mode |
| Stop Claude mid-answer | **Esc** |
| See or change permissions | `/permissions` |
| See your subagents | `/agents` |
| Start a fresh conversation (keeps files) | `/clear` |
| Run a custom command | Type `/` and pick it, e.g. `/run-crm` |
| Help | `/help` |

---

## Step 0 – Check your laptop (10 min)

Open a terminal (macOS: **Terminal** · Windows: **PowerShell** or **Windows Terminal**) and run:

| Check | macOS | Windows |
|-------|-------|---------|
| Python 3.10+ | `python3 --version` | `python --version` **or** `py --version` |
| Claude Code | `claude --version` | `claude --version` |

- Python older than 3.10, or missing? Ask your instructor or IT. **Don't** install random packages.
- **Windows tip:** if typing `python` opens the Microsoft Store, use `py` instead.
- Make a note of **your** Python command. You'll use it all day: `python3` / `python` / `py`.

Get the starter kit, if you haven't already:
- With git: `git clone https://github.com/matthew20103/simple-crm.git` then `cd simple-crm`
- Without git: open https://github.com/matthew20103/simple-crm, click **Code → Download ZIP**, and unzip it

> **Done when:** both commands print a version number and you are in the project folder.

---

## Step 1 – Understand the project and fill in CLAUDE.md (25 min)

1. Read `docs/SPEC.md` and `docs/ARCHITECTURE.md`. Skim them. You don't need to memorise anything.
2. Open `.claude/settings.json`. Answer for yourself: *Why do we deny `pip install` and `curl` on a company laptop?*
3. Fill in **every** `STUDENT TODO` in `CLAUDE.md`:
   - How to run the app (macOS **and** Windows)
   - How to run the tests
   - At least 4 more coding rules
   - A rule about third-party packages
   - Definition of done
4. Start Claude Code in the project folder (`claude`) and ask it to check your work:

   > Read CLAUDE.md. Is anything unclear, missing or contradictory for building this project? Don't write any code yet.

5. **Mini exercise – your personal safety rule.** Create `.claude/settings.local.json` (it's in
   `.gitignore`, so it stays personal). Add a `deny` rule that stops Claude from deleting folders
   or pushing to git. Look at `settings.json` for the format. Check it worked with `/permissions`.

> **Done when:** there are no `STUDENT TODO` comments left in CLAUDE.md, and Claude found no big gaps.

---

## Step 2 – Plan before you build (15 min)

Switch to **Plan Mode** (Shift+Tab) and ask:

> Using docs/SPEC.md and docs/ARCHITECTURE.md, plan how to build SimpleCRM in 4 milestones:
> (1) server + Hello page, (2) Companies CRUD, (3) Contacts CRUD + search, (4) Activities + Dashboard.
> For each milestone list the files you will create or change, the routes, and how we will test it.
> Remember the hard constraints in CLAUDE.md.

Read the plan carefully. **Push back** if something breaks a rule (for example, it suggests Flask
or a CDN). When you're happy, approve it.

> **Done when:** you have a plan you understand and agree with.

---

## Step 3 – Milestone 1: server + Hello page (20 min)

> Build milestone 1: app.py with a ThreadingHTTPServer on 127.0.0.1, a simple router, the database
> setup in db.py (all three tables from SPEC.md), a base page layout in views.py with the navigation
> bar, static/style.css, and a Dashboard page that shows zero counts. Add a first test.

Then run `/run-crm` and open **http://127.0.0.1:8000** in your browser.

> **Done when:** you see the Dashboard with the navigation bar, and a `crm.db` file appears.

**Troubleshooting**
- *"Address already in use"* → another program uses port 8000. Try `/run-crm 8080`.
- *Browser shows nothing* → is the server still running? Check the terminal.
- *Windows:* `PORT=8080 python app.py` doesn't work in PowerShell. Use `python app.py --port 8080`.

---

## Step 4 – Milestone 2: Companies (20 min)

> Build milestone 2: Companies list with search, create, detail, edit and delete, exactly as in
> SPEC.md. Validate that the name is required and unique. Add tests. Run them.

Try it in the browser: add 2–3 companies, edit one, delete one. Try saving an empty name.

> **Done when:** AC3 in SPEC.md works in the browser and the tests pass.

---

## Step 5 – Milestone 3: Contacts + search (25 min)

> Build milestone 3: Contacts CRUD with the company drop-down and status drop-down, search (q) and
> status filter on the list page, and all validation rules from SPEC.md. Add tests. Run them.

Try it: create contacts at different companies, then search by part of a company name. Try an
invalid email. Try the name `<script>alert(1)</script>`. It must appear as plain text.

> **Done when:** AC4–AC8 and AC12 work.

---

## Step 6 – Milestone 4: Activities + Dashboard (20 min)

> Build milestone 4: logging activities on the contact detail page, deleting activities, and the full
> Dashboard (counts, contacts per status, 10 most recent activities). Add tests. Run them.

> **Done when:** AC9–AC11 work. Stop the server, start it again, and check that your data is still there (AC14).

---

## Step 7 – Check your work like a pro (25 min)

1. **Finish `/check-spec`**: complete the `STUDENT TODO` sections in
   `.claude/commands/check-spec.md`, then run `/check-spec`.
2. **Finish the `spec-reviewer` subagent**: write its checklist in
   `.claude/agents/spec-reviewer.md`, then ask:

   > Use the spec-reviewer agent to review the whole project.

3. Fix everything under **Must fix** and **Should fix**. Ask Claude to fix one finding at a time.

> **Done when:** `/check-spec` shows every AC passing, and the reviewer reports no "Must fix" items.

---

## Step 8 – Your own slash command + a stretch goal (20+ min)

1. Write `.claude/commands/add-feature.md` from scratch (the hints are in the file).
2. Use it:

   > /add-feature export all contacts to a CSV file from the contacts page

3. Other ideas from SPEC.md section 5: a Deals pipeline, sorting, dark mode.

> **Done when:** your command works and the new feature has a test.

---

## Final checklist

- [ ] The CRM opens at http://127.0.0.1:8000 and all AC1–AC16 pass
- [ ] `python -m unittest discover -s tests` (or `python3` / `py`) is green
- [ ] No third-party packages were installed (AC15)
- [ ] CLAUDE.md, `/check-spec`, `/add-feature` and `spec-reviewer` are all completed

## Reflection questions (for the class discussion)

1. Which line in CLAUDE.md saved you the most trouble? Which one did Claude ignore?
2. When did Plan Mode change what Claude was going to do?
3. What did the reviewer subagent catch that you missed?
4. How would you explain "vibe coding with guard rails" to a colleague?

## Reset / recover

- **Start with an empty database:** stop the server and delete `crm.db`. It's recreated on the next start.
- **Claude went off track:** press **Esc**, explain what's wrong, or use `/clear` and restate the milestone.
- **Stop the server:** press **Ctrl+C** in the terminal where it runs, or ask Claude to stop the background process.
