# CIT HOD Office Desk: Supabase + GitHub + Vercel

Roles: staff, class tutor, admin (HOD), super admin.

Folder contents
- api/index.js ........ server (Vercel serverless function)
- public/index.html ... the website
- package.json, vercel.json
- supabase-schema.sql . ready-made database tables for Supabase
- .env.example ........ the four settings Vercel needs

## 1. GitHub
1. github.com > New repository > name `cit-desk` > Private > Create.
2. Click "uploading an existing file" and drag in: the `api` folder, the `public` folder, package.json and vercel.json.
   (If a folder is skipped: Add file > Create new file, type `api/index.js` as the name, paste the code, Commit. Repeat for `public/index.html`.)
3. Commit. The repository front page must show api, public, package.json, vercel.json.

## 2. Supabase
1. supabase.com > New project > name `cit-desk`. Database password: letters and numbers only (avoid @ : / # ?). Save it. Pick the nearest region.
2. SQL Editor > New query > paste all of supabase-schema.sql > Run. You should see 11 tables in Table Editor.
3. Click Connect > choose "Transaction pooler" (port 6543) > copy the string > replace [YOUR-PASSWORD] (with the brackets) by your password.
   Do not use the "Direct connection" string: Vercel cannot reach it.

## 3. Vercel
1. vercel.com > Add New > Project > import `cit-desk` > Framework Preset: Other.
2. Environment Variables (see .env.example): DATABASE_URL, AUTH_SECRET, SUPERADMIN_USER, SUPERADMIN_PASS.
3. Deploy > Visit.
4. Settings > Functions: set Function Region near your Supabase region, then redeploy.
5. Settings > Deployment Protection: use Standard Protection (or off) so staff do not need a Vercel login.

## 4. First use
Sign in as the super admin > Staff & accounts > create your Admin (HOD) account, then staff and class tutors > reset the super admin password.

## 5. Updating later
Edit the file on GitHub (pencil icon), paste the new code, Commit. Vercel redeploys by itself. Press Ctrl+F5.

## Troubleshooting
- "Server not configured": a Vercel variable is missing or misspelled. Fix and redeploy.
- Log shows ENOTFOUND / ETIMEDOUT / ENETUNREACH: you used the Direct connection string. Use the Transaction pooler string.
- "password authentication failed": wrong password or brackets left in the string.
- "Tenant or user not found": copy the string again; it must start with postgres.<project-ref>.
- 404 on /api: the api folder is missing in GitHub.
- "Access request" page from Vercel: change Deployment Protection (step 3.5).
- "Too many wrong attempts": wait 15 minutes or reset the password.
- Site worked, then "Server error" after a quiet week: Supabase paused the free project. Click Restore project.
- Optional: if a certificate error appears, set a Vercel variable DB_CA with the Supabase CA certificate text.
