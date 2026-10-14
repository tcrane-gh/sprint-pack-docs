---
title: Views
---

<!-- Status: written 2026-10-08 (clarity pass C1; inferred reopens in the board panel, approved 2026-10-09). -->

<a id="views"></a>
# Views

Sprint Pack shows the same sprint view in four places, plus a portfolio across boards. Everything counts the issues you can see (see [whose view is counted](numbers.md#counted-as)), and anyone who can see a board in Jira can use its views.

| Where | What |
|---|---|
| A project's **Sprint scope** tab | [The full view](#project-page) for any board in the project |
| A sprint's **…** menu → **Scope changes** | [The same view](#sprint-action), opened on that sprint |
| Dashboards → **Sprint scope** gadget | [A compact view](#gadget) for one board, or a portfolio |
| New dashboards → **Sprint scope** widget | [The same](#widget), with tiles when narrow |
| Apps → **Sprint portfolio** | [Several boards side by side](#portfolio), and any one board's full view from there |

<a id="project-page"></a>
## The Sprint scope page

**One row per sprint: the active sprint plus the last 6 closed sprints (choose 3, 6, 10 or 15), newest first.**

**Finding it:** under Jira's new navigation a project shows the **Sprint scope** tab only once someone adds it with **+** beside the project's tabs. Without the tab, the same view is in **Apps → Sprint portfolio**: a board's name there opens its view.

- **Controls:** the board (the project's boards), how many sprints, issues or the board's estimates, and Refresh.
- **Each row:** the sprint's name and state, its dates, its goal, and its [buckets](numbers.md#buckets): Committed, Added, Removed, Completed, Not completed, Carried over, Re-estimated, and [Drift](numbers.md#drift).
- **Flags:** a sprint with bad data shows an [anomaly](anomalies.md) label. Completion cells marked "≈" are [approximate](numbers.md#approximate).
- **Computing:** a sprint not yet computed shows "computing" instead of numbers; the page refreshes itself. See [computing](numbers.md#computing).
- **Updating:** a sprint being computed again (after an update changed the counting rules, or a [rebuild](admin.md#rebuild)) keeps its earlier numbers, marked "updating", until the new ones are ready. Exports leave it out until then. The label links to [updating](numbers.md#updating).
- **Click any number** to see its issues, directly under the table: see [drill-down](#drill-down). Every row also has a "Search in Jira" link for the sprint's issues.
- **Under the table:** one line on how it's counted ("Matches Jira's Sprint Report as you see it", or that it can't while the board's settings are unreadable), plus short status when it applies: issues hidden from you, issues the app can't see, approximate sprints, sprints left out of averages.
- **"Couldn't check which issues you can see"** means Jira didn't answer when Sprint Pack asked which issues you can see, often because Jira was busy. No counts are shown rather than counts that might be wrong. Refresh, or open the page again, after a moment.
- **Warnings stay on screen** when you need to act: the board's estimation changed during the sprints shown (see [estimation change](numbers.md#estimation-change)), or Sprint Pack can't read the board's settings (see [settings unreadable](numbers.md#settings-unreadable)).
- **Below:** the [charts](#charts), the [burndown](#burndown), the [exports](exports.md) and the [BI](bi.md) entry point ("Use in Power BI / Excel").

**No Scrum board in the project:** the page says so, since Sprint Pack counts sprints and Kanban boards have none, and links to the [portfolio](#portfolio), whose picker lists every board you can see.

**Board admins** also see a "BI data for this board" panel here, and on the board's view in the Sprint portfolio: publishing (when allowed), [labels](#labels), [working days](#working-days), the BI handoff, and the board's [inferred reopens](admin.md#inferred-reopens), when it has any. Jira decides who counts as a board admin: space admins on team-managed boards, the board's administrators on company-managed boards, and Jira admins everywhere.

<a id="sprint-action"></a>
## From a sprint's menu

On the board's backlog or sprint, **… → Scope changes** opens the same view in a dialog for that sprint's board, with that sprint highlighted and its burndown shown. The sprint is always in the table, even when it's older than the last 6 closed sprints; while it's computing, the burndown shows another sprint and switches to it when it's ready. A future sprint has nothing to show yet.

<a id="gadget"></a>
## Dashboard gadget

**The Sprint scope gadget puts the view on a Jira dashboard.** Its settings choose:

- **One board** (the sprint table), or **several boards**, **every board with a label** or **every board in a project** (the [portfolio](#portfolio)). A label selection picks up boards that get the label later.
- How many sprints (3, 6, 10 or 15), and issues or estimates.

No board is chosen for you. If the list is empty, you can't see any Scrum board: ask a Jira admin for access to a board's project and filter. If the boards can't be loaded, the settings say why and offer Retry.

On a dashboard the table is compact, with short column headers (hover one for its full name):

| Header | Column |
|---|---|
| Start | [Committed](numbers.md#committed) |
| Add | [Added](numbers.md#added) |
| Rem | [Removed](numbers.md#removed) |
| Done | [Completed](numbers.md#completed) |
| Open | [Not completed](numbers.md#not-completed) |
| Carry | [Carried over](numbers.md#carried-over) |
| Re-est | [Re-estimated](numbers.md#re-estimated) |
| Drift | [Drift](numbers.md#drift) |

**`*` after a drift value** means some issues had no estimate at the start; hover for how many (see [unestimated](numbers.md#unestimated)).

The heading names the board, with a link to open it. The active sprint's goal is shown above the table. The gadget refreshes with the dashboard, and **Refresh** reloads it now, in the portfolio too.

<a id="widget"></a>
## Dashboard widget (new dashboards)

**On Jira's new dashboards, the Sprint scope widget shows the same view, with the same settings as the gadget.**

- **Wide:** the full table, charts and burndown, as on the gadget. When the row is shorter than the view, it scrolls inside the widget; a Large or Extra Large row shows more at once.
- **Narrow or short** (half a row or less, under about 600 pixels, or a Small or Extra Small row): the sprint goal and four tiles:
  - **Say/do** of the last closed sprint;
  - **Average completed** over the closed sprints shown;
  - **Scope change** of the last closed sprint;
  - **Burndown:** what's left in the active sprint, the ideal, and working days left (plus the scope change since the start, if any).
  
  The tiles skip sprints with [anomaly flags](anomalies.md#left-out). An "Open the board" link goes to the board, and **Refresh** reloads all four tiles, also after an error.
- **Several boards, a label or a project:** the [portfolio](#portfolio). Narrow, it's one line per board: last and average say/do and the trend.
- **Adding it:** in **Add widget**, search for Sprint scope. The preview shows made-up example sprints, not your data; your boards appear once you choose them in the settings.
- **Refresh:** the widget reloads itself every 15 minutes while the dashboard is open and showing; a hidden tab waits until it's shown again. "Refreshed" gives the time of the last load, and **Refresh** reloads now.
- **Settings:** open the widget's settings and press the dashboard's **Save**. The view shows your changes as you make them.
- **Dashboard filters:** the widget always shows the boards in its settings. When the dashboard has filters set, a line says so, so you don't read its numbers as filtered.

<a id="board-hidden"></a>
**"You can't see this board in Jira"** shows when a gadget or widget is set to a board you can't open in Jira, for example when its filter isn't shared with you. Nothing about that board is shown. To fix it, ask the dashboard's owner or a Jira admin for access to the board's filter and project. The view updates on the next load. The board lists in the settings show only boards you can see. A portfolio says how many of its boards are hidden from you, and leaves them out of its rows and totals.

<a id="drill-down"></a>
## Drill-down

**Click any count to see the issues behind it and the JQL search that finds them.** The list opens directly under the table, headed by the bucket, the sprint and the number of issues, 25 to a page.

- **The query** is shown above the list, with a copy button, "Open in search" (Jira's issue search, as you) and a link to the function's help. It's a normal search: save it as a filter, use it on a board or in another gadget.
- **Each issue:** key, summary, status, and what happened: "Added 3 Oct 10:15 by Ana", "Removed …", "Carried to Sprint 15", or "Committed at start".
- **Estimates:** Jira's value → the value at close, with the change and percentage; "none" if it had no estimate; "removed" if the estimate was cleared. On time boards, time logged.
- **Show estimate history:** every change with when, who, and whether it was during the sprint.
- The list shows every issue in the bucket that you can see, however many there are.

<a id="charts"></a>
## Charts

Under the table, three charts over the same sprints (computing sprints are left out):

- **Scope per sprint:** stacked bars of committed, added, removed and carried over.
- **Velocity: committed vs completed:** grouped bars, like Jira's velocity chart. The subtitle gives the [average completed](numbers.md#velocity), leaving out the active sprint and flagged sprints.
- **Say/do % and scope change % per sprint:** one line each. See [say/do](numbers.md#say-do) and [scope change](numbers.md#scope-change).

- **The active sprint** is in every chart, labelled "(in progress)", because its figures aren't final. Its completed bar is a separate "Completed so far" series in another colour. In [slides](exports.md#slides), its bars are hatched and its line points hollow.
- Approximate sprints are marked in the table, not in the charts.
- Charts follow the issues/estimates toggle. Hours are shown for time-estimate boards. Bars start at zero.
- **Under each chart,** one sentence says what it shows, for example "Sprint 14 completed 12 of 15 issues committed", so the figures are there for screen readers and at a glance. The burndown has one too.

<a id="portfolio"></a>
## Sprint portfolio

**Apps → Sprint portfolio compares boards: one row per board, ratios rather than raw totals, each counted as you see it.**

- **Choose:** boards, a label or a project; the last N sprints per board, or sprints closed between two dates; issues or estimates; and grouping by label or project. Show. Your choice is remembered for next time.
- **Anyone who can see a board can add it.** Every Scrum board you can see is in the pickers. Rows appear as each group of ten arrives.
- **More than 50 boards** are shown 50 at a time, with **Next** and **Previous**. Every figure on the page, the group medians included, covers the 50 shown, and the page says which.
- **Each board's name** opens that board's full Sprint scope view on this page, with a link back to the portfolio; **board** next to it opens the board in Jira.
- **Columns:**
  - **Sprints:** closed sprints counted.
  - **Last say/do:** the last closed sprint without an [anomaly flag](anomalies.md#left-out), named under the figure.
  - **Avg say/do, Velocity ratio, Scope change, Carry-over:** ratios of sums over the selected closed sprints. See [say/do](numbers.md#say-do), [velocity ratio](numbers.md#velocity-ratio), [scope change](numbers.md#scope-change), [carry-over](numbers.md#carry-over-rate).
  - **In progress:** the active sprint, completed of committed so far, with added and removed, and the sprint's name.
  - **Trend:** see [below](#portfolio-trend).
  - **CSV / Excel:** that board's exports.
- **Labels on a board row:** "≠ published" (the published totals differ from yours, because they count issues you can't see), "settings unreadable", "computing N", and anomaly flags. Each label links to its explanation.
- **Grouped:** each label or project gets a row with the median (and mean) of its boards' ratios, issue counts added up, and point totals only when every board uses the same unit, otherwise "mixed units". A board with several labels appears under each; boards with none sit under "(no label)". See [units](numbers.md#units).
- **Slides and summary:** "Download for slides" and "Show a summary to copy" work here too (see [exports](exports.md#slides)). Under the trend chart, one sentence gives each board's last say/do and how many rose or fell.
- **When boards don't load:** if a group of boards fails to load, for example because Jira was busy, the error names those boards and offers Retry, which asks again for just those boards. They're left out of the table and the group medians rather than shown with old or empty figures.
- **Large selections and the hourly budget:** checking which issues you can see costs about a point of Sprint Pack's [hourly budget for views](admin.md#budget) per issue. When a group of boards needs more than is left this hour, the error says how many points it needs; choose fewer boards or sprints, or try again after the top of the hour.
- **While sprints compute,** the page asks again every 20 seconds, for the boards still computing only.
- **What's missing travels with the figures:** the summary, the slide footer and the snapshot cover each say how many boards were hidden from you, still computing, or failed to load.

<a id="portfolio-trend"></a>
### Trend

**The trend arrow compares the last say/do with the board's average:** ↑ more than 5 percentage points above, ↓ more than 5 below, → within 5. Flagged sprints are left out of both sides.

**The trend chart** (shown when 8 or fewer boards are selected) draws each board's say/do per closed sprint, counted back from its last closed sprint ("last", "−1", "−2" …). Flagged sprints are left out, as in the table.

<a id="burndown"></a>
## Burndown

**The burndown shows a sprint's remaining work per day against the ideal line, rebuilt from the same list of changes Jira's burndown chart uses.**

- **Pick a sprint:** the active sprint by default (or the one you opened from its menu). The burnup toggle shows completed against scope instead.
- **Under the chart:** Jira's list of changes: when, which issue, what changed (added, removed, estimate changed, moved into or out of the last column), and what was remaining after it. The issue column header links to the matching `addedAfterSprintStart` / `removedAfterSprintStart` search.
- **"So far":** the last point of an active sprint is today's value so far, and the subtitle says how much is left, the ideal, and the working days left.
- **Done** is the board's last column, as saved at the close for a closed sprint, today's columns for the active one.
- **Done at the start is in scope:** issues already done when the sprint started are part of the scope, not of what's remaining, as in Jira's chart. (The sprint table counts them differently: see [done at the start](numbers.md#done-at-start).)
- **Daily points:** Jira's chart is a stepped line; Sprint Pack draws one point per day, the totals at the end of each day in the board's time zone.
- **Counted as you see them;** the [published burndown](bi.md#publishing) counts what the app can see.
- **Nothing to draw?** A sprint whose start is after its close has no days to show; it's flagged [dates reversed](anomalies.md#dates-reversed).
- **"History still being refreshed":** see [history refreshing](numbers.md#history-refreshing).
- Sub-tasks are left out, as in Jira's chart.

<a id="working-days"></a>
### Working days

**The ideal line runs from the scope at the start to zero at the planned end, flat across non-working days.** Jira keeps a board's working days in a setting Sprint Pack can't read, so:

- **The default** is the site's time zone and Monday–Friday with no holidays. That's exactly Jira's setting for team-managed boards.
- **For other settings,** a board admin or Jira admin opens **Working-day settings** under the burndown, or **Working days** in the board panel, and enters the time zone, working weekdays and non-working dates (the same three sections as Jira's Board settings → Working days).
- **Or paste them:** the form links to the board's configuration in Jira. Select all, copy, and paste it into the form. Only the working-days part is kept.
- **Use the default** asks first, since it replaces the time zone, days and dates entered.
- The chart's subtitle names the settings used. Settings changed later in Jira need entering again.

<a id="labels"></a>
## Labels

**Labels group boards for the portfolio and for BI tools,** for example by business line ("Payments") and by release train ("ART 2"). A board can have several.

- **Board admins** set their own board's labels in the board panel on the Sprint scope page. **Jira admins** set any board's on the admin page, add or remove a label on several boards at once, or paste a mapping, one board per line (`12, Payments, ART 2`).
- Labels are trimmed, at most 60 characters, and can't contain semicolons. A label that matches an existing one ignoring case keeps the existing spelling.
- **Labels are current, not historical.** Relabelling a board changes the labels on every past sprint of that board, in the portfolio and in BI tools. See [labels in BI data](bi.md#labels-bi).
