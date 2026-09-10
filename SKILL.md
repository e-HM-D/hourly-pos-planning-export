---
name: hourly-pos-planning-export
description: "Reformat MAX Gaming Hourly Totals POS .xls exports into the standard planning-session layout, adding day/date headers and retaining hourly total figures. Use for hourly bar, café, bistro, or outlet sales exports; not for general POS reporting."
---

# Hourly POS planning export

Convert a MAX Gaming **Hourly Totals Report** into the clean three-column format used for planning-session analysis. The result must let a reader compare each day’s hourly sales without per-location detail.

## Confirm the source and target layout

- Read the source report’s first row. It must contain a range in the form `Between dd/mm/yyyy And dd/mm/yyyy`.
- Treat the end date as an **exclusive boundary**. A report from 31/07/2026 to 11/09/2026 has 42 daily sections, from 31 July through 10 September.
- The blank rows in the source identify the beginning of each daily section. The number of these rows must equal `end date - start date` in days.
- If the dates, number of sections, or layout do not reconcile, stop and ask the user before changing the export.
- When a planning-session reference file is supplied, match its layout and formatting. It takes priority over the defaults below.

## Standard planning-session layout

Use one worksheet and retain the source report’s first row. Reformat every daily section to three columns only:

| Column | Content |
| --- | --- |
| A | Hour or metric label |
| B | Metric label or total count/profit |
| C | Total sale value |

Replace each daily separator row with:

- A: blank
- B: weekday and date formatted exactly as `Monday  09/03/2026` (two spaces before the date)
- C: `Total`

Map the raw source data as follows:

- For a `Sale Value` row, retain source columns A, B, and C. Column C is the hourly sale total.
- For `Transaction Count`, `Item Count`, and `Profit`, retain source columns A and B only. Clear column C and discard all per-location columns.
- Discard source columns D onward. Do not try to combine or carry their per-location values into the result.

Keep the planning-session format plain: Arial 10 pt, standard gridlines, no merged cells, and no coloured date bands. Use `h:mm AM/PM` for hours, a currency format for sale value and profit, and `#,##0.00` for counts and item totals. Use the reference column widths when available; otherwise use approximately A 22.8, B 20.6, and C 50.1.

## Build and validate

Use the spreadsheet-authoring workflow for the final `.xlsx` outputs. Source `.xls` files can be inspected with a compatible reader, but create the final workbooks with the approved spreadsheet authoring tool.

Before delivery, check at least one exported workbook visually and verify:

- Day headers begin at the start date and progress one day at a time.
- The last day is the day before the report’s end boundary.
- Every daily section has a date header and a `Total` column heading.
- Hours display as times, not currency or decimal serial values.
- `Sale Value` uses column C, while counts and profit use column B.

Create separate corrected copies unless the user explicitly asks to overwrite the source exports.
