# Julia Reisin — Portfolio

Editorial, single-page portfolio for Julia Reisin — AI-powered content creator &
training/learning specialist. Static site, no build step, deployed on Cloudflare Pages.

## Stack

- **Plain HTML/CSS/JS** in a single `index.html` — no framework, no build.
- **Type:** Fraunces (display), Inter (body), IBM Plex Mono (labels) via Google Fonts.
- **Design:** editorial / print-magazine direction — paper + ink + terracotta, light & dark themes.
- **Media:** self-hosted, web-optimized H.264 video (`+faststart`) and JPEG, all under Cloudflare's 25 MiB/file limit.

## Structure

```
.
├── index.html        # the entire site (markup + styles + scripts inline)
├── favicon.svg       # JR monogram
├── _headers          # Cloudflare Pages: security headers + asset caching
├── .gitignore
├── assets/
│   ├── *.mp4         # work-sample videos (optimized)
│   ├── campaing.jpeg # listicle creative
│   └── posters/      # video poster frames (*.jpg)
└── _archive/         # local backups, NOT deployed (gitignored)
```

## Local preview

No tooling required — serve the folder with any static server:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

(Opening `index.html` via `file://` also works, but a server matches production
behavior for the videos and headers.)

## Deploy — Cloudflare Pages (GitHub Actions)

Every push to `main` deploys automatically via
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml), which runs
`wrangler pages deploy . --project-name=portfolio-juli`.

**One-time setup** — in the GitHub repo, create an **Environment** named `CLOUDFLARE`
(Settings → Environments) and add two secrets to it:

| Secret | Where to get it |
|--------|-----------------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare dashboard → My Profile → API Tokens → *Create Token* → **Edit Cloudflare Workers** (or a custom token with `Account › Cloudflare Pages › Edit`). |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare dashboard → Workers & Pages → right sidebar **Account ID**. |

The first run creates the `portfolio-juli` Pages project automatically. The `_headers`
file (security headers + asset caching) is applied by Cloudflare on every deploy.

### Manual deploy (optional)
```bash
npm install -g wrangler
wrangler pages deploy . --project-name=portfolio-juli
```

## Editing notes

- All copy, styles, and scripts live in `index.html`.
- To add or swap a work video: drop the file in `assets/`, generate a poster frame
  (e.g. `ffmpeg -ss 1 -i assets/clip.mp4 -frames:v 1 -q:v 3 assets/posters/clip.jpg`),
  and add a `.proof-card` block in the relevant work item.
- Keep each video under **25 MiB** (Cloudflare Pages per-file limit).
