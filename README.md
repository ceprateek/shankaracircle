# Flocking

An ambient [boids](https://www.red3d.com/cwr/boids/) flock (Reynolds: separation ·
alignment · cohesion) under a sky that **follows the real time of day in Pacific
(`America/Los_Angeles`)** — deep night → pre-dawn → serene dawn → bright midday →
golden hour → vivid sunset → dusk → starlit night, with a sun/moon arc, fading
stars, drifting clouds, and night motion-trails that vanish in daylight.

Inspired by the flocking on shankaracircle.com; algorithm after `rystrauss/boids`.

This is the **homepage for `bodymindcafe.com`** (apex + `www`). It is a single
self-contained static file — no build, no dependencies.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site — markup, styles, and the canvas simulation |
| `vercel.json` | Static config (clean URLs, revalidate HTML) |

## Run locally

It's pure static — any of these work:

```bash
# from this folder
npx serve .          # → http://localhost:3000
# or
python3 -m http.server 8080
```

(You can also just open `index.html` directly — Pacific time still resolves
correctly from the browser.)

## Controls

- **Move the cursor** — the flock flees it like a hawk · **click** to scatter
- `⚙` (bottom-right) or **`H`** — settings panel
- In the panel: toggle **Live Pacific time** off to scrub a **time-of-day**
  slider (preview dawn / sunset / night instantly); sliders for bird count,
  cohesion, alignment, separation, speed
- **`Space`** pause · **`R`** reseed · **`F`** fullscreen
- Cursor & chrome auto-hide after 4 s idle (screensaver / kiosk friendly)

## Deploy to Vercel

Same pattern as `game-lab` → `games.bodymindcafe.com`, but this is the **root
domain**.

### 1. Create the project

**Via GitHub (recommended — auto-deploys on push):**

```bash
git init && git add -A && git commit -m "Flocking homepage"   # already done locally
gh repo create bodymindcafe --public --source=. --push          # or create the repo in the UI
```

Then on vercel.com → **Add New… → Project** → import the repo.
Framework preset: **Other**. Build command: *(none)*. Output dir: *(leave blank
/ root)*. Deploy.

**Or via CLI:**

```bash
npm i -g vercel
vercel        # link/create project
vercel --prod # ship it
```

### 2. Point the root domain at it

In the Vercel project → **Settings → Domains**, add **both**:

- `bodymindcafe.com`   ← set as the primary (canonical)
- `www.bodymindcafe.com` ← Vercel will offer **Redirect to `bodymindcafe.com`**; accept it

Then add the DNS records Vercel shows you at your registrar:

| Host | Type | Value |
|---|---|---|
| `@` (apex) | `A` | `76.76.21.21` |
| `www` | `CNAME` | `cname.vercel-dns.com` |

(If your DNS supports `ALIAS`/`ANAME` for the apex, Vercel will suggest that
instead of the `A` record — either is fine.) SSL is issued automatically once
DNS propagates.

> `games.bodymindcafe.com` (game-lab) is a separate Vercel project and is
> unaffected — it keeps its own `games` `CNAME`.
