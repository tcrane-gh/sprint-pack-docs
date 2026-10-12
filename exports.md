---
title: Exports
---

<!-- Status: written 2026-10-08 (clarity pass C1, C7). -->

<a id="exports"></a>
# Exports

**Every export is built in your browser from the numbers you're looking at, counted as you see them, and saved as a file. Nothing is sent anywhere.**

| Export | Where | What you get |
|---|---|---|
| [Sprints, Issues, Estimate history (CSV)](#csv) | Sprint scope view | One table per file |
| [All three (Excel)](#excel) | Sprint scope view; per board in the portfolio | One workbook |
| [Download for slides](#slides) | Sprint scope view, sprint menu, widget, portfolio | A .pptx with editable charts |
| [Slide for one sprint](#sprint-slide) | Sprint scope view, sprint menu | One slide: the sprint's numbers and what changed |
| [Copy summary](#copy-summary) | Sprint scope view, portfolio | Plain sentences for email or chat |
| [Share a snapshot](#snapshot) | Sprint scope view, portfolio | A dated, self-describing .xlsx |

- **Which sprints:** the sprints the view shows (computing sprints are left out).
- **Who it's counted as:** you. Every file says so. See [whose view is counted](numbers.md#counted-as).
- **No scheduled exports.** To keep a spreadsheet or BI tool up to date automatically, [publish the board for BI tools](bi.md).

<a id="csv"></a>
## CSV

Three files, each starting with the same four header lines:

1. "As visible to you on <date and time>"
2. The board, the sprints included, the estimate field and unit, and the time zone
3. "Definitions and caveats: <link to this help>"
4. Whether the file is complete: "Complete: every sprint shown is included.", or a line starting "INCOMPLETE:" that names each sprint left out and why

The table always starts on line 5, under the row that begins `sprintId`. A script can skip the first four lines, or look for that row. Before using the data, check that line 4 starts with "Complete".

A sprint the view shows as **computing** or **updating** is left out of an export until Sprint Pack has finished it, because exports carry finished numbers only. Line 4 names it; export again once it shows as up to date.

Then one table:

- **Sprints:** one row per sprint: dates, goal, estimate field and unit, `countedAs`, [`approximate`](numbers.md#approximate), [`anomalies`](anomalies.md), the issue count and estimate for every [bucket](numbers.md#buckets) (including completed from the commitment and completed outside the sprint), [`sayDo`](numbers.md#say-do), the [estimate change totals](numbers.md#drift), and `jqlCommitted`, `jqlAdded`, `jqlRemoved`, `jqlCompleted`, `jqlNotCompleted`, `jqlCarriedOver`: the search behind each count, as text you can paste into Jira's issue search.
- **Issues:** one row per issue and sprint: key, summary, type, status, assignee, the buckets it's in, when it was added or removed and by whom, whether it was completed, where it was carried to, and its estimates (Jira's value at the start, when added and at the close; the current value; re-estimates; net change).
- **Estimate history:** one row per estimate change: from, to, when, who, and whether it was before the start, during the sprint or after the close.

Dates carry your site's time-zone offset. Time estimates are in hours (and seconds, in the Issues and History files).

**If your Excel uses `;` to separate values** (common in Europe and Latin America), opening a CSV file puts each row in one column. Use **All three (Excel)** instead, or import the CSV with Data → From Text/CSV and choose comma as the delimiter.

<a id="excel"></a>
## Excel

**All three (Excel)** puts the Sprints, Issues and Estimate history tables in one workbook, plus a **Definitions** sheet: one row per column with its meaning and a link to its section of this help. The `jql…` columns are clickable links to Jira's issue search.

- **Dates are real Excel dates** in your site's time zone, so they sort, filter and work in formulas. Each is followed by a column ending in `Iso` with the same moment as text, with the time-zone offset, as in the CSV files.
- **Say/do is a percentage** (0.889 shows as 88.9%).

In the portfolio, each board row has its own **CSV** (sprints) and **Excel** (all three) buttons. They export the sprints the row shows: the last N, or the sprints closed between the portfolio's dates. The Excel file's `jql…` columns are links, as in the Sprint scope view's.

<a id="slides"></a>
## Download for slides

**A .pptx built from the view's numbers: one slide per board.**

- **Board slide:** the title with the period (calendar days in your site's time zone); the active sprint's goal; the velocity chart (committed against completed, the average in the title); the say/do and scope change line chart; and a table of the sprints shown. More than 8 sprints continue on a second slide, "(continued)".
- **Portfolio slide:** say/do by board (average and last sprint), and a table of ratios per board with medians per label or project. Points are never added across boards with different units. The slide shows the first 12 boards and says how many more there are; the snapshot has them all.
- **The charts are real PowerPoint charts,** with their data inside ("Edit data" works). Value axes start at zero, so bar heights compare fairly.
- **The active sprint** is labelled "(in progress)" in the charts and table. Its bars are hatched, and its line points are hollow on a dashed segment.
- **It uses Office's default theme,** 16:9, with the "Title Only" layout. Paste the slides into your own deck with **Use destination theme** to take its fonts and colours.
- **The footer** says "Counted from the issues visible to <your name> on <date> · Sprint Pack", with a link to how the numbers are counted.
- Google Slides may turn imported charts into pictures.

<a id="sprint-slide"></a>
## Slide for one sprint

**One slide for a sprint review: the sprint's numbers, then what changed and who changed it.**

- **Which sprint:** the one the sprint menu was opened from, otherwise the active sprint, otherwise the last closed one. The button names it.
- **On the slide:** the sprint's dates and goal; say/do and scope change; a chart of committed, added, removed, completed and carried over; and a table of each issue added (by whom, when), removed (by whom, when) and carried over (to which sprint). A long list continues on further slides.
- **Counted as you:** the list comes from the issues you can see, like the Issues export, and uses the same hourly budget.

<a id="copy-summary"></a>
## Copy summary

**One plain sentence per sprint, from a fixed template, for email or chat.** For example:

> Sprint 14: committed 18 points, delivered 16 of them (89%); 5 points added and 2 removed mid-sprint.

- The wording never varies, and nothing is generated by AI.
- Units are spelled out; approximate sprints say "(approximate)"; the active sprint says "(in progress)".
- The portfolio version gives medians across boards and never adds points across different units.
- The text is dated and ends with who it was counted as.
- Select the text and copy it.

<a id="snapshot"></a>
## Snapshots

**A snapshot is a dated, self-describing .xlsx you can send to someone else: it records what you saw, who it was counted as, and links that let them check every number live.**

**Sheets:**

1. **Snapshot** (the cover):
   - the board or boards, and the sprints covered;
   - a one-paragraph summary (the [copy summary](#copy-summary) text) of the sprints in the file, with its date;
   - "Taken <date and time in the site's time zone> (<UTC>)";
   - "Counted as <your name>, who could see N issues; M issues on these sprints were hidden from them", and how many issues Sprint Pack itself couldn't see;
   - the app version and the counting rules' version;
   - each sprint's [anomaly flags](anomalies.md);
   - **Check it live:** for each sprint, a link to each bucket's Jira search, so whoever opens the file can re-run the counts under their own permissions;
   - links to "How these numbers are counted" and "Why two people can see different numbers".
2. **Sprints:** the [Sprints table](#csv), with `anomalies` and the `jql…` columns as links.
3. **Issues** and 4. **History:** the same as the CSV exports.
5. **Definitions:** one row per column and metric, with a link to its help.

**People's names** appear where today's exports have them: the assignee, who added or removed an issue, and who changed an estimate. Think about who you send the file to, as you would with any Jira export.

**Why two people can see different numbers:** each count includes only issues the person counting could see. If the recipient's "Check it live" link finds more or fewer issues, the difference is issues one of you can't see, or changes since the snapshot was taken. See [hidden issues](numbers.md#hidden-issues).

<a id="links-in-spreadsheets"></a>
## Links in spreadsheets

**Excel files have real, clickable links. CSV files can't, so their links are plain text.**

- **.xlsx** (Excel export, snapshots): links are proper hyperlinks, not `=HYPERLINK()` formulas.
- **CSV:** the header lines carry the help address as plain text ("Definitions: https://…"). Excel and Sheets usually turn it into a link once you edit the cell. The `jql…` columns are plain text to paste into issue search.
- **Google Sheets (Apps Script):** the script writes an **About** tab with links to the definitions and the anomaly help, and puts a note with the help link on each tab's header row. See [Google Sheets](bi.md#sheets).
- **BI data:** every column in the column contract carries a link to its definition. See [columns](bi.md#columns).

<a id="untested-apps"></a>
## Apps the files haven't been opened in

**The vendor hasn't yet opened the generated slides in PowerPoint, Keynote or Google Slides, or the generated workbooks and snapshots in Excel, Numbers or Google Sheets.** They have been opened in LibreOffice, and their structure is checked by automated tests. If a file doesn't open or looks wrong in one of these apps, please tell us.

<a id="export-budget"></a>
## Large exports and the hourly budget

**The Sprints table always exports in full. The Issues and History tables read each issue from Jira as you, which counts against Sprint Pack's hourly budget for views.**

- If the budget runs out part-way, the export stops at a sprint boundary and says so: "Stopped at Sprint 14 … The sprint table is complete; the issue and history tables cover 3 of 6 selected sprints. Try again after the top of the hour."
- The file says so too: the fourth header line of every CSV and sheet starts "INCOMPLETE" and names the sprints left out, and a snapshot's cover has an "Incomplete" row. Anyone you forward it to can see it's partial.
- Issues are read in pages of 500, so large sprints export in full.
- See [the budget](admin.md#budget).
