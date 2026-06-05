# Portfolio Website

A dark-themed static portfolio — HTML, CSS, vanilla JS. No build step, no framework, no dependencies.

**Primary deployment:** Cloudflare Pages (free, auto-deploys on push, global CDN, automatic HTTPS)  
**Alternate deployment:** Self-hosted on a Raspberry Pi with Nginx (setup scripts included)

---

## Live Demo

Deploy to Cloudflare Pages and your site is live at `https://<project-name>.pages.dev` within 60 seconds of pushing. See [SETUP.md](SETUP.md) for exact steps.

---

## Repository Structure

```
├── index.html                        # Single-page site
├── css/
│   └── style.css                     # Dark theme, CSS variables, responsive grid
├── js/
│   └── main.js                       # Minimal JS (year footer, smooth scroll)
├── nginx/
│   └── portfolio.conf                # Nginx site config for self-hosting
├── scripts/
│   ├── setup-rpi.sh                  # Install Nginx + deploy site on a fresh Pi
│   ├── setup-cloudflare-tunnel.sh    # Expose Pi publicly via Cloudflare Tunnel
│   ├── setup-tailscale.sh            # Private mesh VPN + optional Tailscale Funnel
│   └── deploy.sh                     # Push site updates to Pi over rsync/SSH
├── _headers                          # Cloudflare Pages security headers
└── SETUP.md                          # Full deployment guide (start here)
```

---

## Quick Start — Cloudflare Pages

1. Fork or clone this repo to your GitHub account.
2. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → Create → Pages → Connect to Git.
3. Select your repo. Leave build command and output directory **blank** (static site, no build needed).
4. Click **Save and Deploy**.

That's it. Every push to `main` redeploys automatically.

Full walkthrough including custom domain setup: **[SETUP.md](SETUP.md)**

---

## Customization

### Your name and headline
Edit `index.html`, find the `#hero` section:
```html
<h1>Hey, I'm <span class="accent">Your Name</span></h1>
<p class="hero-sub">Your tagline here.</p>
```

### Project cards
Copy any `<article class="project-card">` block in the `#projects` section:
```html
<article class="project-card">
  <div class="card-header">
    <span class="card-icon">🛠</span>
    <span class="card-lang">Python</span>
  </div>
  <h3><a href="https://github.com/you/repo">Project Name</a></h3>
  <p>What it does in one sentence.</p>
  <div class="card-footer">
    <span class="tag">tag1</span>
    <span class="tag">tag2</span>
  </div>
</article>
```

### Colors
All colors are CSS variables at the top of `css/style.css`:
```css
:root {
  --bg:      #0d1117;   /* page background   */
  --accent:  #58a6ff;   /* blue highlights   */
  --accent2: #3fb950;   /* green (logo, CLI prompts) */
}
```

---

## Self-Hosting on Raspberry Pi

If you want to run this on your own hardware instead of Cloudflare Pages:

```bash
# On a freshly flashed Raspberry Pi OS Lite 64-bit:
git clone https://github.com/starlordz12/rpi-website.git
cd rpi-website
bash scripts/setup-rpi.sh

# Then pick how to expose it publicly:
bash scripts/setup-cloudflare-tunnel.sh   # requires a domain you own
bash scripts/setup-tailscale.sh           # no domain needed (Tailscale Funnel)
```

To push updates to a running Pi:
```bash
bash scripts/deploy.sh <pi-ip-address>
```

Full self-hosting walkthrough is in the appendix of [SETUP.md](SETUP.md).

---

## Security Headers

`_headers` configures Cloudflare Pages to serve:
- `X-Frame-Options: SAMEORIGIN`
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: camera=(), microphone=()`

The Nginx config in `nginx/portfolio.conf` applies the same headers for self-hosted deployments.