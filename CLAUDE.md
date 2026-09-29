# SimpleCRM – Project Instructions for Claude

> **For students:** Claude Code reads this file at the start of every session. It's like a
> briefing note for a new assistant. Your instructor has already written the technical parts.
> **Your job is Part B**: answer the questions in plain English, no programming words needed.
> Replace each `(write your answer here)` with your own words.

---

# Part A – Technical setup (written by your instructor, no changes needed)

## Project overview

SimpleCRM is a small, single-user CRM (Companies, Contacts, Activities, Dashboard).
It runs locally and the user opens it at **http://127.0.0.1:8000**.

- Requirements and acceptance criteria: @docs/SPEC.md
- System design, security rules and project layout: @docs/ARCHITECTURE.md

## Hard constraints (never break these)

1. **Python 3.10+ standard library only.** Do not use `pip install`. Do not add `requirements.txt`, Flask, Django, SQLAlchemy, or any other third-party package.
2. **No external resources.** No CDN links, web fonts or remote scripts in HTML/CSS. The app must work fully offline.
3. **Local only.** The server binds to `127.0.0.1`, never `0.0.0.0`.
4. **Cross-platform.** It must run on Windows and macOS. Use `pathlib`, and don't require any shell scripts to run the app.
5. **Keep the layout** described in ARCHITECTURE.md section 4 (`app.py`, `db.py`, `views.py`, `static/`, `tests/`).

## Tech stack

| Layer     | Choice                                             |
|-----------|----------------------------------------------------|
| Server    | `http.server.ThreadingHTTPServer`                  |
| Database  | `sqlite3`, file `crm.db` (created automatically)   |
| HTML      | Rendered on the server by Python, with `html.escape` |
| Styling   | One plain CSS file in `static/`                    |
| Tests     | `unittest` (standard library)                      |

## How to run the app and the tests

- Start the app. macOS: `python3 app.py` · Windows: `python app.py` (or `py app.py`)
- Open **http://127.0.0.1:8000**. Use another port with `--port 8080`. Stop with **Ctrl+C**.
- Run the tests. macOS: `python3 -m unittest discover -s tests` · Windows: `python -m unittest discover -s tests`
- Tests must use a temporary database (set the `CRM_DB` environment variable), never the real `crm.db`.

## Coding rules

- Keep functions small and give them clear names. Readable beats clever.
- SQL only with `?` placeholders. All SQL lives in `db.py`.
- Escape every value shown in HTML with `html.escape`.
- After a successful form POST, redirect with `303 See Other` (Post/Redirect/Get).
- Unknown pages and IDs show a friendly 404 page, never a Python error in the browser.

## How Claude should work in this project

- Work one milestone at a time (see docs/LAB_GUIDE.md). Before a big change, show a short plan first.
- After every code change, run the tests and fix any failures before saying you are done.
- The student is **not a programmer**. Explain what you did in plain English, without jargon.
- Treat the student's answers in Part B as requirements. If an answer is unclear, ask.

---

# Part B – What I want (written by the student, in plain English)

### B1. Who is this CRM for?
*Who would use it, and what for? Think of a real team or business.*
*Example: "A small travel agency keeping track of corporate clients and the calls we make to them."*

- (write your answer here)

### B2. When someone makes a mistake on a form
*What should happen if someone forgets a required field (like the last name) or types an email address wrongly?*
*Sentence starters: "The app should…", "The person should see…", "Nothing should…"*

- (write your answer here)

### B3. Unusual text
*People's names and notes can contain accents, apostrophes (O'Brien) or odd symbols like `< >`. How should the app show them?*

- (write your answer here)

### B4. Deleting things
*What should happen before something is deleted? And if a company is deleted, what should happen to the people who work there?*

- (write your answer here)

### B5. Extra software
*Your laptop is managed by IT, and we are not allowed to install extra software. What should Claude do if it thinks it needs something extra?*

- (write your answer here)

### B6. How Claude should talk to me
*How do you want Claude to explain its work? Think about length, words to avoid, and when it should stop and ask you.*

- (write your answer here)

### B7. When is it "done"?
*When would you tell your manager "the CRM is ready"? List 3 things you would check yourself first.*
*Sentence starters: "I can…", "I have tried…", "Claude has confirmed…"*

- (write your answer here)
- (write your answer here)
- (write your answer here)

---

# Part C – Rules from my answers (Claude fills this in)

*After you finish Part B, ask Claude: "Turn my answers in Part B into clear rules for yourself
and write them here in Part C. Don't change my original answers."*

- (Claude will write here)
