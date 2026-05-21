# EPIC-AMP — Vercel Deployment

Static front-end for the EPIC-AMP antimicrobial peptide classifier & MIC predictor.
The machine-learning models stay on Hugging Face — this site just calls them from the browser.

## What's in this folder

```
epic-amp/
├── index.html        ← the entire app (HTML + CSS + JS in one file)
├── vercel.json       ← static-hosting config (caching + security headers)
└── README.md         ← this file
```

## ⚠️ Before you deploy: add your asset files

`index.html` references these files by relative path. **Copy them into this folder**
(next to `index.html`) or they'll 404 once live:

| File                | Used for                                  | Has fallback? |
|---------------------|-------------------------------------------|---------------|
| `logo.png`          | header logo + PDF header                  | no            |
| `favicon.png`       | browser tab icon                          | no            |
| `shap.png`          | SHAP plot (Model Metrics tab + PDF)       | yes (text)    |
| `image2.png`        | PDF report banner                         | yes (drawn)   |
| `demo.mp4`          | the "Demo" modal video                    | no            |
| `team-member-1.jpg` | About → Ali Abdalhalim                    | yes (avatar)  |
| `team-member-2.jpg` | About → Ahmed Amr                         | yes (avatar)  |
| `team-member-4.jpg` | About → Prof. Eman Badr                   | yes (avatar)  |

Items marked "yes" degrade gracefully if missing; the others will simply show a
broken-image / empty box.

## Deploy — pick one

### Option A — drag & drop (no tools)
1. Go to https://vercel.com/new
2. Drag this whole `epic-amp` folder onto the page.
3. Vercel auto-detects a static site — click **Deploy**.

### Option B — Vercel CLI
```bash
npm i -g vercel
cd epic-amp
vercel          # preview deploy; follow the prompts
vercel --prod   # promote to production
```

### Option C — Git
Push this folder to a GitHub/GitLab repo, then **Import Project** in Vercel.
No build command, no framework preset — it's pure static.

## Notes

- **No build step.** Vercel serves `index.html` directly. The `vercel.json` only
  sets caching for media files and a couple of security headers.
- **Predictions still run on Hugging Face** (`nonzeroexit/AMP-Classifier` and
  `nonzeroexit/AMP-Mailer`). Nothing about the model moved — the page connects to
  those Spaces from the visitor's browser via the `@gradio/client` ES module.
- **If a Space is private**, set `HF_TOKEN` in the CONFIG block near the top of the
  `<script>` in `index.html`. For a public Space, leave it `null`.
- **Cold starts:** if a Space has been idle it may take ~30–60s to wake on the first
  request; the UI already shows a retry message for this.
