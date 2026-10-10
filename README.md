# Student Activity & Achievement Portal

A replacement for the Google Form **"Student activity and achievement Data 1st July 2025 to 30th June 2026"** (K J Somaiya School of Engineering).

- **Student form** at `/`: the same questions as the Google Form, a searchable activity list, a DD-MM-YYYY date picker, PDF upload with preview and progress, an `@somaiya.edu` email check, "remember my details", and a reference number (e.g. `SA-00042`) after submitting.
- **Admin console** at `/admin` (password protected):
  - **Overview:** totals, charts by month, level, class and activity type.
  - **Responses:** search, filters, sorting, inline PDF viewer (no download needed), bulk export and delete.
  - **Students:** responses grouped by Year → Branch → Roll No.
  - **Settings:** open/close the form, deadline and period, edit the activity options, email domain.
- **Exports:**
  - **Excel (.xlsx):** Responses, Summary and Students sheets, with a clickable link to each proof.
  - **ZIP:** the Excel file plus every proof PDF, organised as `Proofs/<Branch>/<Year>/…`.

## Run locally

Requires Node.js 20+.

```bash
npm install
cp .env.example .env.local   # then set ADMIN_PASSWORD
npm run dev                  # http://localhost:3000  (admin: /admin)
```

For production on your own machine or server: `npm run build && npm start`.

With no Supabase keys, everything is stored locally: an SQLite database in `data/portal.db` and the PDFs in `data/uploads/`. **Back up the `data/` folder.**

## Deploy on Vercel + Supabase

1. Create a Supabase project. In the **SQL editor**, run `supabase/schema.sql`.
2. Import this repo in Vercel and add these environment variables:
   - `ADMIN_PASSWORD`: the admin password.
   - `SESSION_SECRET`: a long random string.
   - `SUPABASE_URL`: from Project Settings → API.
   - `SUPABASE_SERVICE_ROLE_KEY`: from Project Settings → API. Keep it secret.
   - `MAX_UPLOAD_MB`: `50` on Supabase's free plan, which caps files at 50 MB. Up to `100` on paid plans; raise the bucket limit too.
3. Deploy. PDFs upload directly from the browser to Supabase Storage, so Vercel's request-size limit doesn't apply.

## Useful scripts

- `npm run lint`
- `node scripts/smoke-test.mjs http://localhost:3000 <admin-password>`: an end-to-end API check (upload, submit, admin login, PDF preview, Excel and ZIP export). It creates one sample response.
