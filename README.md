# AgentGrid

A comparison-first directory of AI agents and bots - a differentiated take on the prompt-centric botdirectory.ai concept.

**Live prototype:** https://test.appcodecraft.com

## What it does

- Curated listings of 28 real AI agents with structured metadata: category, pricing model (free / freemium / paid / open source), autonomy level (assist / automate / autonomous), and platforms.
- Server-side search plus faceted filters (category, pricing, autonomy) - all SSR, no client framework.
- Side-by-side comparison of up to three agents (`/compare?ids=a,b,c`).
- Editorial one-line verdicts per listing: "Best for" / "Watch out for".
- Agent submission form with honeypot spam protection; submissions queue in Workers KV for editorial review (nothing auto-publishes).
- Machine-readable catalog: `/api/agents.json` (CORS-open JSON feed), `/rss.xml`, `/sitemap.xml`, `robots.txt`.
- SEO from day one: SSR HTML, canonical URLs, Open Graph, JSON-LD (ItemList, SoftwareApplication, BreadcrumbList).
- Monetization-ready: labeled FEATURED badges and a sponsored-slot placeholder - explicitly marked "no paid placements live" in the prototype.

## Architecture

Single-file Cloudflare Worker (`worker.js`, ~32 KB, zero dependencies). Embedded data module, SSR HTML, Workers KV for the submission queue. Runs entirely on the Workers free tier.

## Source

The worker source is tracked in the wv forge OpenBot workspace (`agentgrid/worker.js`) alongside its test suite.

## Clean revert

Delete these four resources and nothing remains:

1. Worker script `agentgrid`
2. Workers route `test.appcodecraft.com/*` (zone appcodecraft.com)
3. DNS record `A test.appcodecraft.com` (proxied)
4. KV namespace `agentgrid-submissions`

The production wildcard route (`*.appcodecraft.com/*`), the root domain, and the sibling `bot.appcodecraft.com` experiment were not touched.
