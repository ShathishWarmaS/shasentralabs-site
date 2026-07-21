# Deployment notes — shasentralabs.com

Live site: https://shasentralabs.com
Repo: https://github.com/ShathishWarmaS/shasentralabs-site
Host: GitHub Pages (free), branch `main`, `/ (root)`
Cost: ₹0

## How to update the site

Edit `index.html`, then:

```sh
git add -A && git commit -m "your message" && git push
```

GitHub Pages redeploys in ~30–60s. Browsers cache aggressively —
use a hard reload (Cmd+Shift+R) to see changes.

## DNS (at GoDaddy)

Web records — point the apex at GitHub Pages:

| Type | Host | Value |
|---|---|---|
| A | `@` | 185.199.108.153 |
| A | `@` | 185.199.109.153 |
| A | `@` | 185.199.110.153 |
| A | `@` | 185.199.111.153 |
| CNAME | `www` | shathishwarmas.github.io |

Mail records — **do not touch**, these keep Zoho email working:

| Type | Host | Value |
|---|---|---|
| MX | `@` | mx.zoho.in (10), mx2.zoho.in (20), mx3.zoho.in (50) |
| TXT | `@` | v=spf1 include:zoho.in ~all |
| TXT | `zoho._domainkey` | DKIM key |
| TXT | `_dmarc` | v=DMARC1; p=none; rua=mailto:shathish@shasentralabs.com; fo=1 |
| CNAME | `zb54375864` | zmverify.zoho.in |

## Outstanding

- [x] **Enforce HTTPS** — DONE 2026-07-21. Cert `CN=shasentralabs.com` issued,
      valid to 19 Oct 2026, auto-renews. HTTP now 301s to HTTPS.

      **Gotcha for next time:** GitHub's cert request stalled for 2+ hours because
      its first DNS check failed (the `www` CNAME initially pointed at the apex
      instead of `shathishwarmas.github.io`). The automatic retry never fired.
      Fix: Settings → Pages → Remove custom domain → re-enter it → Save. Cert
      issued within a minute. Run `git pull` afterwards — GitHub rewrites CNAME.
- [ ] `platform.shasentralabs.com` — no destination yet. Add a CNAME once the
      Next.js app is hosted somewhere that can run SSR (GitHub Pages cannot).

## Constraints

GitHub Pages serves **static files only**. The main ShaSentra Next.js app
(SSR, API routes, Genkit, dashboard) cannot run here — it needs Cloud Run,
Firebase App Hosting, AWS Amplify, Vercel, or similar.
