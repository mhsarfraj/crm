# Security notes
- The Apps Script URL must be public ("Anyone") for a static site to call it, so security is enforced in code: every action except `health`/`login` needs a server-side session token (CacheService, 6h); role and status are re-checked from the Users sheet on every request, so disabling a user takes effect immediately.
- Passwords: random per-user salt + 2000 rounds SHA-256. This is weaker than bcrypt/argon2 (unavailable in Apps Script); use long passwords. Login locks an email for 15 min after 5 failures.
- Server-side validation, enum checks, formula-injection neutralisation on every write; the UI escapes all output.
- Admin-only: user management and audit log. Sales users can read/write CRM records (no per-record ownership restrictions).
- Session token lives in sessionStorage (cleared on tab close). No secrets are in frontend code.
- Pending/limits: no password-reset flow, no CSRF token (not cookie-based, so not applicable), Apps Script quotas apply, and the whole sheet is read per request (fine for thousands of rows, not hundreds of thousands).
- **Your uploaded ZIP contained a `.env` file with a SECRET_KEY and database URL. Treat them as exposed: rotate the MySQL password and never share that file.** It is not included in this package.
