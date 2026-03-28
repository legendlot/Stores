# CLAUDE.md — LOT OPS ERP
# Repo: legendlot/stores
# Live: store.legendoftoys.com

## What this is
An internal operations ERP for Legend of Toys store and production management.
Handles raw material inwarding, daily issue to production, returns, and reporting.
Do not introduce paid infrastructure without asking. Keep costs at zero where possible.

## Architecture
- Frontend: Cloudflare Workers (deployed via Wrangler)
- Backend: Google Apps Script (Apps Script URL called via fetch)
- Data store: Google Sheets (master store sheet)
- Deploy command: wrangler deploy
- Local dev: wrangler dev

## Access levels (strictly enforce these)
- Reann (store manager): data entry only, no costs visible
- Afshaan (owner): full access to everything
- Vinay (co-founder): read-only + financials + CSV/Excel report downloads

## Store team
- 4 unskilled operators, 1 supervisor, Reann (acting store manager)
- Reann is the primary daily user — UI must be simple, unambiguous, mobile-friendly

## Google Sheets structure
- inventory — current stock levels
- transactions — all movements (inward, issue, return)
- issue_calculator — BOM-based daily issue calculator
- returns_log — RTO and RTV return records

## Bagging system
- Parts are stored in bags of 25
- Stock is weight-counted, not individually counted

## Features — Phase 1 (build these first)
1. Apps Script API layer
2. GRN entry (goods received note — raw material inward)
3. Work Order / Kit Calculator (BOM-based daily issue to production)
4. Ad hoc issue
5. Live dashboard (current stock levels)
6. Returns entry (RTO and RTV)
7. Report downloads (CSV/Excel, Vinay-facing)
8. Vinay finance view

## Immediate priority
Kit Calculator tab in Store Master Control — this is the next thing to build.

## Returns context
- RTO: customer didn't open product — goes straight back to RTD stock unless box is damaged
- RTV: customer return — needs repair/refurbishment before restock
- Both come from Amazon, Flipkart, website, Blinkit, Zepto, Instamart

## Coding rules
- Cloudflare Worker frontend is the shell — keep it thin, logic lives in Apps Script
- All data calls go through the Apps Script web app URL (store in a config/env variable)
- LockService is already implemented in Apps Script — do not remove it
- Access level gating must be enforced on the frontend (check role before rendering sections)
- UI must work on both desktop and mobile

## Deploy
- wrangler deploy from the repo root
- Confirm on store.legendoftoys.com after deploy
