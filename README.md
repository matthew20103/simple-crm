# SimpleCRM Lab: Vibe-Code a Local CRM with Claude Code

In this lab you use **Claude Code** to build a small, working CRM (Companies, Contacts,
Activities, Dashboard) that runs on your own laptop at **http://127.0.0.1:8000**.

It is built for company laptops: **Python 3.10+ standard library only**, so there is nothing to
`pip install` and no internet is needed while the app runs.

## Get the starter kit

**Option A: with git**
```bash
git clone https://github.com/matthew20103/simple-crm.git
cd simple-crm
```

**Option B: without git.** On this page, click **Code → Download ZIP**, unzip it, and open the folder.

## Start the lab

1. Check your tools: `python3 --version` (Windows: `python --version` or `py --version`) and `claude --version`
2. Open a terminal in the project folder and run `claude`
3. Follow **[docs/LAB_GUIDE.md](docs/LAB_GUIDE.md)**, starting at Step 0

## What's inside

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | Project instructions for Claude (you fill in the blanks) |
| `.claude/settings.json` | Permission guard rails |
| `.claude/commands/` | Custom slash commands: `/run-crm`, `/check-spec`, `/add-feature` |
| `.claude/agents/` | The `spec-reviewer` subagent |
| `docs/SPEC.md` | What to build, plus acceptance criteria |
| `docs/ARCHITECTURE.md` | How to build it, plus security rules |
| `docs/LAB_GUIDE.md` | Step-by-step instructions |
