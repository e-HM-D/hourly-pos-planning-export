# Hourly POS Planning Export

Reusable Codex skill for converting MAX Gaming **Hourly Totals Report** `.xls` exports into the standard planning-session format.

## What it does

- Reads the report date range and checks that the daily sections reconcile.
- Uses each blank separator row to add a weekday and date header.
- Converts the report to a simple three-column layout: hourly label, metric total, and total sale value.
- Retains total figures only and removes location-by-location detail.
- Checks that times, currency, dates, and the final workbook layout are correct before delivery.

## Use it for

Hourly bar, café, bistro, and outlet sales exports that need to be prepared for a planning session.

## Invocation

Use `$hourly-pos-planning-export` and provide the source export files. Add a prior planning-session export when a venue-specific reference layout must be matched.

## Important date rule

The date after `And` in the MAX export header is the exclusive end boundary. For example, a report from `31/07/2026` to `11/09/2026` contains daily sections from 31 July through 10 September.
