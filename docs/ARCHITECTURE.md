# SimpleCRM – System Architecture

## 1. Design goals

| Goal | What it means for us |
|------|----------------------|
| **Runs on a company laptop** | No `pip install`, no `npm install`, no admin rights, no Docker. We use only Python and its standard library. |
| **Works offline** | No CDN links (Bootstrap, jQuery, Google Fonts, ...). All CSS is served from our own `static/` folder. |
| **Local only** | The server listens on `127.0.0.1`, so other computers on the network cannot reach it. |
| **Easy to read** | Few files, plain HTML forms, no JavaScript framework. |
| **Same on Windows and macOS** | Use `pathlib` for file paths. No shell scripts are needed to run the app. |

## 2. System requirements

| Item | Requirement | How to check |
|------|-------------|--------------|
| Python | 3.10 or newer | macOS: `python3 --version` · Windows: `python --version` or `py --version` |
| Browser | Any modern browser (Edge, Chrome, Safari, Firefox) | – |
| Claude Code | Installed and signed in | `claude --version` |
| Disk | < 1 MB for code + database | – |
| Network | Not needed while the app is running | – |
| Port | 8000 is free (or pick another) | Just start the app. It tells you if the port is taken. |

## 3. The big picture

```
 ┌──────────────┐   HTTP (GET pages, POST forms)   ┌─────────────────────────────────────┐
 │   Browser    │ ───────────────────────────────▶ │  app.py                             │
 │ 127.0.0.1:8000│ ◀─────────────────────────────── │  ThreadingHTTPServer (127.0.0.1)    │
 └──────────────┘      HTML pages / redirects      │    │                                │
                                                   │    ▼                                │
                                                   │  Router: (method, URL pattern)      │
                                                   │          → handler function         │
                                                   │    │                                │
                                                   │    ├─▶ validate form input          │
                                                   │    ├─▶ db.py   (read / write data)  │
                                                   │    └─▶ views.py (build HTML)        │
                                                   └───────────────┬─────────────────────┘
                                                                   │ sqlite3 (parameterised SQL)
                                                                   ▼
                                                           ┌───────────────┐
                                                           │   crm.db      │  ← one local file
                                                           └───────────────┘
```

## 4. Project layout (target)

```
simple-crm/            # your project folder (the cloned repo)
├── CLAUDE.md          # Instructions Claude Code reads every session
├── .claude/           # Claude Code settings, slash commands, subagents
├── app.py             # Entry point: HTTP server, router, request handlers
├── db.py              # Database: schema, connection, all SQL queries
├── views.py           # HTML templates / rendering helpers
├── static/
│   └── style.css      # All styling (no CSS framework)
├── tests/
│   └── test_app.py    # unittest tests (standard library)
├── docs/
│   ├── ARCHITECTURE.md
│   ├── SPEC.md
│   └── LAB_GUIDE.md
└── crm.db             # Created automatically. Do NOT commit.
```

### Responsibilities

| Module | Does | Does **not** |
|--------|------|--------------|
| `app.py` | Parse the URL, the query string and the form body. Pick a handler. Validate input. Send responses and redirects. Start the server. | Contain SQL. Build large HTML strings. |
| `db.py` | Create tables. Open connections. Run parameterised queries. Return plain dicts or rows. | Know about HTTP or HTML. |
| `views.py` | Turn data into HTML. **Escape every value** with `html.escape`. | Query the database. |

## 5. Standard-library modules we use

| Module | Used for |
|--------|----------|
| `http.server` | `ThreadingHTTPServer`, `BaseHTTPRequestHandler` |
| `urllib.parse` | `urlparse`, `parse_qs` (query strings and form bodies) |
| `sqlite3` | Database |
| `html` | `html.escape` to prevent XSS |
| `re` | URL patterns for routing, email check |
| `pathlib` | File paths that work on Windows and macOS |
| `datetime` | Timestamps |
| `unittest` | Tests |
| `argparse` / `os` | `--port` flag / `PORT` environment variable |

## 6. Request flow example: "Create a contact"

1. The browser sends `GET /contacts/new`. The handler loads the companies for the drop-down, and `views.py` renders an empty form.
2. The user submits the form, and the browser sends `POST /contacts/new` with the body `first_name=Ada&last_name=Lovelace&...`
3. `app.py` reads `Content-Length` bytes and parses them with `parse_qs`.
4. The handler validates the input:
   - **Invalid:** re-render the form with the errors and the typed values, status **400**.
   - **Valid:** `db.create_contact(...)` returns the new id, and the server sends **303** with `Location: /contacts/<id>`.
5. The browser follows the redirect with `GET /contacts/<id>` and sees the detail page.

## 7. Security basics (keep these even though it is "only local")

| Risk | Rule |
|------|------|
| Other machines connecting | Bind to `127.0.0.1` only. Never use `0.0.0.0`. |
| SQL injection | Always use `?` placeholders. Never build SQL with f-strings or `+` from user input. |
| XSS (cross-site scripting) | Escape every user value with `html.escape` before putting it in HTML. |
| CSRF (another website posting to our local app) | For POST requests: if an `Origin` header is present, it must equal `http://127.0.0.1:<port>` or `http://localhost:<port>`. Otherwise reply 403. |
| DNS rebinding | Reject any request whose `Host` header is not `127.0.0.1:<port>` or `localhost:<port>` (403). |
| Path traversal in `/static/` | Only serve files that really sit inside the `static/` folder. Reject `..`. |

## 8. Configuration

| Setting | Default | Override |
|---------|---------|----------|
| Host | `127.0.0.1` | Not configurable (on purpose) |
| Port | `8000` | `python app.py --port 8080` or environment variable `PORT=8080` |
| Database file | `crm.db` next to `app.py` | Environment variable `CRM_DB=/path/to/file.db` (the tests use this) |
