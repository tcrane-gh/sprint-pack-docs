---
title: Sprint Pack help
---

<!-- Status: written 2026-10-08 (clarity pass C1). -->

# Sprint Pack help

Sprint Pack shows what happened to each sprint's scope: what was committed at the start, what was added and removed after it, what was completed, and what was carried over. Every number is a Jira search you can open.

## Pages

- [How the numbers are counted](numbers.md): every column and ratio, whose view is counted, "≈" approximate sprints, "computing".
- [Anomalies](anomalies.md): the flags on sprints with bad data, and why flagged sprints are left out of averages.
- [JQL functions and fields](jql.md): the 12 functions, the sprint references they take, the 8 fields, how fresh they are, and what search errors mean.
- [Views](views.md): the Sprint scope page, the sprint menu, the dashboard gadget and widget, drill-downs, charts, the Sprint portfolio and the burndown.
- [Exports](exports.md): CSV, Excel, slides, the copy summary and snapshots, and how links behave in spreadsheets.
- [Admin page](admin.md): what needs attention, health, the point budget, rebuilds and backfill.
- [BI tools](bi.md): publishing sprint totals for Power BI, Excel, Google Sheets, Looker Studio, Tableau and warehouses.
- [Support](support.md), [privacy policy](privacy.md) and [security](security.md).

## The short version

- **Counted as you see them.** Views and files count only the issues you have permission to see, as Jira's own Sprint Report does. See [whose view is counted](numbers.md#counted-as).
- **Nothing leaves Atlassian.** Sprint Pack runs on Atlassian's platform and has no server of its own. Files are built in your browser. Published BI data stays in Jira as board properties.
- **The searches are the source.** Click any count to see its issues and the JQL behind it. See [drill-down](views.md#drill-down).
