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

## What's on the page

- **Standings podium:** the top 3 stand on a 2-1-3 podium as bobbleheads: photo head on a spring, race suit in their team color with their driver number. Heads bob on their own; tap one to make it wobble. A 4th driver (or more) is listed under the podium. Ties are broken by who reached that win count first.
- **Next race:** the first track on the 2026 calendar with no result logged yet. Log a result and the card moves on to the next unraced track, skipping any you've already done. Log this race opens the form with that track selected.
- **Light and dark mode:** follows the device setting by default. The toggle in the toolbar overrides it and is remembered on that device. Jai's black color shows as white in dark mode and black in light mode (any near-black or near-white driver color flips the same way).
- **Wins over time:** the x axis shows a flag and track name for each race. Hover a race column for the winner.
- **Photos:** driver photos are in `img/` (`rohan.jpg`, `jai.jpg`, `bhavya.jpg`) and the header banner is `img/group.jpg`. The repo is public, so anyone with the link can see these.
- **Adding a photo for a new driver:** put the image in `img/`, then in the Supabase SQL editor run `update public.drivers set photo = 'img/name.jpg' where name = 'Driver Name';`. Until then they show their initials.

## Notes

- The Supabase URL and anon key inside `index.html` are meant to be public. Safety comes from row-level security: the public can only read, and every write goes through a function that checks the passcode.
- Supabase free projects pause after about a week of no activity. If the page says it can't reach the database, open the project in the Supabase dashboard and click Restore.
- Season 2 plan: add a `position` column to `races` (or a `results` table) and a points table. Season 1 rows stay as they are, since `races.season` already separates seasons.
