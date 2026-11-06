# Methodology — NexaGroup SBU Loss Recovery Dashboard

## 1. Abstract

This project compresses a quarterly loss-recovery review of twelve loss-making strategic business
units (SBUs) of a fictional diversified group into one self-contained, zero-dependency HTML
dashboard. The analytical frame is a **loss bridge**: the distance between budgeted and actual
profit is attributed to revenue shortfall at the unit's own realised gross margin, and the residue
is named rather than guessed. Around that spine the tool carries four layers an MD actually needs
— a rule-driven *direction* per SBU, a ranked set of *decisions with options, NP impact, owners and
kill criteria*, the *evidence* behind each call (regional loss demarcation, dealer churn, budget-vs-
actual trend, market share, people cost), and a *coverage test* of whether each SBU's own commitment
plugs its deficit inside the coming quarter. Every figure is synthetic and randomly rescaled; source
labels such as ERP tables, Data IDs and GL codes are illustrative scaffolding. All charts are
hand-written inline SVG on a tokenised dark-navy design system. The output is deliberately static:
data snapshots are inlined at build time so the file opens from disk, deploys as a static site, and
states plainly where the data is missing.

## 2. Problem Framing

A diversified group closes a quarter with a dozen business units below break-even. The recurring
management questions are blunt: which unit should be fixed, refocused, protected or reviewed for
exit; what has to be decided *this week*; and does each unit's own recovery promise actually cover
its shortfall before the next quarter ends?

In practice the answers sit scattered across a P&L pack, a sales scorecard, an ERP order extract,
a market-share sheet and a data-request tracker. Each artefact is defensible alone and
contradictory together, so review meetings spend their time arguing provenance instead of choosing
action. The dashboard exists to make one number agree across layers: it attaches a source tag and a
completeness measure to every figure, and refuses to display an estimate where an input is missing.
Missingness is a first-class result, not a gap to hide.

## 3. Related Practice

Analysts do not invent a method for this problem; they borrow three well-established ones.
**Loss-bridge / price-volume-mix decomposition** is the standard P&L diagnostic — budget-to-actual
profit variance is split into volume, price, mix, cost and fixed-cost effects, and whatever the
inputs cannot explain is booked as an unexplained residual. **Break-even (CVP) analysis** supplies
the sales level at which EAT reaches zero, `fixed cost ÷ contribution-margin %`, and the "coverage"
logic used here is a simple commitment-against-shortfall test rather than a forecasting model.
**Portfolio screens** — growth/share matrices and stop-loss criteria — are the conventional way to
force one directional label per unit from two or three measured numbers. Where a real engagement
would run these in spreadsheets or BI tools, the framing is unchanged: quantify, attribute, then
attach a decision, an owner and a kill criterion to each finding.

## 4. Methodology & Data Design

*Conceptual provenance.* The demo model stands in for six inputs: an SBU P&L pack (revenue, budget,
COGS, gross margin, EAT, fixed cost, break-even sales), an order-level sales ledger (dealer activity
by month and region), a scorecard of self-declared recovery commitments, a marketing market-share
tracker, a people-cost extract and a data-request register that assigns an owner and status to every
ask. In this build each is a synthetic JSON snapshot under `data/`, generated with fictional names
and randomly rescaled figures, then inlined into `index.html` between the `MDDATA` markers by
`scripts/embed_data.py`. Nothing is fetched at runtime.

*Transformation steps.* (1) `scripts/validate_data.py` checks keys, types and ranges per snapshot.
(2) `embed_data.py` writes the objects as JS literals — the file stays the single source of truth.
(3) At load the app normalises, sorts SBUs most-loss-first, derives group totals and computes the
per-unit measures below. (4) Renderers emit one pane per view; nothing is stored between sessions
except the theme choice.

*Metric definitions.*

| Metric | Formula |
|---|---|
| Revenue deficit | `budget revenue − actual revenue` (per SBU, quarter) |
| Net loss | aggregate of negative EAT across loss-making units |
| Coverage | `stated commitment ÷ revenue deficit` (100 % = fully committed, not profitable) |
| Revenue effect (loss bridge) | `(budget revenue − actual revenue) × actual gross-margin %` |
| Residual | `NP gap − revenue effect` — named, not decomposed, because budgeted margin and unit data are absent |
| Driver label | market-driven if `|revenue effect| ≥ 0.5 × |NP gap|`, else internal |
| Break-even sales | `fixed cost ÷ contribution-margin %` |
| Dealer churn | dealers with ≥1 order in Q4 and zero in Q1, ÷ Q4-active dealers |
| Data completeness | share of the P1 Data IDs actually available to the unit |
| Simulator EAT | `rev·(1+p)·(1+v) − cogs·(1+c)·(1+v) + other income/costs held at Q1` |
| Game payoff | elasticity −1.2 on own price move, rival matches 60 % of any cut |

## 5. Analytical Framework

The UI encodes a decision tree, applied in strict rule order so that two analysts get the same
answer. **Direction:** gross margin ≤ 0 → *Restructure / Exit review*; margin < 10 % and loss > 5 %
of revenue → *Refocus*; revenue ≥ 70 % of budget and loss ≤ 12 % of revenue → *Fix & Recover*;
otherwise *Insufficient data*. Every label is stamped **provisional** while data completeness is
below full coverage. **Evidence hierarchy:** measured ERP/finance facts are shown first; analyst
interpretation (hypotheses H1–H9, Q2 verdicts, recommendations) is visually separated and named as
interpretation. **Decision layer:** a decision is only promoted to the top-5 list if it carries an
option set including do-nothing, an NP impact, an owner, a decide-by date and a kill criterion; if
the inputs are absent it is parked in a "waiting for data" queue with the blocking Data ID and its
owner. The competitive-game and simulator screens exist to convert a diagnosis into a bounded
question: *is holding price the best response, and how large a move is needed to reach break-even?*

## 6. System Design

One static file: `index.html` holds markup, the design layer and the data literals, plus a single
vanilla-JS `<script>`. Charts are hand-written inline SVG (grouped budget/actual bars, a break-even
timeline with hover markers, a committed-vs-required bar grid, a demand chart) so there is no chart
library, no bundler and no network dependency beyond the Google Fonts link. Rendering is
function-per-pane (`renderMD`, `renderOverview`, `renderSbu`, `renderMarket`, `renderLoss`,
`renderSales`, `renderRootCause`, `renderData`, `renderSim`) with a hash-free menu that toggles
`.pane.active`. The presentation layer is token-driven CSS custom properties — surfaces, type scale,
status colours and chart palette — with a `data-theme="light"` variant and a memory-only fallback
when storage is unavailable, so it behaves identically over `file://` and HTTPS. Static delivery was
chosen deliberately: a review artefact that must open on a projector, in an email, or offline should
have no moving parts, no credentials and no failure mode other than the file itself.

## 7. Findings & Design Decisions

*What the (synthetic) data story shows.* The loss is concentrated: two units (Nexa Essentials,
Nexa Steel) carry ~57 % of total net loss and both are supply/price-recoverable inside one quarter,
while several others are structurally thin-margin businesses where the margin test alone explains
the loss. Across the ten revenue-deficit units the stated commitments cover about 77 % of the
deficit and only one unit commits 100 % or more, so the recovery pack as written does not close the
gap in Q2 — that is the single most decision-relevant number on the Overview. Regional demarcation
shows decline is localised rather than general, and dealer churn identifies the cheapest recoverable
revenue. Several blocks (SKU-level bridge, contribution margin, regional competitor share) stay
deliberately unpopulated: the honest read is *insufficient data*, with the ask named.

*Key UI choices.* Dark navy, data-dense, two accents only (blue + amber; no violet). KPI cards lift
2 px on hover and carry delta chips so each headline number states its own consequence. Charts keep
tooltips plus legends and never rely on colour alone; tables use sticky headers and right-aligned
tabular numerals so multi-unit columns read as one grid. Missing data is rendered as a
`Data pending` chip with the blocking Data ID instead of a zero or a blank. Motion is feedback only:
one ≤450 ms load-in stagger, hover/press transitions at 150–300 ms, and everything disabled under
`prefers-reduced-motion`. A visible *synthetic demo data* chip in the top bar keeps the fiction
front-and-centre rather than buried in a footnote.

## 8. Limitations & Validity

All names, figures, tables of source, competitor brands and people are synthetic; nothing here
describes a real company and no real statistic is quoted. The snapshots are randomly rescaled, so
magnitudes are illustrative and no external validity should be inferred — the object of the exercise
is the method and the interface. The loss bridge is a two-term attribution (revenue effect plus
residual), not a full price-volume-mix-cost decomposition, because the inputs for the missing terms
are absent by design; elasticity and rival-response constants in the game screen are assumptions,
not observations. Break-even timing is the unit's own stated plan, not an independent forecast, and
coverage measures promise against shortfall — it is not a probability of recovery. Scope is one quarter of fourteen fictional units (twelve of
them loss-making); nothing here tests seasonality, cash conversion or balance-sheet capacity.

## 9. Reproducibility & Deployment

```bash
git clone <this repo> && cd business-recovery-dashboard
start index.html            # Windows — or open index.html directly (no server needed)
node tests/test_analysis.js # analysis-rule checks
python -m http.server 8000  # optional local serving
```

Edit data in `data/*.json`, then re-validate and re-embed:
`python scripts/validate_data.py data/ && python scripts/embed_data.py`
(`scripts/split_inventory.py` regenerates the `src/` working copy and inventory snapshot.)
Requires only a modern browser; Python is needed only to rebuild snapshots, Node only to run tests.
**Vercel:** import the folder, framework *Other*, no build command, output directory = project root;
the repo deploys as-is with no environment variables, keys or API — the single runtime network
request is the Google Fonts stylesheet, and system font fallbacks render correctly if it is blocked.
