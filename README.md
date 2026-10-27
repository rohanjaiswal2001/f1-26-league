# F1 26 League: 2026 Season 1

A static dashboard (one `index.html`) backed by Supabase. Anyone with the link can view; adding or editing races needs a passcode.

## Publish on GitHub Pages (about 3 minutes)

1. On github.com, click **New repository**. Name it e.g. `f1-26-league`. Set it to **Public** (free GitHub Pages needs a public repo). Create it.
2. Click **Add file → Upload files**, drag in `index.html`, and click **Commit changes**.
3. Go to **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, Branch to **main**, folder **/ (root)**, then **Save**.
4. After about a minute the site is live at `https://<your-github-username>.github.io/f1-26-league/`. Send that link to your friends.

## Passcode

The passcode is not stored in this repo (it lives hashed in Supabase), and this repo is public, so don't write it here. Ask Rohan for it. To change it, open the Supabase SQL editor for the `f1-26-league` project and run:

```sql
update public.league_settings
set passcode_hash = extensions.crypt('your-new-passcode', extensions.gen_salt('bf'))
where id = 1;
```

Browsers remember the passcode after the first unlock. If you change it, people are asked again.

## Day to day

- **Log a race:** unlock, click Log a race, pick the track, tap the winner, save. Everyone's page updates live.
- **Fix a mistake:** click Edit on any row in the race log, then change it or delete it.
- **Driver changes:** Manage drivers lets you edit names, 2 to 4 letter short codes (RJ, JAI, VIJ), numbers and colors, and add the 4th player when they join.

## Notes

- The Supabase URL and anon key inside `index.html` are meant to be public. Safety comes from row-level security: the public can only read, and every write goes through a function that checks the passcode.
- Supabase free projects pause after about a week of no activity. If the page says it can't reach the database, open the project in the Supabase dashboard and click Restore.
- Season 2 plan: add a `position` column to `races` (or a `results` table) and a points table. Season 1 rows stay as they are, since `races.season` already separates seasons.
