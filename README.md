# Flocking — shankaracircle.com

An ambient [boids](https://www.red3d.com/cwr/boids/) flock (Reynolds: separation ·
alignment · cohesion) under a sky that **follows the real time of day in Pacific
(`America/Los_Angeles`)** — deep night → pre-dawn → serene dawn → bright midday →
golden hour → vivid sunset → dusk → starlit night, with a sun/moon arc, fading
stars, drifting clouds, and night motion-trails that vanish in daylight.

This is the **homepage for `www.shankaracircle.com`** (primary) and
`shankaracircle.com` (apex, redirects to `www`). It is a single self-contained
static file — no build, no dependencies.

> Unrelated: `game-lab` stays on `games.bodymindcafe.com` as its own separate
> Vercel project.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site — markup, styles, and the canvas simulation |
| `vercel.json` | Static config (clean URLs, revalidate HTML) |

## Run locally

Pure static — any of these work:

```bash
npx serve .            # → http://localhost:3000
python3 -m http.server 8080
```

(You can also just open `index.html` directly — Pacific time still resolves
correctly from the browser.)

## Built-in config panel

No redeploy needed to tune the flock — open the panel in the live site:

- Click the **`⚙`** (bottom-right) or press **`H`**
- **Live Pacific time** toggle — turn off to scrub a **time-of-day** slider and
  preview any hour (dawn / sunset / night) instantly
- Sliders: **Birds** (count), **Cohesion** (how tightly they pull together),
  **Alignment**, **Separation** (personal space — lower = closer), **Speed**
- **Space** pause · **R** reseed · **F** fullscreen
- Move the cursor — the flock flees it like a hawk · **click** to scatter
- Cursor & chrome auto-hide after 4 s idle (screensaver / kiosk friendly)

The shipped defaults — speed `1.0`, cohesion `0.25`, alignment `1.0`,
separation `3.0`, **5000 birds** — give a loose, widely-spaced but
directionally-aligned drift. The Birds slider ranges 2000–10000 and Separation
0–5. The renderer batches birds into 3 depth layers (3 draw calls/frame) and
caps neighbour sampling, so it stays fast at scale.

## Deploy to Vercel

Already wired: repo **`ceprateek/shankaracircle`** is connected to the Vercel
project **`shankaracircle`**, so **every push to `master` auto-deploys
production**.

```bash
git add -A && git commit -m "..." && git push   # auto-deploys
# or manual:
vercel --prod
```

Live (pre-domain) test URL: **https://shankaracircle.vercel.app**

### Point the domain at it

In the Vercel project → **Settings → Domains**, add **both**:

- `www.shankaracircle.com`  ← set as **primary** (canonical)
- `shankaracircle.com` (apex) ← Vercel offers **Redirect to `www.shankaracircle.com`**; accept it

Then add the DNS records Vercel shows at your registrar:

| Host | Type | Value |
|---|---|---|
| `www` | `CNAME` | `cname.vercel-dns.com` |
| `@` (apex) | `A` | `76.76.21.21` |

(If your DNS supports `ALIAS`/`ANAME` for the apex, Vercel will suggest that
instead of the `A` record.) SSL is issued automatically once DNS propagates.
