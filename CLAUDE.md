# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Spendly** — an INR-focused expense tracker built as a step-by-step learning project. The scaffold and UI are complete; database, auth, and CRUD logic are implemented incrementally by students across numbered steps.

## Commands

```bash
# Activate virtual environment (Windows)
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run dev server (http://localhost:5001)
python app.py

# Run tests
pytest
```

No build step. CSS and JS are plain static files served directly by Flask.

## Architecture

**Stack**: Python Flask 3.1.3 · Jinja2 templates · SQLite · vanilla CSS/JS

```
app.py              # Flask app — all routes defined here
database/db.py      # SQLite helpers: get_db(), init_db(), seed_db() (student-implemented)
templates/          # Jinja2 templates — all extend base.html
static/css/         # Single style.css with CSS custom properties design system
static/js/main.js   # Client-side JS scaffold
```

**Routing**: All routes live in `app.py`. Templates use `url_for()` for all internal links. Placeholder routes return plain strings with the step number where they'll be implemented.

**Database**: SQLite via the stdlib `sqlite3` module. `database/db.py` must expose `get_db()` (connection with `row_factory=sqlite3.Row` and `PRAGMA foreign_keys=ON`), `init_db()`, and `seed_db()`. The `.db` file is gitignored.

**Templates**: `base.html` provides the navbar (Sign in / Get started), footer (Terms, Privacy), Google Fonts (DM Serif Display + DM Sans), and the JS include. All pages extend it via `{% block content %}`.

## Step-based curriculum structure

Steps are referenced in placeholder route comments:
- **Step 1–2**: `database/db.py` — SQLite setup and seed data  
- **Step 3**: `/logout` — session teardown  
- **Step 4**: `/profile` — user profile page  
- **Steps 7–9**: `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete` — expense CRUD  

When implementing a step, replace the placeholder string return with a proper handler.

## CSS conventions

`static/css/style.css` uses CSS custom properties defined on `:root` for all colors, spacing, and radii. Key tokens: `--accent` (green `#1a472a`), `--accent-warm` (orange `#c17f24`), `--danger` (red `#c0392b`). Max content width is `1200px`; auth forms are `440px`. Do not introduce a CSS framework — extend the existing design system.
