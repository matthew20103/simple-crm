# SimpleCRM – Project Instructions for Claude

> Claude Code reads this file at the start of every session. Keep it short and accurate.
> Lines marked `STUDENT TODO` are for **you** to complete during Step 1 of the lab.

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

## How to run the app

<!-- STUDENT TODO: Write the exact command(s) to start the app on macOS AND on Windows,
     the URL to open, how to use a different port, and how to stop the server.
     Hint: see ARCHITECTURE.md sections 2 and 8. -->

## How to run the tests

<!-- STUDENT TODO: Write the exact command to run all tests (macOS and Windows).
     Hint: the tests live in the tests/ folder and use unittest. -->

## Coding rules

- Keep functions small and give them clear names. Readable beats clever.
<!-- STUDENT TODO: Add at least 4 more rules. Think about:
     - how SQL must be written (look at the security table in ARCHITECTURE.md)
     - how user text must be put into HTML
     - which module is allowed to contain SQL
     - what to do after a successful form POST
     - what happens when form input is invalid -->

## How Claude should work in this project

- Work one milestone at a time (see docs/LAB_GUIDE.md). Before a big change, show a short plan first.
- After every code change, run the tests and fix any failures before saying you are done.
- Explain what you changed in 2–3 plain sentences. The student is learning.
<!-- STUDENT TODO: Add one rule telling Claude what to do if it thinks it needs
     a package that is NOT in the Python standard library. -->

## Definition of done

<!-- STUDENT TODO: List 3–4 checks that must ALL be true before a task counts as "done".
     Hint: think about tests, the acceptance criteria in SPEC.md, and trying it in the browser. -->
