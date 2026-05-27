# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personalized landing pages for Clay (clay.com) sales prospects. Each prospect gets a custom HTML page designed to maximize demo booking conversion. Pages are tailored by prospect name, role, company, and how Clay solves their specific pain points.

## Architecture

- **Landing pages**: Self-contained single-file HTML pages (embedded CSS + JS, no build step). Named `{firstname}-{lastname}-{company}.html`.
- **Prospect data**: `landing-page-prospects.csv` — contains name, email, title, company, website, and LinkedIn for each prospect.
- **Sales context**: `clay_icp.txt` (ideal customer profile) and `clay_value_prop.txt` (value propositions and product details) provide the messaging foundation.

## Design System

**Each prospect's page mirrors their own company's site structure and visual language** — not a single shared template. This is intentional: a page sent to a Linear employee should feel like an extension of linear.app, a page sent to an Airtable employee should feel like an extension of airtable.com, etc.

Current pages and their design references:
- `sarah-mitchell-retool.html` — Retool baseline (warm dark `#1c1917`, Inter weight 300, teal/coral/peach accents)
- `marcus-chen-linear.html` — Linear: dark `#08090A`, dot-grid background, glowing orbs, Linear-style issue-board mockup, single-column flow
- `emily-rodriguez-loom.html` — Loom: light/warm, Fraunces serif headlines, video-thumbnail "research playback" metaphor, soft rounded cards
- `james-okonkwo-webflow.html` — Webflow: clean white + dark code blocks, Space Grotesk geometric headlines, persona-tab switcher, JetBrains Mono accents
- `rachel-goldstein-airtable.html` — Airtable: colorful database-block hero, persona-tab pills, dark AI-agent banner, multi-color feature cards

When editing an existing page, read the file first — the structure differs significantly between prospects. Do not assume Sarah's Retool scaffold is the shared base.

## Creating a New Landing Page

1. Read the prospect row from `landing-page-prospects.csv`
2. Fetch the prospect's company website and study its **layout, sections, and visual language** — not just colors. The page should look like it could live on their company's site.
3. Cross-reference their role/title against `clay_icp.txt` to identify relevant pain points
4. Map pain points to Clay solutions using `clay_value_prop.txt`
5. Build the page with personalized copy addressing the prospect by name, referencing their company and role throughout
6. Use `open <file>.html` to preview in browser
7. Deploy to Vercel (see Deployment) and update `landing_page_url` in the CSV

## Development

No build tools, package manager, or dev server. Pages are static HTML opened directly in a browser. To preview: `open <filename>.html`.

`index.html` at the project root is a copy of Sarah's page (legacy from the first deploy) — leaving it as-is; it makes `ch4-landing-page.vercel.app/` resolve to a real page rather than a 404.

## Deployment

Pages are hosted on Vercel — project name `ch4-landing-page`, owned by the `heysouvikbera-9108` account. The project is already linked to this folder (see `.vercel/` directory).

**Live URLs:** `https://ch4-landing-page.vercel.app/{firstname}-{lastname}-{company}.html`

**To redeploy after editing any HTML file:**
```
cd "/Users/souvik/Documents/CC COHORT MAY26/ch4-landing-page" && vercel --prod --yes
```

Vercel CLI is installed globally via npm. If `vercel` is not found, see the env notes in `~/.claude/projects/.../memory/env_node_vercel.md`.

## Outbound Distribution (Clay)

The `landing-page-prospects.csv` is also imported as a table in Clay under workspace `948689`:
- Table: `landing-page-prospects`
- URL: https://app.clay.com/workspaces/948689/workbooks/wb_0tfnfkpYhkdj6nkTWEW/tables/t_0tfnfkpMFBiF95xxyd4

The `landing_page_url` column in that table is the per-prospect URL to insert into cold-email or sequence templates. When the CSV changes locally, re-import to a *new* Clay table (Clay's UI doesn't sync existing tables with file changes).
