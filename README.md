# Hourly POS Planning Export

Reusable Codex skill for converting verified StarPOS **Hourly Totals Report** `.xls` exports into the standard planning-session format.

## What it does

- Accepts StarPOS Hourly Totals reports only and rejects other POS report types.
- Reconciles the source date range against every valid daily separator before assigning dates.
- Uses each blank separator row to add a weekday and date header.
- Converts the report to a simple three-column layout: hourly label, metric total, and total sale value.
- Retains total figures only and removes location-by-location detail.
- Requires independent date, data, and format proofing before delivery.

## Use it for

StarPOS hourly bar, café, bistro, and outlet sales exports that need to be prepared for a planning session.

## Invocation

Use `$hourly-pos-planning-export` and provide the source export files. Add a prior planning-session export when a venue-specific reference layout must be matched.

## Important date rule

The date after `And` in the StarPOS header is the exclusive end boundary only when it reconciles to the number of valid daily separators. For example, a report from `31/07/2026` to `11/09/2026` with 42 valid separators contains daily sections from 31 July through 10 September. A mismatch stops the workflow for review.
