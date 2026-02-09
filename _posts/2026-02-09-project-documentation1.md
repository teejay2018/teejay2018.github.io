---
layout: post
title: "Agent Holgers documentation on project model portfolio"
hidden: true
date: 2026-02-09
categories: [invest, portfolio, agent]
tags: [openclaw, agent]
---
# 🧾 Model Portfolio — Setup Documentation (v1)

**File:** `2026-02-09-model-portfolio-setup-documentation-v1.md`  
**Date:** 2026-02-09  
**Author:** Holger  
**Owner:** Tom  

---

## 0) 🎯 What this system is
We’ve set up a **weekly model portfolio workflow** with:
- A **Google Sheet** (Holdings + Transactions) as the system of record.
- A **weekly Markdown note** saved to Google Drive.
- A strict **transaction budget (max 20 lifetime trades)** to force discipline.
- A consistent **pricing convention** so each week is comparable.
- Automation via **OpenClaw cron** + **gog (Google CLI)** + **browser automation**.

This is a *simulation* / model portfolio (not a broker integration).

---

## 1) 🧠 Portfolio rules (the “constitution”)
### 1.1 Transaction budget
- **Max 20 transactions total** for the lifetime of the model.
- Default action is **HOLD**.
- Trades are allowed only if:
  - thesis materially changes,
  - a clear risk event emerges,
  - there’s a clearly superior swap.

### 1.2 Basket size and intent
- Starting point: **8-stock growth-tilted basket**.
- Tom preference: **understandable, growing businesses** (tech-leaning), avoid gambling.
- Fun sleeve context: <5% of total net worth (separate from safe core).

---

## 2) 🏷️ Naming conventions (Drive + Sheet)
### 2.1 Google Drive folder
- Shared Drive folder: **exchange**
- Folder ID: `17CneptcZVaEYnxe580_VC01Av2s1Umaw`

### 2.2 Weekly note (Markdown)
- Format: **Markdown + emojis**
- Naming convention:
  - `YYYY-MM-DD-model-weekly-note.md`

Example:
- `2026-02-09-model-weekly-note.md`

### 2.3 Model portfolio sheet (Google Sheets)
- Naming convention:
  - `YYYY-MM-DD-model-portfolio`

Example:
- `2026-02-09-model-portfolio`

### 2.4 Sheet tabs
- **Holdings** (was initially “Model portfolio”, renamed by Tom)
- **Transactions**

---

## 3) 📊 Google Sheet data model
### 3.1 Transactions tab
Purpose: **immutable ledger** of actions over time.

Header (current):
- Timestamp (Europe/Copenhagen)
- Week start
- Action (BUY/SELL)
- Ticker
- Company
- Amount DKK
- Price
- Shares
- Fees DKK
- Trade #
- Notes

Notes:
- We log initial buys as trades **#1–#8**.
- We keep the ledger append-only.

### 3.2 Holdings tab
Purpose: **current snapshot** of positions + valuation.

Columns A–H: the “static” holdings definition:
- Ticker, Name, Region, Sector/Theme, Thesis, Start weight %, Entry date, Notes

Columns I–U: first-draft valuation block added on 2026-02-09:
- Amount DKK
- Entry Price
- CCY
- Entry FX
- Shares
- Entry Value DKK
- Ref Price (Close)
- Ref date
- Ref FX
- Ref Value DKK
- P/L DKK
- P/L %
- Weight %

Implementation detail:
- Ref Value DKK / P&L / weights are computed via **formulas** so the weekly job only needs to update:
  - Ref Price (Close)
  - Ref date
  - Ref FX

---

## 4) 💱 Pricing + FX conventions (locked)
### 4.1 Weekly reference price
- **Previous trading day close**.
- Since the job runs on Monday morning, that is typically **Friday close**.

### 4.2 FX convention
- Use the **same prior trading day close** for:
  - USD/DKK
  - EUR/DKK

Rationale:
- Makes each week comparable and removes intraday noise.

---

## 5) 🧰 Tools & authentication

### 5.1 gog (Google Workspace CLI)
We use `gog` for:
- Drive: search, download, upload, rename
- Sheets: get/update/append/format/metadata

Account:
- `cantaloop.agent@gmail.com`

#### Keyring / non-interactive requirement
Problem encountered:
- `gog` initially failed with:
  - `no TTY available for keyring file backend password prompt; set GOG_KEYRING_PASSWORD`

Fix:
- Use **manual auth** when browser is not on the same machine (SSH):
  - `gog auth add ... --manual`
- Ensure the gateway service has:
  - `GOG_KEYRING_PASSWORD` set in its environment

We learned:
- Pasting a callback URL into the shell won’t work unless you’re in the right flow.
- `--manual` is the correct approach for headless/SSH setups.

### 5.2 systemd user service env vars
The gateway runs as a **systemd user service**.

We set env vars using:
- `systemctl --user edit openclaw-gateway.service`

and added, e.g.:
- `GOG_ACCOUNT=cantaloop.agent@gmail.com`
- `GOG_KEYRING_PASSWORD=...`

Then:
- `systemctl --user daemon-reload`
- `systemctl --user restart openclaw-gateway.service`

### 5.3 web_fetch
Used for lightweight HTTP pulling.

Observed issue:
- Some endpoints (e.g., Yahoo) returned **429 Too Many Requests**.

Mitigation:
- Prefer Nordnet via browser automation for equities.
- Use stooq for FX closes when needed.

### 5.4 browser automation (OpenClaw browser tool)
Initial issue:
- Browser tool couldn’t start because no supported browser was detected.

Fix path:
1) Installed Chromium — but it was a **snap** build and proved unreliable for automation.
2) Installed **Google Chrome stable** and configured OpenClaw to use it.

OpenClaw browser config (final):
- headless: true
- noSandbox: true
- executablePath: `/usr/bin/google-chrome-stable`

After this:
- Nordnet search automation worked reliably.

---

## 6) 🕰️ Cron automation (OpenClaw)

### 6.1 What we changed
We converted the weekly reminder cron to a fully automated **agent run**.

Schedule:
- **Every Monday 05:00 Europe/Copenhagen**

Why 05:00:
- EU/DK markets are closed → Nordnet “last” is effectively **prior close**.

### 6.2 Job behavior (desired)
Each Monday 05:00, the automated run should:
1) Set `weekStart` = today’s date in Copenhagen.
2) Fetch **prior close** prices for the 8 tickers using Nordnet search (browser tool).
3) Fetch **USD/DKK** and **EUR/DKK** prior close (fallback: stooq).
4) Update Sheet:
   - Holdings: update Ref Price/Ref date/Ref FX
   - Transactions: append only if trades occur
5) Create + upload weekly note:
   - `{weekStart}-model-weekly-note.md`
6) Send Tom a short summary.

---

## 7) 🧾 Current holdings (tickers)
The model portfolio basket uses:
- MSFT
- NVDA
- AMZN
- GOOGL
- ASML.AS (Amsterdam listing for ASML)
- NOVO-B.CO (Copenhagen listing)
- SAP.DE (XETRA listing)
- ASM.AS (Amsterdam listing)

---

## 8) 🔍 Nordnet automation notes
### 8.1 Search overlay
We use Nordnet’s site search overlay to locate instruments and read the displayed last price.

### 8.2 “Close” correctness
- At **05:00 Copenhagen**, “last” should correspond to **previous close** for EU/DK.
- US names will also be previous close.

If Nordnet changes its UI, we may need to adjust selectors and/or read the instrument page fields.

---

## 9) ✅ What’s “done” vs “next”
### Done
- Drive folder + file naming standard
- Sheet naming standard + tabs renamed
- Pricing convention locked
- gog auth stabilized for non-interactive gateway
- Chrome-based browser automation working
- Cron moved to Monday 05:00 and upgraded to agent-run concept
- Holdings valuation block (first draft) added

### Next improvements (optional)
- Add a weekly snapshot row/table (portfolio total value, weekly return, cumulative return).
- Improve robustness of “close” extraction if Nordnet UI changes.
- Add automated “top movers” and per-holding commentary in weekly note.

---

## 10) 🔒 Security & ops notes
- Keep `GOG_KEYRING_PASSWORD` protected (systemd user override is convenient; env file with 600 perms is safer).
- Browser automation runs headless with `--no-sandbox` (pragmatic for servers; keep gateway bind loopback unless you intentionally expose it).

---

*End of v1.*
_Last updated: February 2026_  
_Source: Cantaloop Aps._
