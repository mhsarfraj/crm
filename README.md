# CRM Pro — HTML/CSS/JS + Google Apps Script + Google Sheets

Migrated from the Flask/MySQL starter. No Python, Node, MySQL or build step is needed.

## Structure
- `index.html`, `css/app.css`, `js/config.js` (API URL), `js/app.js` (UI, router, API service)
- `google-apps-script/Code.gs` (backend: auth, validation, CRUD, dashboard, audit)
- `DEPLOYMENT.md`, `SECURITY.md`

## What was kept / changed from the original project
| Original (Flask/MySQL) | Now |
|---|---|
| users (hash, role admin/sales/manager) | `Users` sheet, salted iterated SHA-256, roles admin/sales |
| leads, customers, deals, tasks, activities | Sheets with UUID ids, archive flag, timestamps; full create/edit/archive/restore |
| audit_logs | `AuditLogs` sheet, written on every login/create/update/archive/move |
| dashboard (counts, pipeline, lead statuses) | `dashboard` action: KPIs, weighted pipeline, win/conversion rate, charts |
| pipeline.html | Deals board with stage moves (select, persisted; no duplicates) |
| contacts.html, reports.html, `*_backup` folders, `.venv` | Not migrated (see "Not included") |

## API contract (POST, body is JSON sent as text/plain)
Request `{action, token, ...}` → `{ok:true, data}` or `{ok:false, code, error}`.
Public: `health`, `login{email,password}`. Authenticated: `logout`, `lookups`, `list{entity,archived?}`, `save{entity,record,force?}`,
`archive{entity,id,restore?}`, `moveDeal{id,stage,loss_reason?}`, `convertLead{id}`, `dashboard`.
Admin only: `userList`, `userSave{record}`, `auditList`. Entities: leads, customers, deals, tasks, activities.
Error codes: AUTH, AUTH_FAILED, LOCKED, FORBIDDEN, VALIDATION, DUPLICATE, NOT_FOUND, SETUP, BAD_REQUEST, SERVER.

## Dashboard calculations
Pipeline value = sum(value) of deals not Won/Lost. Weighted = sum(value × probability/100) over the same deals.
Lead conversion % = Converted leads ÷ all active leads. Win rate % = Won ÷ (Won + Lost). Shown as "—" when the denominator is 0.
Overdue = open tasks with due date before today. Monthly revenue = Won deals grouped by expected-close month.

## Not included (be aware)
Contacts module, a dedicated Reports page (date-range filters, printable layouts, per-salesperson reports), notes/documents (Drive),
notifications, password-reset email, CSV import, bulk actions, drag-and-drop (use the stage select), Lucide icons (unicode glyphs used),
configurable pipeline stages and sample-data generator. Stages are constants in `Code.gs` (`STAGES`).
