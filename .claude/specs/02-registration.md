# Spec: Registration

## Overview
Implement user registration so new visitors can create a Spendly account. This step wires up the POST handler for `/register`, validates the submitted form, hashes the password with Werkzeug, inserts the new user into the `users` table, establishes a Flask session, shows a success message and then redirects to the landing page. It also sets `app.secret_key` (required for sessions to work) and updates `base.html` to show a personalised navbar when a user is logged in.

## Depends on
- Step 1 — database setup: `get_db()` must be working and the `users` table must exist.

## Routes
- `GET /register` — render the registration form — public
- `POST /register` — validate form data, create user, start session, redirect to landing — public

## Database changes
No database changes. The `users` table from Step 1 (`id`, `name`, `email`, `password_hash`, `created_at`) is sufficient.

## Templates
- **Create:** none — `templates/register.html` already exists with the form and `{{ error }}` block
- **Modify:**
  - `templates/base.html` — update the navbar: when `session.user_id` is set, replace the "Sign in / Get started" links with the user's name and a "Sign out" link pointing to `url_for('logout')`

## Files to change
- `app.py` — add `app.secret_key`, new imports (`request`, `redirect`, `url_for`, `session` from flask; `generate_password_hash` from werkzeug), add POST method to `/register` route and implement the handler
- `templates/base.html` — conditional navbar based on `session.user_id`

## Files to create
None.

## New dependencies
No new dependencies. `werkzeug.security` is already installed via Flask.

## Rules for implementation
- No SQLAlchemy or ORMs
- Parameterised queries only — never use string formatting in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- `app.secret_key` must be set before any session usage; use a hardcoded dev string (e.g. `"spendly-dev-secret"`)
- Session must store at minimum: `session["user_id"]` and `session["user_name"]`
- On duplicate email, catch `sqlite3.IntegrityError` and re-render the form with `error="An account with that email already exists."`
- Redirect after successful registration goes to `url_for('landing')`
- Server-side validation required: name non-empty, password ≥ 8 characters; re-render with an `error` message on failure

## Definition of done
- [ ] `GET /register` still renders the form without errors
- [ ] Submitting the form with valid data inserts a new row in `users` with a hashed password (verify with a SQLite browser or `seed_db` guard logic)
- [ ] After successful registration the browser redirects to the landing page
- [ ] `session["user_id"]` is set immediately after registration
- [ ] The navbar shows the user's name instead of "Sign in / Get started" when logged in
- [ ] Submitting with an already-registered email re-renders the form with an error (no 500 crash)
- [ ] Submitting with a password shorter than 8 characters re-renders the form with an error
- [ ] Submitting with an empty name re-renders the form with an error
- [ ] Passwords are stored as a hash — plain text is never saved to the database
- [ ] `python app.py` starts without errors
