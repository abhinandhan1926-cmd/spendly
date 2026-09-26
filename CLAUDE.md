# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendly is a personal expense-tracker web app built with Flask, currently set up as a **student teaching scaffold** — a step-by-step build exercise, not a finished app. Route handlers and files carry comments like `# Students will write this file in Step 1` or return placeholder strings like `"Logout — coming in Step 3"`; these mark deliberately unimplemented steps, not bugs.

The intended end state: users register/log in, log expenses (category, amount, date, description), and view spending patterns — category breakdowns, monthly summaries, date-range filtering. The app is INR-oriented (₹, "Track every rupee. Know where it goes.").

## Commands

```bash
# Run the dev server (http://localhost:5001)
python app.py

# Run tests
pytest
```

There is no build step, linter, or formatter configured. `requirements.txt` lists `flask`, `werkzeug`, `pytest`, and `pytest-flask`, but no test files exist yet — `pytest-flask` fixtures (e.g. `client`) are expected to be used once tests are added.

The venv at `venv/` is checked into the working tree (but gitignored) — activate it or invoke Python from it directly rather than assuming a global install has Flask.

## Architecture

- **`app.py`** — single-file Flask app; all routes live here. There is no blueprint structure — new routes should be added directly to `app.py` following the existing pattern (route function returns `render_template(...)` or, for unimplemented steps, a placeholder string).
- **`database/db.py`** — intentionally a stub (docstring only). It's meant to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample dev data). No ORM is used — raw SQLite via the standard `sqlite3` module is the intended approach.
- **`templates/`** — Jinja templates. `base.html` defines the shared layout (nav, footer, font links, `static/css/style.css`, `static/js/main.js`) with `title`/`head`/`content`/`scripts` blocks; page templates `{% extends "base.html" %}` and fill `content`. Internal links use `url_for('<endpoint>')`, never hardcoded paths.
- **`static/css/style.css`** and **`static/js/main.js`** — plain CSS/JS, no build tooling, no framework. `main.js` is currently a placeholder comment; JS is added incrementally as features are built directly into this file (no bundler/module system).
- **Database file** `spendly.db` is SQLite, created at runtime and gitignored — never commit it.
- Fonts are DM Serif Display (headings) + DM Sans (body), loaded via Google Fonts `<link>` tags in `base.html`.

## Current implementation state

Static/auth-adjacent pages (landing, login, register, terms, privacy) and the base layout are built out with real markup and styling. Everything below the auth layer is a stub:
- No database wiring — `db.py` has no functions yet.
- Login/register/logout have no real auth logic.
- `/profile` and all `/expenses/*` routes return literal placeholder strings instead of rendering anything.

When implementing one of these "Step N" placeholders, replace the placeholder route body with real logic (DB queries via `database/db.py`, session handling for auth, real templates for expense CRUD) rather than treating the placeholder string as an API contract to preserve.
