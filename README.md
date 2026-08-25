# Studio Tracker — Supabase + GitHub Pages

This version uses Supabase Auth and a shared Postgres table. Colleagues sign in with email/password, and all authenticated users read/write the same tracker state.

## 1. Create the Supabase project
Create a project at https://supabase.com/

## 2. Configure authentication
In Supabase Dashboard → Authentication → Providers, enable Email.

Create your office users, or allow controlled sign-up and then have colleagues confirm their email.

## 3. Create the database
Open SQL Editor and run the complete `supabase.sql` file in this package.

The SQL enables Row Level Security and grants tracker access only to authenticated users. Supabase recommends RLS for exposed tables and using `auth.uid()` in policies.

## 4. Add the Supabase URL and publishable key
Open `index.html` and find:

    const SHARED_CONFIG = {
      url: 'YOUR_SUPABASE_URL',
      anonKey: 'YOUR_SUPABASE_PUBLISHABLE_KEY',

Replace those placeholders with the project URL and the browser-safe publishable/anon key from Supabase.

NEVER put the `service_role` key in this file. It must remain server-side.

## 5. Publish with GitHub Pages
Upload these files to the root of a GitHub repository:

- `index.html`
- `.nojekyll`
- `supabase.sql`
- this README

GitHub → repository Settings → Pages → Deploy from branch → `main` → `/ (root)`.

## 6. Configure Supabase Auth redirect URL
After GitHub Pages gives you the site URL, add that URL under:
Supabase Dashboard → Authentication → URL Configuration → Redirect URLs.

Example:
https://YOUR-GITHUB-USERNAME.github.io/studio-tracker/

## What is shared
Projects, milestones, tasks, revisions, events and team data are stored in Supabase and are visible to authenticated colleagues.

## Live updates
The app subscribes to Supabase Realtime changes on `studio_state`, so updates made by one colleague are reflected for other logged-in colleagues.

## Security
Only authenticated users can access the tracker table through RLS. Do not disable RLS and do not expose a service-role key in the browser.
