---
name: hourly-pos-planning-export
description: "Convert StarPOS Hourly Totals reports into the validated planning-session layout, with reconciled day/date headers and independent proofing. Use only for StarPOS hourly reports; do not use for MAX Gaming or other POS exports."
---

# StarPOS hourly POS planning export

Convert a StarPOS **Hourly Totals Report** into the three-column planning-session layout. The source report controls dates and totals. A planning-session reference file controls visual layout only.

## Scope and exclusions

Use this skill only for a StarPOS hourly report that matches the required signature below. Do not use it for MAX Gaming, daily sales summaries, item reports, payment reports, or a generic spreadsheet that happens to contain hourly sales.

Do not infer dates from the first or last recorded sale, a user's expected date, or a planning-session reference file.

## Required StarPOS report signature

Before editing, concatenate every nonblank cell in the first populated title row, then normalise whitespace in that combined text. Verify all of the following against the combined title text:

- The title contains `Hourly Totals Report`.
- The title contains `Between dd/mm/yyyy And dd/mm/yyyy`.
- The title contains `Selected Locations`.
- Fully blank rows separate daily sections.
- Each day has repeating four-row metric blocks in this order: `Sale Value`, `Transaction Count`, `Item Count`, `Profit`.
- A `Sale Value` row starts with an Excel time serial in column A, from 0 inclusive to 1 exclusive.

Stop and explain the failed signature check if any requirement does not match. Do not adapt this skill to another report type without a new reviewed instruction.

## Date and separator reconciliation

Parse the header dates strictly as `dd/mm/yyyy`.

For StarPOS Hourly Totals reports, `And <end date>` is an exclusive boundary. Derive the expected number of daily sections as:

`end date - start date`

A valid separator row is fully blank, including cells that only contain whitespace, and its next populated row begins a daily block with a numeric time in column A and `Sale Value` in column B.

Require the number of valid separators to equal the expected number of days exactly. Assign separator index `i` the date `start date + i days`.

For example, `Between 31/07/2026 And 11/09/2026` with 42 valid separators produces headers from `Friday  31/07/2026` through `Thursday  10/09/2026`. Never add an `11/09/2026` header.

If the separator count suggests an inclusive range, is missing or duplicated, or does not reconcile, stop for user confirmation. Never guess or fill dates.

## Transformation

Keep the source title row in columns A to C. Convert the sheet to three columns only:

| Source row | Output A | Output B | Output C |
| --- | --- | --- | --- |
| Daily separator | blank | `dddd  dd/mm/yyyy` | `Total` |
| Sale Value | source time | `Sale Value` | source column C total |
| Transaction Count | source label | source column B total | blank |
| Item Count | source label | source column B total | blank |
| Profit | source label | source column B total | blank |

Discard source columns D onward. They are per-location detail and must not be copied, summed, or substituted for the total column.

Preserve blank, zero, and negative values. Do not create rows for hours with no source data. Do not use a fixed daily sales window to decide whether a section or date is valid.

## Format and reference rules

Match any supplied planning-session reference file for column order, widths, and plain visual style. It must not override the StarPOS header dates, section count, or totals.

Without a reference file, use Arial 10 pt, standard gridlines, no merged cells, and no coloured day bands. Use `h:mm AM/PM` for times, a currency format for Sale Value in column C and Profit in column B, and `#,##0.00` for counts and item totals. Starting widths are A 22.8, B 20.6, and C 50.1.

## Required proofing and release gate

When delegation is available, use three independent read-only review agents after the main conversion. Do not give reviewers the intended conclusion; give them the source report and final output.

1. **Source/date auditor** creates a manifest for each file: exact header text, parsed start and end dates, valid separator rows, expected count, assigned first and last dates, and daily block counts.
2. **Data-reconciliation auditor** checks every output row against the source mapping. It confirms that titles are preserved, Sale Value totals use source C, other totals use source B, output C is blank on non-Sale rows, D onward is absent, and no value, row, or day was lost or duplicated.
3. **Format and visual auditor** reopens the output and checks all date headers plus representative first, middle, and last sections. It verifies three columns, `Total` in C, time/currency/count formats, dates in one-day increments, and the reference layout where supplied.

The primary agent acts as release reviewer. Deliver only when every audit passes for every file. If any audit fails, repair the output and run all three checks again. If subagents are unavailable, perform the same three checks as separate, labelled review passes before delivery.

## Stop conditions

Stop without exporting a final workbook when any of the following occurs:

- The report is not a verified StarPOS Hourly Totals Report.
- Header dates are missing, ambiguous, or end on or before the start date.
- A blank separator does not lead into a valid Sale Value block.
- The date range and valid separator count do not reconcile exactly.
- A daily block does not follow the required metric order.
- A reference file conflicts with the validated StarPOS source dates or data.

State the exact failed check and ask the user for a correctly exported source or direction.
