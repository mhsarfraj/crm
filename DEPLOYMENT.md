# Deployment (Windows, no installs)
1. Extract the ZIP. 2. Create a Google Sheet (sheets.google.com) → name it "CRM Pro DB".
3. In the sheet: **Extensions → Apps Script**. Delete the sample code, paste all of `google-apps-script/Code.gs`, save.
4. **Project Settings (gear) → Script properties → Add**: `ADMIN_EMAIL` = your email, `ADMIN_PASSWORD` = a strong password (10+ chars).
   (Skip them to get a random password printed in the execution log instead.)
5. Select function **setup** → **Run** → authorize (Advanced → continue). Open **Execution log**; confirm "Setup complete". Delete `ADMIN_PASSWORD` afterwards.
6. **Deploy → New deployment → Web app**: Execute as **Me**, Who has access **Anyone**. Deploy and copy the URL ending in `/exec`.
   (After later code edits use **Deploy → Manage deployments → Edit → New version**.)
7. Open `js/config.js` and replace `PASTE_YOUR_WEB_APP_URL_HERE` with that URL.
8. **Local test**: open the folder in VS Code and use a static server, or just double-click `index.html` (works from file://).
   **Online**: push the folder to a GitHub repo → Settings → Pages → deploy from branch `main`, root.
9. Sign in, create users under **Users**, add data.

## Troubleshooting
- "Cannot reach the backend": wrong URL, deployment not set to *Anyone*, or not redeployed as a new version.
- "Spreadsheet not initialised": run `setup()`. "Session expired": sessions last 6 hours.
- Use a standalone script? Add script property `SPREADSHEET_ID`.
- Backend errors: Apps Script → Executions.
