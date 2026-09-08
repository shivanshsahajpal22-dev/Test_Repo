# MidnightBazaar — FICTIONAL Demo Target for OnionEye

A make-believe marketplace **generated purely as an OnionEye scanner test target**.
Every handle, wallet, email, PGP block, and onion address on these pages is
fabricated. There is no real market, vendor, or key.

## What this folder is

GitHub publishes a project repository's **`/docs` folder** directly as a web
site — no build step, no workflow required. When this repo is hosted on GitHub
Pages, the URL layout is:

| Page | Served URL |
|---|---|
| Home (market overview) | `https://sudo-problems.github.io/OnionEye/` |
| Listing (vendor page) | `https://sudo-problems.github.io/OnionEye/listing/` |
| Forum thread | `https://sudo-problems.github.io/OnionEye/thread/espresso-review.html` |
| Contact desk | `https://sudo-problems.github.io/OnionEye/contact/` |

Internal links are **absolute** on purpose: the OnionEye crawler only follows
extracted absolute `http(s)://` URLs, so sub-pages get discovered and depth-2
crawling exercises the BFS frontier.

## How to host (one-time)

1. Commit + push this repo to GitHub (`Sudo-Problems/OnionEye`).
   ```bash
   git add docs/
   git commit -m "feat: add fictional MidnightBazaar demo target site"
   git push
   ```
2. GitHub → **Settings → Pages** → Source: **Deploy from a branch**
   - Branch: `main` (or your default branch), folder: **`/docs`** → Save.
3. Wait ~1 minute; the site is live at
   `https://sudo-problems.github.io/OnionEye/`.

> If the repo lives under a different account/URL, replace every
> `https://sudo-problems.github.io/` prefix in the four HTML files (grep it —
> it appears in `<nav>` links and the CSS `<link>` tag).

## What scanning it demonstrates

The pages are deliberately wired so OnionEye produces a **connected** entity graph:

- **3 actor handles** extracted from `<span class="username">` + `@mention`:
  `DarkVault`, `CipherMist`, `NightOwl`, plus `QuantumQuill` on the contact page.
- **Shared escrow BTC wallet** `1Je9pEjppgGYcPLB24ye9L6uw3wq` on both the home
  page (`DarkVault`) and the listing page (`CipherMist`)
  → persona link confidence **0.80**.
- **Shared support email** `support@darkvault.pro` on home (`DarkVault`) and the
  thread (`NightOwl`) → persona link confidence **0.65**.
- **Shared PGP key block** on listing (`CipherMist`) and thread (`NightOwl`)
  → persona link confidence **0.90**.
- **Onion v3 address** + **`nginx/1.24.0` server banner** → infra indicator nodes.
- `QuantumQuill` shares nothing → isolated leaf node (proves both dense clusters
  and standalone nodes render).

The person-link triangle `DarkVault — CipherMist — NightOwl` is exactly the
"linkages" output the scanner could not produce for the GitHub-Pages portfolio
page, because that page had **zero extractable identifiers** — this site fixes
that.

## Scan it

```bash
curl -s -X POST http://localhost:8080/api/v1/investigations \
  -H 'Content-Type: application/json' \
  -d '{
    "analyst_id": "demo-analyst",
    "authorization_ref": "AUTH-DEMO-0001",
    "target_type": "clearnet_site",
    "target_value": "https://sudo-problems.github.io/OnionEye/",
    "target_host": "sudo-problems.github.io",
    "seed_terms": ["MidnightBazaar", "DarkVault", "CipherMist"],
    "max_depth": 3
  }'
```

Then check the graph: `GET http://localhost:3000/api/v1/dashboard/graph`.