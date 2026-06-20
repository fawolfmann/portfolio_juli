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

## Deploy — Cloudflare Pages

### Option A: Git integration (recommended)
1. Push this repo to GitHub/GitLab.
2. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git**.
3. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `/`
4. Deploy. Every push to the production branch publishes automatically.

### Option B: Direct upload via Wrangler
```bash
npm install -g wrangler
wrangler pages deploy . --project-name=julia-reisin-portfolio
```

The `_headers` file is applied automatically by Cloudflare Pages.

## Editing notes

- All copy, styles, and scripts live in `index.html`.
- To add or swap a work video: drop the file in `assets/`, generate a poster frame
  (e.g. `ffmpeg -ss 1 -i assets/clip.mp4 -frames:v 1 -q:v 3 assets/posters/clip.jpg`),
  and add a `.proof-card` block in the relevant work item.
- Keep each video under **25 MiB** (Cloudflare Pages per-file limit).
