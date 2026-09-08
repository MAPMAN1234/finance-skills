# Finance Skills — Install Checklist

**Source:** https://github.com/himself65/finance-skills  
**Date compiled:** 2026-05-28

---

## ⚠️ Platform Reality Check

Most skills in this repo require shell access, pip, and live network calls.
They will **not** function on claude.ai's sandboxed environment.

| Install path | Best for | Works on |
|---|---|---|
| `npx plugins add himself65/finance-skills` | All 8 skills, full capability | **Claude Code** (you) ✓ |
| ZIP upload → claude.ai Customize → Skills | `generative-ui` only (uses built-in `show_widget`) | claude.ai sandbox |

**Recommended: use the plugin install path below.**

---

## Step 1 — Install the Plugin (Claude Code)

Run once from any directory:

```powershell
npx plugins add himself65/finance-skills
```

This registers all 6 plugin groups. Skills are namespaced as:
- `/finance-market-analysis:yfinance-data`
- `/finance-market-analysis:options-payoff`
- `/finance-market-analysis:earnings-preview`
- `/finance-market-analysis:earnings-recap`
- `/finance-market-analysis:estimate-analysis`
- `/finance-market-analysis:stock-correlation`
- `/finance-data-providers:funda-data`
- `/finance-ui-tools:generative-ui`

To install only one plugin group:
```powershell
npx plugins add himself65/finance-skills --plugin finance-market-analysis
```

---

## Step 2 — Funda AI Setup (two surfaces)

`funda-data` is the highest-value skill but requires separate auth for each surface.

### Surface A: MCP (research/synthesis) — OAuth

The MCP config has been pre-written to `C:\Users\dusty\.claude\.mcp.json`:

```json
{
  "mcpServers": {
    "funda": {
      "type": "http",
      "url": "https://funda.ai/api/mcp"
    }
  }
}
```

**To activate:**
1. Sign up / log in at https://funda.ai and start a subscription
2. Restart Claude Code — it will detect the new `.mcp.json`
3. On first call to `mcp__funda__agent_chat`, a browser tab opens for OAuth approval
4. Token is auto-managed (1-hour access + 30-day refresh)

### Surface B: REST (raw data) — API key

Set your key as a permanent Windows user environment variable:

```powershell
[System.Environment]::SetEnvironmentVariable("FUNDA_API_KEY", "YOUR_KEY_HERE", "User")
```

Then restart your terminal. Verify with:
```powershell
$env:FUNDA_API_KEY
```

Get your key at: https://funda.ai → Account → API Keys

---

## Step 3 — Verify installs

After Step 1 and a Claude Code restart, test each skill:

| Skill | Test phrase |
|---|---|
| `yfinance-data` | "Get me AAPL's last 5 days of price history" |
| `funda-data` | "funda: preview MSFT's next earnings" |
| `earnings-preview` | "Give me an earnings preview for NVDA" |
| `earnings-recap` | "Recap AMZN's last earnings report" |
| `estimate-analysis` | "Show me estimate revisions for META" |
| `options-payoff` | "Plot a covered call payoff on TSLA at $250 strike" |
| `stock-correlation` | "What stocks are most correlated with SPY?" |
| `generative-ui` | (dependency — fires automatically when other skills render charts) |

---

## Skill Install List (reference)

| # | Skill | Plugin group | ZIP (claude.ai fallback) | What it does |
|---|---|---|---|---|
| 1 | `yfinance-data` | finance-market-analysis | `yfinance-data.zip` | Stock prices, history, financials via yfinance |
| 2 | `funda-data` | finance-data-providers | `funda-data.zip` | Analyst synthesis (DCF, comps) + raw data via Funda AI |
| 3 | `earnings-preview` | finance-market-analysis | `earnings-preview.zip` | Pre-earnings briefing from Yahoo Finance |
| 4 | `earnings-recap` | finance-market-analysis | `earnings-recap.zip` | Post-earnings analysis after a report drops |
| 5 | `estimate-analysis` | finance-market-analysis | `estimate-analysis.zip` | Analyst estimate revision trends |
| 6 | `options-payoff` | finance-market-analysis | `options-payoff.zip` | Interactive options payoff curve from a position |
| 7 | `stock-correlation` | finance-market-analysis | `stock-correlation.zip` | Correlated stocks, sector peers, trading pairs |
| 8 | `generative-ui` | finance-ui-tools | `generative-ui.zip` | Design system for `show_widget` inline charts |

ZIP downloads (claude.ai only): https://github.com/himself65/finance-skills/releases/latest

---

## Skipped Skills

| Skill | Reason |
|---|---|
| `saas-valuation-compression` | SaaS-specific; not relevant to ABF/credit focus |
| `sepa-strategy` | Chart-pattern trading strategy; not applicable |
| `hormuz-strait` | Geopolitical niche |
| `twitter/discord/telegram/linkedin-reader` | Social scrapers; TOS-sensitive |
