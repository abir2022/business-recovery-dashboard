# NexaGroup SBU Loss Recovery — Diagnostic Dashboard

**One-liner:** A zero-dependency, single-file HTML dashboard that turns a diversified group's
loss-making business units into one clear executive view — why each unit is losing, what its
recovery plan commits, and whether it reaches Q2 profit.

> **Synthetic-data demo.** All company names, SBUs, competitors, brands, people, and monetary /
> volume figures are fictional demo data, randomly rescaled for this portfolio. No real company,
> financial, or personal information is included; `data/*.json` are static snapshots and every
> number is inlined into `index.html` — nothing is fetched at runtime.

## The problem

A diversified group runs a dozen business units at a loss. Leadership needs to see, in one place,
*why* each unit is losing, *what* its recovery plan promises, and *whether* that plan closes the
gap next quarter — but the analysis lives scattered across spreadsheets and reports, hard to read
and easy to distrust.

## The solution

One self-contained HTML file, fully client-side, that renders:

- An **Overview** — portfolio KPIs (revenue deficit, net loss, stated commitment, break-even
  coverage), a committed-vs-required recovery chart, a break-even vs Q2-window timeline, and a
  full SBU scorecard.
- **A page per loss-making SBU** — internal (Acquisition / Conversion / Retention / Degression)
  and external problem cards, a monthly budget-vs-actual chart, a Q2-profit verdict, and the
  recovery plan with commitment & accountability.

## Features

- Single file, no build step, no CDN, no framework — opens directly in a browser.
- Inline SVG charts and scorecards; severity-toned cards and verdict badges.
- Collapsible sidebar with most-loss-first ordering; surplus units auto-hidden.
- Clean separation of fact vs analyst interpretation, flagged on screen.

## Tech stack

- HTML + CSS + vanilla JavaScript (one `<script>`, embedded data model).
- Hand-rolled inline SVG for all charts.
- Python / Node helper scripts for offline data validation & inlining (not needed to run).

## Run locally

```bash
start index.html            # Windows (or: open index.html)
python -m http.server 8000  # or serve statically
```

## Deploy (Vercel — static)

Import the folder into Vercel as a **static** site: framework *Other*, no build command, output =
project root. `index.html` is served as-is; no API keys or environment variables required.
