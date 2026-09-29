# SimpleCRM – Functional Specification

> **For students:** this document is written mainly for Claude. You **don't** need to understand
> the tables. Read the introduction below and the **checklist in section 4**. That checklist
> is how you'll test your CRM.

SimpleCRM is a small customer relationship manager for one user. It runs on your own
computer and you open it in a browser at **http://127.0.0.1:8000**.

It keeps track of three things:

1. **Companies**: the organisations you work with
2. **Contacts**: the people at those companies (or independent people)
3. **Activities**: calls, emails, meetings and notes logged against a contact

A **Dashboard** gives a quick overview.

---

## 1. Data model

SQLite database file `crm.db`. Foreign keys must be switched on (`PRAGMA foreign_keys = ON`).
All timestamps are stored as ISO-8601 text, for example `2026-09-29T14:05:00`.

### 1.1 `companies`
| Column      | Type    | Rules                                  |
|-------------|---------|----------------------------------------|
| id          | INTEGER | Primary key, auto-increment            |
| name        | TEXT    | **Required**, unique (case-insensitive)|
| industry    | TEXT    | Optional                               |
| website     | TEXT    | Optional                               |
| phone       | TEXT    | Optional                               |
| address     | TEXT    | Optional                               |
| notes       | TEXT    | Optional                               |
| created_at  | TEXT    | Set automatically on insert            |
| updated_at  | TEXT    | Set automatically on insert and update |

### 1.2 `contacts`
| Column      | Type    | Rules                                                          |
|-------------|---------|----------------------------------------------------------------|
| id          | INTEGER | Primary key, auto-increment                                    |
| first_name  | TEXT    | **Required**                                                   |
| last_name   | TEXT    | **Required**                                                   |
| email       | TEXT    | Optional; if given, must look like an email (`x@y.z`)          |
| phone       | TEXT    | Optional                                                       |
| job_title   | TEXT    | Optional                                                       |
| company_id  | INTEGER | Optional, references `companies.id`, **ON DELETE SET NULL**   |
| status      | TEXT    | One of `lead`, `prospect`, `customer`, `inactive`; default `lead` |
| notes       | TEXT    | Optional                                                       |
| created_at  | TEXT    | Set automatically on insert                                    |
| updated_at  | TEXT    | Set automatically on insert and update                         |

### 1.3 `activities`
| Column        | Type    | Rules                                                     |
|---------------|---------|-----------------------------------------------------------|
| id            | INTEGER | Primary key, auto-increment                               |
| contact_id    | INTEGER | **Required**, references `contacts.id`, **ON DELETE CASCADE** |
| type          | TEXT    | One of `call`, `email`, `meeting`, `note`                 |
| subject       | TEXT    | **Required**                                              |
| details       | TEXT    | Optional                                                  |
| activity_date | TEXT    | Date `YYYY-MM-DD`; defaults to today                      |
| created_at    | TEXT    | Set automatically on insert                               |

---

## 2. Pages and routes

All pages share a top navigation bar with the links **Dashboard · Contacts · Companies**.

| Method   | Path                          | What it does |
|----------|-------------------------------|--------------|
| GET      | `/`                           | Dashboard |
| GET      | `/contacts`                   | Contact list. Optional query `?q=` (search) and `?status=` (filter) |
| GET      | `/contacts/new`               | Empty "new contact" form |
| POST     | `/contacts/new`               | Create contact → redirect to its detail page |
| GET      | `/contacts/<id>`              | Contact detail + activity timeline + "log activity" form |
| GET      | `/contacts/<id>/edit`         | Edit form (pre-filled) |
| POST     | `/contacts/<id>/edit`         | Save changes → redirect to detail page |
| POST     | `/contacts/<id>/delete`       | Delete contact (and its activities) → redirect to `/contacts` |
| POST     | `/contacts/<id>/activities`   | Log a new activity → redirect to contact detail |
| POST     | `/activities/<id>/delete`     | Delete one activity → redirect to its contact |
| GET      | `/companies`                  | Company list. Optional `?q=` search on name/industry |
| GET      | `/companies/new`              | Empty "new company" form |
| POST     | `/companies/new`              | Create company → redirect to its detail page |
| GET      | `/companies/<id>`             | Company detail + list of its contacts |
| GET      | `/companies/<id>/edit`        | Edit form |
| POST     | `/companies/<id>/edit`        | Save changes → redirect to detail page |
| POST     | `/companies/<id>/delete`      | Delete company (its contacts are kept, `company_id` becomes empty) |
| GET      | `/static/<file>`              | Static files (CSS) from the `static/` folder only |

### 2.1 Dashboard (`/`)
- Total number of companies, contacts and activities
- Number of contacts in each status (lead / prospect / customer / inactive)
- The 10 most recent activities (date, type, subject, contact name as a link)

### 2.2 Contact list (`/contacts`)
- Table: Name (link), Company (link), Email, Phone, Status
- Search box `q`: case-insensitive match against first name, last name, email **or** company name
- Status drop-down filter (with "All")
- Sorted by last name, then first name
- Shows a friendly "No contacts found" message when the list is empty

### 2.3 Forms
- The contact form has a **drop-down of companies** (plus a "— none —" option) and a **status drop-down**
- Every delete button asks the user to confirm first (a simple `onclick="return confirm(...)"` is fine)

---

## 3. Behaviour rules

1. **Post/Redirect/Get**: when a POST succeeds, the server replies `303 See Other` and redirects to a page.
2. **Validation errors**: the form is shown again with HTTP **400**, a list of error messages, and
   the values the user already typed. Nothing is saved.
3. **Unknown IDs** (for example `/contacts/9999`) show a friendly **404** page.
4. **Unknown paths** show the same 404 page.
5. Text typed by users must be shown exactly as typed. `<script>` must be displayed as text, not run.
6. The database file and tables are created automatically the first time the app starts.

---

## 4. Acceptance criteria (the "is it done?" checklist)

- [ ] AC1: `python app.py` starts the server and prints the URL `http://127.0.0.1:8000`
- [ ] AC2: Opening `http://127.0.0.1:8000` shows the Dashboard with zero counts on a fresh database
- [ ] AC3: I can create, view, edit and delete a **company**
- [ ] AC4: I can create, view, edit and delete a **contact**, and link it to a company
- [ ] AC5: Creating a contact with an empty last name shows an error and saves nothing
- [ ] AC6: Entering `not-an-email` as an email shows an error
- [ ] AC7: Typing part of a company name in the Contacts search box finds the contacts at that company
- [ ] AC8: Choosing a status (for example "customer") in the Contacts filter shows only contacts with that status
- [ ] AC9: I can log an activity on a contact, and it appears on the contact page and on the Dashboard
- [ ] AC10: Deleting a contact also deletes its activities
- [ ] AC11: Deleting a company keeps its contacts, and they now show no company
- [ ] AC12: A contact named `<script>alert(1)</script>` is displayed as plain text
- [ ] AC13: Opening the address of a contact that doesn't exist (http://127.0.0.1:8000/contacts/9999) shows a friendly "not found" page, not a crash
- [ ] AC14: Data is still there after the server is stopped and started again
- [ ] AC15: The project installs **no** third-party packages (standard library only)
- [ ] AC16: Automated tests pass with `python -m unittest discover -s tests`

---

## 5. Out of scope (stretch goals, only after all AC items pass)

- Export contacts to CSV (`/contacts/export.csv`, using the `csv` module)
- A **Deals** table (title, value, stage, contact) with a pipeline summary on the Dashboard
- Sorting by column / pagination
- Dark mode (CSS only)
