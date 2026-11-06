# Changelog

All notable changes to this project are documented here. This project adheres to
semantic versioning.

## v1.1.0 — Visual redesign (design-spec v2 "Modern")

Presentation layer only — no data object, id, selector or file name changed.

- Design layer rewritten on CSS custom properties (dark-navy, data-dense): one radius scale,
  one spacing scale, blue + amber accents only (indigo/violet removed), Fira Sans / Fira Code.
- Sticky top bar with product mark, one-line descriptor, a visible *synthetic demo data* chip and
  a persisted dark/light theme toggle; new sidebar active-state (3 px accent bar + tint).
- KPI cards gain hover lift and delta chips; charts recoloured to the shared palette with added
  SVG tooltips, a demand-chart legend and corrected colour wording in tooltips.
- Tables: sticky headers, right-aligned tabular numerals, shared `.tbl-wrap` scroll containers.
- Accessibility & motion: `:focus-visible` rings, `aria-label` on icon-only buttons, contrast-safe
  status inks, one ≤450 ms load-in stagger, full `prefers-reduced-motion` opt-out.
- Footer gains a methodology anchor (`PROFILE.md`) and owner credit; `README.md` and `PROFILE.md`
  rewritten to the portfolio documentation standard (9-section research-style methodology).

## v1.0.0 — Demo release

Initial public demo release of the **NexaGroup SBU Loss Recovery Dashboard**.

- Self-contained, single-file HTML diagnostic dashboard (no build step, no CDN, no framework).
- Portfolio overview: revenue deficit, net loss, stated commitments, break-even coverage,
  and a full SBU scorecard with revenue-recovery and break-even-timeline charts.
- One page per loss-making SBU: internal/external problem cards, monthly revenue
  (budget vs actual), a Q2-profit verdict, a recovery plan, and commitment & accountability.
- **All figures, brand names, competitors, and people are synthetic demo data.** Monetary and
  volume values are randomly rescaled from a demo generator; no real company, financial, or
  personal information is included. This release is for portfolio demonstration only.
