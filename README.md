# NexaGroup SBU Loss Recovery — Diagnostic Dashboard

A **self-contained, single-file HTML dashboard** (no build step, no CDN, no framework) that
diagnoses why the fictional **NexaGroup's** loss-making SBUs are losing money in
**Q1 FY2026-27 (Jul–Sep 2026)**, what their recovery plans are, and whether those plans reach
Q2 profit.

> **Synthetic demo.** This is a portfolio project. All company names, SBUs, competitors,
> brands, people, and every monetary / volume figure are **synthetic demo data** — randomly
> rescaled from a demo generator. The `data/*.json` files are static build snapshots; the
> dashboard ships with all numbers already inlined in `index.html`, so **no live source, ERP,
> spreadsheet, or API is used at runtime.**

---

## 1. Problem

A diversified group has a dozen business units running at a loss. Leadership needs, in one view:
*why* each unit is losing, *what* the recovery plan commits, and *whether* that plan actually
closes the gap in the coming quarter. That analysis normally lives scattered across spreadsheets
and reports — hard to read, easy to distrust.

## 2. Solution

A single HTML file that renders an executive **Overview** plus a page per loss-making SBU, with
charts and scorecards derived from the embedded data model. It is fully client-side and deploys
as a static site.

- **Overview** — portfolio KPIs (revenue deficit, net loss, stated commitment, break-even
  coverage), a "revenue recovery — committed vs required" chart, a "break-even vs the Q2 window"
  timeline, and a full SBU scorecard table.
- **One page per loss-making SBU** (collapsible sidebar, most-loss-first), each showing:
  - **Internal problems** — Acquisition · Conversion · Retention · Business degression cards.
  - **External problems** — market / regulatory / macro factors.
  - **Monthly revenue** chart (budget vs actual, Jul/Aug/Sep).
  - **Q2-profit verdict**, **recovery plan**, and **commitment & accountability**.

## 3. Features

- Zero-dependency, single-file app — opens directly in a browser.
- Inline SVG charts (recovery, break-even timeline, monthly trend) — no chart library.
- Severity-toned cards, verdict badges, and an SBU scorecard with rankings.
- Collapsible navigation, most-loss-first ordering, surplus units filtered out.

## 4. SBUs covered (fictional)

Nexa Electronics · Nexa Logistics · Nexa Care Pharmacy · Nexa Jobs · Nexa Telecom ·
Nexa Essentials · Nexa Readymix · Nexa Commodities · Nexa Asphalt · Nexa Infotech ·
Nexa Steel · Nexa Mediplex

## 5. Repo layout

```
business-recovery-dashboard/
├── index.html            # the entire dashboard (data inlined)
├── data/                 # static demo snapshots (not fetched at runtime)
├── src/                  # analysis modules (used at build time only)
├── scripts/              # offline build/validation helpers
├── tests/                # unit tests for the analysis functions
├── PROFILE.md            # portfolio one-pager
├── CHANGELOG.md
└── README.md
```

## 6. Code organization (inside `index.html`)

| Symbol | What it holds |
|--------|---------------|
| `SBUS`  | Array of SBU objects (the data model). Surplus units (loss 0) are spliced out, then sorted most-loss-first at load. |
| `META`  | Per-SBU lookup: business head, loss region, order scorecards, commitments, owner, People/Process/Technology. |
| `BE`    | Break-even month per SBU, used by the overview timeline. |
| `renderOverview()` / `renderSbu()` | Overview KPIs/charts and per-SBU page rendering. |

## 7. Run locally

No build step — it's a static file:

```bash
# just open it
start index.html          # Windows
open index.html           # macOS
# or serve it
python -m http.server 8000
```

## 8. Deploy (Vercel — static)

Import the repository (or this folder) into Vercel. It's a static site: framework **Other**,
build command **none**, output directory the project root. `index.html` is served as-is; no API
or environment variables are required.

## 9. Conventions & gotchas

- **Full names, not codes** in all visible labels.
- Money in **BDT crore** throughout (synthetic).
- `deficit` = top-line deficit; `rev.q1.deficit` = budget − actual.
- Surplus SBUs (loss 0) are hidden at load — remove the filter line to show them.

## 10. Disclaimer

Prepared for a personal portfolio. No real company, financial, or personal information is
included; any resemblance to a real organisation is coincidental. See `PROFILE.md`.
