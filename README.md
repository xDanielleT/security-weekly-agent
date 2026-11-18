# Security Weekly Agent

An AI agent that publishes a plain-language cybersecurity brief to my portfolio every Monday.
**Live briefs:** https://danibytes.dev/security-weekly.html

## How it works

**Gather → Filter → Summarize → Publish**

1. **Gather:** CISA Known Exploited Vulnerabilities (JSON) plus BleepingComputer, The Hacker News and Krebs on Security (RSS)
2. **Filter:** last 7 days only, duplicate stories removed, anything matching my tool watchlist (`agent/watchlist.json`) flagged and moved to the top
3. **Summarize:** Claude rewrites each item: what happened, what to do, and why it matters for help desk / IT support
4. **Publish:** GitHub Actions saves `public/briefs/YYYY-MM-DD.json` into my portfolio repo, and Netlify redeploys

## Branches

| Branch | What it does when the workflow runs |
|---|---|
| `main` | Publishes the brief to the portfolio (also runs automatically every Monday) |
| `dev` | Builds a preview only, downloadable from the workflow run as `brief-preview` |

Work happens on `dev`; changes reach `main` through a pull request.

## Setup

Add two repository secrets (**Settings → Secrets and variables → Actions**):

| Secret | Value |
|---|---|
| `ANTHROPIC_API_KEY` | API key from console.anthropic.com |
| `PORTFOLIO_TOKEN` | Fine-grained token with access to `my-portfolio` only, permission **Contents: Read and write** |

## Run locally

```bash
npm install
ANTHROPIC_API_KEY=your-key npm run brief   # writes to briefs/
```

## Customize

- Tools to watch: `agent/watchlist.json`
- Schedule: the `cron` line in `.github/workflows/security-weekly.yml`
- News sources: `NEWS_FEEDS` in `agent/build-brief.mjs`

## Tech

Node.js · Claude API · CISA KEV · RSS · GitHub Actions · Netlify
