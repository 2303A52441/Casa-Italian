# Casa Italiana — Auth + Cart (Supabase) + Reservations (Postgres via Vercel API)

## What this adds
- Pages: `register.html`, `login.html`, `profile.html`, `settings.html`, `cart.html`
- Supabase Auth (email/password) with client SDK
- Cart stored in Supabase table with Row Level Security (RLS)
- Reservation form saved via Vercel Serverless `/api/reserve` (optional Postgres)

## Setup — Supabase (Auth + Cart)
1) Create a **Supabase** project → get **Project URL** and **anon key**.
2) In `js/config.js`, set:
```js
window.SUPABASE_URL = "https://YOUR-PROJECT.supabase.co";
window.SUPABASE_ANON_KEY = "YOUR-ANON-KEY";
```
3) In Supabase **SQL Editor**, run:
```sql
create table if not exists cart_items (
  id bigserial primary key,
  user_id uuid not null references auth.users(id) on delete cascade,
  item_name text not null,
  price numeric not null,
  qty integer not null default 1,
  created_at timestamptz not null default now()
);

alter table cart_items enable row level security;

create policy "Users can manage own cart"
on cart_items
for all
to authenticated
using (user_id = auth.uid())
with check (user_id = auth.uid());
```
4) In **Authentication → Settings**, enable email/password signups.

## Optional — Postgres for reservations
Create a Postgres DB and set `POSTGRES_URL` in Vercel env, then run:
```sql
create table if not exists reservations (
  id bigserial primary key,
  name text not null,
  date date not null,
  notes text,
  created_at timestamptz not null default now()
);
```

## Deploy
- Upload everything to a **public GitHub** repo.
- Vercel → New Project → Import repo → Framework **Other**, Build **(empty)**, Output **/** → Deploy.
- Add `POSTGRES_URL` env var only if you want the reservation form to save to DB.

## Use
- Register/Login.
- On Home, click **Add to cart** (requires login) → saved in `cart_items`.
- **Cart** shows your items and total; form can insert demo items.
- **Settings** lets you change name and password.
