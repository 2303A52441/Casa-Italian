# Casa Italiana — Hanamkonda (Static + Optional DB)
Aesthetic Italian café site tailored for Hanamkonda. Deploys on Vercel as static, with optional serverless API + Postgres for reservations.

## Quick Deploy
1) Create a **public GitHub** repo and upload everything in this folder (including `api/`).
2) On **vercel.com → New Project → Import** your repo.
3) Settings: Framework Preset **Other**, Build Command **(empty)**, Output Directory **/**.
4) Deploy — your site goes live. The form will work once you add the DB env var below.

## Enable Database (optional)
1) Create a Postgres DB (Neon / Supabase / Vercel Postgres). Copy connection string as `POSTGRES_URL`.
2) Create table:
```sql
create table if not exists reservations (
  id bigserial primary key,
  name text not null,
  date date not null,
  notes text,
  created_at timestamptz not null default now()
);
```
3) In Vercel → Project → Settings → Environment Variables: add `POSTGRES_URL`.
4) Redeploy (or push any change). Test `POST /api/reserve` via the form.
5) (Temporary check) Open `/api/reservations` on your domain to list rows.

## Customize
- Name, headings, menu, prices → `index.html`
- Colors/typography → `styles.css` (accent `#b35f3f`)
- Instagram link → replace `https://instagram.com/your-handle`
- Maps link → change the query to your precise address
- Add your photos in `images/` and keep the same filenames or update `src` in HTML
