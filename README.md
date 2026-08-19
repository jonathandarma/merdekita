# MERDEKITA

Platform aksi kemerdekaan: event, challenge, volunteer, komunitas, UMKM, passport, XP, badge, leaderboard, dan rekomendasi.

## Architecture

Next.js App Router is the frontend; UI components access application data through `/api/*` only. Route handlers form the backend boundary with Zod validation and consistent `{ success, data | error }` responses. Supabase provides Auth, PostgreSQL, Storage, and RLS in production. The local activity adapter deliberately supports visual/demo use before Supabase is connected; replace it with Supabase queries for persistent deployment.

## Run locally

1. Copy `.env.example` to `.env.local` and fill Supabase values.
2. Run `npm install` then `npm run dev`.
3. Apply both files in `supabase/migrations/` in numeric order, then run `supabase/seed.sql` in the Supabase SQL editor.

Optional integrations: Mapbox (`NEXT_PUBLIC_MAPBOX_TOKEN`), OpenWeather (`OPENWEATHER_API_KEY`), and OpenAI (`OPENAI_API_KEY`). Missing optional keys show graceful fallbacks; recommendation uses deterministic scoring.

## Core REST APIs

- `GET /api/activities?kind=&q=`
- `GET /api/activities/:id`
- `POST /api/events/:id/join`
- `POST /api/recommendations`

The migration defines profiles, events/participants, challenges/submissions, XP transactions, badges, communities, volunteer opportunities, UMKM, favorites, and notifications, including uniqueness constraints and baseline RLS. Workflow RPC functions atomically prevent duplicate registration and update participation, XP ledger, and notifications.

## Deploy

Set every environment variable in Vercel, connect the Supabase project, apply migrations, and deploy the repository. Never expose `SUPABASE_SERVICE_ROLE_KEY` to client-side code.
