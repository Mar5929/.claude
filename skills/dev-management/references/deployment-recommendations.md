# Deployment Recommendations Reference

Use this reference to recommend deployment platforms based on the user's tech stack. Lead with the simplest, most cost-effective option for their situation.

---

## Quick Recommendation Guide

| Stack | Recommended Platform | Why | Alternatives |
|---|---|---|---|
| Next.js | Vercel | Built by the Next.js team, zero-config, free tier | Netlify, AWS Amplify, Cloudflare Pages |
| React (Vite/CRA) | Vercel or Netlify | Free static hosting, auto-deploys from Git | Cloudflare Pages, GitHub Pages |
| Static sites / HTML | Netlify or GitHub Pages | Free, simple, no server needed | Cloudflare Pages, Vercel |
| Node.js API | Railway or Render | Simple, affordable, Git-based deploys | Heroku, DigitalOcean App Platform, Fly.io |
| Python (Django/FastAPI) | Railway or Render | Easy Python hosting, managed databases | Heroku, DigitalOcean App Platform, Fly.io |
| Ruby on Rails | Render or Heroku | Rails-friendly, easy setup | Railway, DigitalOcean App Platform |
| Go / Rust | Fly.io or Railway | Fast cold starts, good for compiled binaries | Render, DigitalOcean |
| Full-stack (DB + API + Frontend) | Railway or Render | All-in-one with managed databases | Vercel (frontend) + Railway (API + DB) |
| Docker-based | DigitalOcean App Platform or Fly.io | Native Docker support | Railway, Render, AWS ECS |
| Enterprise / Complex | AWS or Azure | Full control, any architecture | GCP, DigitalOcean |
| Mobile backend | Supabase or Firebase | Built-in auth, realtime, storage | AWS Amplify, PocketBase |

---

## Platform Details

### Vercel
- **Best for:** Next.js, React, static sites, serverless functions
- **Pricing:** Free tier (hobby), $20/mo (pro)
- **Key features:** Zero-config Next.js, edge functions, preview deployments, analytics
- **Deployment:** Automatic from GitHub — connect repo and go
- **Database:** Vercel Postgres, Vercel KV (Redis), or bring your own

### Netlify
- **Best for:** Static sites, JAMstack, React/Vue/Svelte SPAs
- **Pricing:** Free tier, $19/mo (pro)
- **Key features:** Auto-deploys, serverless functions, forms, identity, split testing
- **Deployment:** Automatic from GitHub
- **Database:** Bring your own (pair with Supabase, PlanetScale, etc.)

### Railway
- **Best for:** Full-stack apps, APIs, background workers, databases
- **Pricing:** Usage-based, $5/mo minimum with credit
- **Key features:** Git-based deploys, managed Postgres/MySQL/Redis, easy scaling, monorepo support
- **Deployment:** Automatic from GitHub or CLI
- **Database:** Built-in Postgres, MySQL, Redis, MongoDB

### Render
- **Best for:** Web services, static sites, background workers, cron jobs
- **Pricing:** Free tier (with limitations), $7/mo (starter)
- **Key features:** Auto-deploys from Git, managed databases, Docker support, cron jobs
- **Deployment:** Automatic from GitHub
- **Database:** Managed Postgres (free tier available)

### Fly.io
- **Best for:** Docker apps, globally distributed apps, Go/Rust services
- **Pricing:** Free tier (3 small VMs), usage-based after
- **Key features:** Global edge deployment, built-in Postgres, fast cold starts, Wireguard networking
- **Deployment:** CLI-based (`fly deploy`)
- **Database:** Built-in Postgres, or bring your own

### Heroku
- **Best for:** Quick prototypes, Rails apps, simple deployments
- **Pricing:** $5/mo (eco dynos), $25/mo (basic)
- **Key features:** Simple Git push deploys, extensive add-on marketplace, easy scaling
- **Deployment:** `git push heroku main` or GitHub integration
- **Database:** Managed Postgres add-on

### DigitalOcean App Platform
- **Best for:** Docker-based apps, full-stack with managed infrastructure
- **Pricing:** $5/mo (starter), usage-based
- **Key features:** Docker support, managed databases, auto-scaling, GitHub auto-deploy
- **Deployment:** Automatic from GitHub or Docker
- **Database:** Managed Postgres, MySQL, Redis, MongoDB

### Supabase
- **Best for:** Backend-as-a-service, mobile/web app backends
- **Pricing:** Free tier, $25/mo (pro)
- **Key features:** Postgres database, auth, realtime subscriptions, storage, edge functions
- **Deployment:** Managed service — no deployment needed
- **Database:** Built-in Postgres with realtime capabilities

### AWS / Azure / GCP
- **Best for:** Enterprise, complex architectures, specific compliance requirements
- **Pricing:** Usage-based (can be complex)
- **Key features:** Full control, any architecture, global infrastructure
- **Note:** Recommend only when simpler platforms don't meet requirements. Higher complexity and operational overhead.

---

## Decision Flowchart

Use this to quickly narrow down the recommendation:

1. **Is it a static site or SPA with no backend?** → Vercel, Netlify, or Cloudflare Pages
2. **Is it a Next.js app?** → Vercel (first choice) or Netlify
3. **Does it need a database + API?** → Railway or Render
4. **Is it Docker-based?** → Fly.io or DigitalOcean App Platform
5. **Does it need global edge distribution?** → Fly.io or Vercel (edge functions)
6. **Is it a mobile app backend?** → Supabase or Firebase
7. **Is there a specific compliance/enterprise requirement?** → AWS, Azure, or GCP
8. **User isn't sure / wants simplest option?** → Railway (most things) or Vercel (frontend-heavy)

---

## Cost Comparison (Approximate Monthly for Small Projects)

| Platform | Free Tier | Small Project | Medium Project |
|---|---|---|---|
| Vercel | Yes (hobby) | $20/mo | $20/mo + usage |
| Netlify | Yes | $19/mo | $19/mo + usage |
| Railway | $5 credit/mo | $5-15/mo | $20-50/mo |
| Render | Yes (limited) | $7-20/mo | $25-75/mo |
| Fly.io | Yes (3 VMs) | $5-15/mo | $20-60/mo |
| Heroku | No | $12-30/mo | $50-100/mo |
| DigitalOcean | No | $12-24/mo | $36-72/mo |
| Supabase | Yes | $25/mo | $25/mo + usage |
