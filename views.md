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
- **At the top, the answer, in two parts.** On the left, the last closed sprint (by close date, the same sprint the [portfolio](#portfolio) leads with): its [scope change](numbers.md#scope-change), what was taken on and removed after its start, its [say/do](numbers.md#say-do), how much of the commitment was done ("9 of 11"; work taken on later and done is counted apart, "+2 taken-on done") and when it closed, with its goal. Under the figures, one line compares its scope change with the board's other closed sprints shown: "Highest scope change of the last 4 closed sprints" (or lowest), otherwise above, near or below the board's average (within 5 points counts as near, as in the [portfolio's trend](#portfolio-trend)), then the average itself, counted as the portfolio counts it, with flagged sprints left out. It sits right under the figures, above the count of changes and their reasons. The line is left out when there are fewer than 2 sprints to compare or the sprint is flagged. The full sentence about the sprint is in the [summary to copy](exports.md#copy-summary). A sprint with an [anomaly flag](anomalies.md) still leads, with its flag. On the right, in a grey box with smaller figures, **Now:** the active sprint so far: its scope change so far, what was taken on and removed, how much of the commitment is done, and when it ends. Say/do waits for the close, since mid-sprint it would read as falling short of work that isn't due yet. Opened from a sprint's menu, that sprint is the main part. With no closed sprint yet, the active sprint is the main part, with no "Now".
- **The changes behind the figures:** the taken on / removed figure ("+3 / −1"), in the main part and in Now, is link-coloured. Select it to list those changes under the headline: when, the issue, its summary, taken on or removed, and the [reason](#reasons) if one was given. Select it again, or **Hide the changes**, to close the list. It shows the same changes as [What changed](#burndown), which is where reasons are added.
- **The page follows the main sprint:** its row in the table (and its goal) is highlighted, with the name in bold, **Slide for** it is in Share and export, and What changed opens on it.
- **Each row:** the sprint's name, state and any flags, with its dates on a small line under them (once when it started and ended the same day; the year only when it isn't this year), then its [buckets](numbers.md#buckets) in reading order: what was committed, how scope moved after the start (Taken on, Removed, and Scope change %, in semibold: it's the number this app is about; on closed sprints a small purple bar under it shows its size against the largest scope change shown, so the sprints where scope moved most stand out), the outcome (Completed, Say/do %, Not done), then estimates (Re-estimated, [Drift](numbers.md#drift)), in grey since they're detail rather than the scope story. Each header has a short caption in plain words; hover it for the full definition.
- **Not done** is what was still in the sprint, not in the last column, at the close ([not completed](numbers.md#not-completed)). Usually all of it goes to a later sprint, so nothing more is shown; under the figure, "4 carried over" (which opens those issues) says when only some did, and "none carried over" when none did. On the active sprint it's what isn't done "so far". Exports, BI data and the slide table keep [Not completed](numbers.md#not-completed) and [Carried over](numbers.md#carried-over) as two columns.
- **Zeros are muted,** so the figures that moved stand out; every other count opens its issues.
- **Completed in two parts:** when work taken on after the start was also done, the Completed cell reads "9 + 2": 9 committed at the start and done, plus 2 taken on and done. Each part opens its own issues, and their searches are in the drill-down. [Say/do](numbers.md#say-do) counts only the first part; the [velocity chart](#charts) draws both.
- **Sprint goals** show on a line of their own under each sprint, across the whole table. **Show sprint goals** above the table hides or shows them.
- **The active sprint's Say/do cell** shows "–" ("Shown once the sprint closes"), and its Not done count is what isn't done yet.
- **Flags:** a sprint with bad data shows an [anomaly](anomalies.md) label. Completion cells marked "≈" are [approximate](numbers.md#approximate).
- **Computing:** a sprint not yet computed shows "computing" instead of numbers; the page refreshes itself. See [computing](numbers.md#computing).
- **Updating:** a sprint being computed again (after an update changed the counting rules, or a [rebuild](admin.md#rebuild)) keeps its earlier numbers, marked "updating", until the new ones are ready. Exports leave it out until then. The label links to [updating](numbers.md#updating).
- **Select any number** (it's link-coloured) to see its issues, directly under the table: see [drill-down](#drill-down). The drill-down also links to all the sprint's issues in Jira's search.
- **"Matches Jira's Sprint Report"** is a small green label beside the headline when the counts agree with Jira's report as you see it. It links to [how we count](numbers.md#counted-as).
- **When something differs,** one warning above the table says what and how to fix it: the board's settings can't be read, issues you can see that the app can't (so the totals differ from Jira's), or the active sprint's completed figures are from the last hourly update. Nothing is shown when all is well.
- **Under the table,** a short line says each number opens its issues, and how many issues are hidden from you, if any (Jira's own report hides them too, so they don't break the match). Approximate sprints and computing sprints get a line when there are any.
- **"Couldn't check which issues you can see"** means Jira didn't answer when Sprint Pack asked which issues you can see, often because Jira was busy. No counts are shown rather than counts that might be wrong. Refresh, or open the page again, after a moment.
- **Warnings stay on screen** when you need to act: the board's estimation changed during the sprints shown (see [estimation change](numbers.md#estimation-change)), or Sprint Pack can't read the board's settings (see [settings unreadable](numbers.md#settings-unreadable)). While the settings can't be read, the headline's completion figures and the Say/do column say they're not available rather than show 0%, here and on a narrow widget.
- **Below the table, three tabs:** **Trends** (the [charts](#charts)), **What changed** (the [scope changes](#burndown) after the start, with who made each and why, then the burndown) and **Share and export** (the [exports](exports.md) in two groups, Share and Data files, and the [BI](bi.md) entry point, "Use in Power BI / Excel"). The page opens on Trends.

**No Scrum board in the project:** the page says so, since Sprint Pack counts sprints and Kanban boards have none, and links to the [portfolio](#portfolio), whose picker lists every board you can see.

**Board admins** also see a **Board settings** button at the bottom, here and on the board's view in the Sprint portfolio, with a count of BI requests waiting. It opens the "BI data for this board" panel: publishing (when allowed), [labels](#labels), [working days](#working-days), the BI handoff, and the board's [inferred reopens](admin.md#inferred-reopens), when it has any. Jira decides who counts as a board admin: space admins on team-managed boards, the board's administrators on company-managed boards, and Jira admins everywhere.

<a id="sprint-action"></a>
## From a sprint's menu

On the board's backlog or sprint, **… → Scope changes** opens the same view in a dialog for that sprint's board, with that sprint highlighted and its burndown shown. The sprint is always in the table, even when it's older than the last 6 closed sprints; while it's computing, the burndown shows another sprint and switches to it when it's ready. A future sprint has nothing to show yet.

<a id="gadget"></a>
## Dashboard gadget

**The Sprint scope gadget puts the view on a Jira dashboard.** Its settings choose:

- **One board** (the sprint table), or **several boards**, **every board with a label** or **every board in a project** (the [portfolio](#portfolio)). A label selection picks up boards that get the label later.
- How many sprints (3, 6, 10 or 15), and issues or estimates.

No board is chosen for you. If the list is empty, you can't see any Scrum board: ask a Jira admin for access to a board's project and filter. If the boards can't be loaded, the settings say why and offer Retry.

On a dashboard the table keeps the seven columns that tell the story, with full names: [Committed](numbers.md#committed), [Taken on](numbers.md#added), [Removed](numbers.md#removed), [Scope change](numbers.md#scope-change), [Completed](numbers.md#completed), [Say/do](numbers.md#say-do) and Not done ([not completed](numbers.md#not-completed), with how much was [carried over](numbers.md#carried-over)). Re-estimated and Drift are on the board's Sprint scope page, with every other column. Above the table, the same two-part [headline](#project-page) as the page: the last closed sprint, and "Now" for the active one.

The heading names the board, with a link to open it. The answer for the active sprint (or the last closed one), with its goal, is shown above the table. The gadget refreshes with the dashboard, and **Refresh** reloads it now, in the portfolio too.

<a id="widget"></a>
## Dashboard widget (new dashboards)

**On Jira's new dashboards, the Sprint scope widget shows the same view, with the same settings as the gadget.**

- **Wide:** the full table, charts and burndown, as on the gadget. When the row is shorter than the view, it scrolls inside the widget; a Large or Extra Large row shows more at once.
- **Narrow or short** (half a row or less, under about 600 pixels, or a Small or Extra Small row): the board's name with an "Open the board" link, then:
  - **Now:** the active sprint so far, with its name and end date on a small line, then two figures: its [scope change](numbers.md#scope-change) so far and what was taken on and removed since its start;
  - the sprint goal;
  - **Scope change** and **say/do** of the last closed sprint, named in the labels;
  - **Burndown:** what's left in the active sprint, the ideal, and working days left (plus the scope change since the start, if any), in the same unit as the rest of the widget: issues, or the board's estimates when the widget is set to show them.

  The last closed sprint skips sprints with [anomaly flags](anomalies.md#left-out); with none closed yet it says "No closed sprint yet", and when every closed sprint is flagged it says so, with a link to why. One line at the bottom says the figures are counted as you see them, how many flagged sprints were left out, whether sprints are still computing and, on a dashboard with filters, that the widget doesn't use them, with a link to this section. **Refresh** reloads everything, also after an error. The full view has the velocity chart and its average.
- **Several boards, a label or a project:** the [portfolio](#portfolio). Narrow, it's one line per board: the most recent sprint's scope change and say/do, and the scope change trend (or why there's no figure). Its footer is one line too.
- **Adding it:** in **Add widget**, search for Sprint scope. The preview shows made-up example sprints, not your data; your boards appear once you choose them in the settings.
- **Refresh:** the widget reloads itself every 15 minutes while the dashboard is open and showing; a hidden tab waits until it's shown again. "Refreshed" gives the time of the last load, and **Refresh** reloads now.
- **Settings:** open the widget's settings and press the dashboard's **Save**. The view shows your changes as you make them.
- **Dashboard filters:** the widget always shows the boards in its settings. When the dashboard has filters set, the line at the bottom says "doesn't use dashboard filters", so you don't read its numbers as filtered.
- **Flagged sprints** (sprints with an [anomaly flag](anomalies.md#left-out), such as one that started empty) are left out of the widget's figures, and the line at the bottom counts them.

<a id="board-hidden"></a>
**"You can't see this board in Jira"** shows when a gadget or widget is set to a board you can't open in Jira, for example when its filter isn't shared with you. Nothing about that board is shown. To fix it, ask the dashboard's owner or a Jira admin for access to the board's filter and project. The view updates on the next load. The board lists in the settings show only boards you can see. A portfolio says how many of its boards are hidden from you, and leaves them out of its rows and totals.

<a id="reasons"></a>
## Reasons for scope changes

**Anyone who can see a board can say why an issue was taken on or removed after a sprint's start.** Numbers say what changed; a reason says why, such as "Escalation from support" or "Moved to the next release", so it travels with the figures when they're shared.

- **Where:** **Add a reason** under the change, in the drill-down's Change column and in the scope changes on the board's What changed tab. **Edit reason** opens the same form with the text, and **Clear the reason** removes it.
- **The prompt under the headline:** when the headline's sprint had work taken on or removed, a line says how many changes there were and how many have a reason ("4 changes after the start · 1 with a reason"). **Add reasons** opens What changed on that sprint. Selecting the headline's "+3 / −1" lists those changes right under the headline, with any reasons, without leaving the top of the page; reasons are added in What changed.
- **Who:** anyone who can see the board and the issue can write, change or clear a reason. Each reason shows who last wrote it and when ("— Ana, 3 Oct").
- **Up to 500 characters.** Plain text.
- **Where it travels:** the Issues export (`reason`, `reasonBy`, `reasonAt`), snapshots, and the sprint slide's change table. Not the copied summary, which stays one line per sprint, and not BI data.
- **Stored** in Atlassian's storage for your site, with the author's account ID, and deleted with the issue's sprint data or when the app is uninstalled. See [privacy](privacy.md).

<a id="outside-team"></a>
## Work from outside the team

**A board's admins can switch on a guess at which work taken on after a start came from outside the team.** It's off by default, because it isn't right for every team: some teams work across several projects by design.

- **Switching it on:** **Board settings → Work from outside the team** on the board's Sprint scope page, for the board's admins and Jira admins. Each signal can be switched on or off:
  - **From another project:** the issue is in a project that isn't one of the board's.
  - **Linked to another project:** the issue is linked to an issue in a project that isn't one of the board's.
  - **Reporter not in the project** (off by default): the person who reported the issue isn't in a role on any of the board's projects. Sprint Pack reads the projects' roles, groups included, and keeps the list for six hours. If it can't read them, this signal says nothing and the setting says so.
- **Where it shows,** only while it's on: a "from outside the team" label with its reason ("linked to PLAT-12") in the drill-down and in "What changed after the start"; "of which 2 from outside the team" under the headline's taken-on figure; the [summary](exports.md#copy-summary) and the sprint slide ("3 issues taken on (2 from outside the team)"); the portfolio's Taken on column; and the Issues export's `outsideTeam` and `outsideReason` columns.
- **When it updates:** reporters and links are read when Sprint Pack reads the issue: at once when it moves between sprints, otherwise at the hourly check, so a link added later shows within the hour. Corrections and the board's setting show on the next load.
- **It's a guess, so anyone who can see the board can correct it:** **Not from outside** or **From outside the team** under the issue's change, and **Back to the guess** to undo. A correction names who made it and wins over the guess.
- **Not in BI data or the JQL functions** for now.

<a id="drill-down"></a>
## Drill-down

**Select any count to see the issues behind it and the JQL search that finds them.** The list opens directly under the table, headed by the bucket, the sprint and the number of issues, 25 to a page.

- **The query** is shown above the list, with a copy button, "Open in search" (Jira's issue search, as you), "All issues in <sprint>" (every issue in the sprint, `inSprint`) and a link to the function's help. It's a normal search: save it as a filter, use it on a board or in another gadget.
- **Each issue:** key, summary, status, and what happened: "Taken on 3 Oct 10:15 by Ana", "Removed …", "Carried to Sprint 15", or "Committed at start". Work taken on or removed shows its [reason](#reasons), if one was given, with **Add a reason** or **Edit reason**.
- **Estimates:** the value at the start (or when the issue was taken on) → the value at the close, or now while the sprint is open, with the change and percentage; "none" if it had no estimate; "removed" if the estimate was cleared. On time boards, time logged.
- **Re-estimated:** how many times the estimate changed during the sprint, and before the start if it was refined then.
- **Show estimate history:** every change with when, who, and whether it was during the sprint.
- The list shows every issue in the bucket that you can see, however many there are.

<a id="charts"></a>
## Charts

Under the table, four charts, one under another, over the same sprints (computing sprints are left out), each running oldest to newest, left to right (the table runs newest first):

- **Scope change % per closed sprint** comes first, since it's the app's main figure: how much the plan moved after the start, including requests taken on, against the commitment; it can pass 100%. A grey line shows the board's average over the closed sprints shown, counted as the [portfolio](#portfolio) counts it (flagged sprints left out). The axis starts at 0 and stops a little above the highest figure (at least 20%), so a board's ups and downs are visible. The active sprint's scope change so far is in the headline's "Now", not on the line, where a figure that isn't final read as a drop. See [scope change](numbers.md#scope-change).
- **How scope moved after each start:** for each sprint, what was committed at the start (blue), what was taken on after it, stacked above (purple), and what was removed after it, drawn below zero (orange). A sprint that [started empty](anomalies.md#nothing-committed) has nothing to compare its work against, so its work is grey rather than purple, and the sentence under the chart names it. Carried-over work is an outcome, not a change of scope, so it's in the table rather than this chart.
- **Velocity: committed vs done:** three bars per sprint: committed at the start, done of what was committed, and taken on after the start and done. The last two add up to the Completed column. The subtitle gives the [average completed](numbers.md#velocity) (both parts), leaving out the active sprint and flagged sprints.
- **Say/do % per sprint:** on a 0–100 axis, the share of the work committed at the start that was finished, for closed sprints only; say/do is shown once a sprint closes. See [say/do](numbers.md#say-do).
- **Short labels:** under the bars, the words every sprint name shares are dropped ("Checkout Sprint 6" shows as "Sprint 6"), so the labels aren't cut off. The sentences under the charts use full names.

- **One colour per meaning, in every chart:** blue is the plan (committed, remaining), green is done (done of committed, completed, say/do), teal is done so far in the active sprint, purple is work taken on (taken on, taken on and done, scope, scope change), orange is work taken out (removed, carried over), and grey is a reference: the ideal line, the board's average, and sprints that started empty. [Slides](exports.md#slides) use the same colours.
- **The active sprint** is in the scope and velocity bars, labelled "(now)", because its figures aren't final; it isn't on the say/do or scope change lines. Its done-of-committed bar is "Done so far", in teal. In slides, its bars are hatched and its line points hollow.
- **No chart without points:** when no sprint shown had work committed at its start, there are no ratios to draw, and only the sentence saying so appears.
- Approximate sprints are marked in the table, not in the charts.
- Charts follow the issues/estimates toggle. Hours are shown for time-estimate boards.
- **Under each chart,** one sentence gives its reading, what the chart shows that the headline and the chart's subtitle don't: where the last sprint's scope change sits against the board's average, which sprint had the most work taken on and the most removed, how far done of committed ranged ("Done of committed ranged from 4 to 9 issues over 4 closed sprints; work taken on after the start was done in 2 of them"), and the range of say/do. They're there for screen readers and at a glance. The burndown has one too.

<a id="portfolio"></a>
## Sprint portfolio

**Apps → Sprint portfolio compares boards: each board's most recent closed sprint, side by side, led by its scope change, with the board's own average and trend under each figure, each counted as you see it.**

Boards differ in size, estimation and kind of work, so the trend compares each board with its own past rather than with the others. A line under the controls says so, with our support address: tell us how you compare boards.

- **Choose:** boards, a label or a project; the last N sprints per board, or sprints closed between two dates; issues or estimates; and grouping by label or project. Show. Your choice is remembered for next time.
- **Anyone who can see a board can add it.** Every Scrum board you can see is in the pickers. Rows appear as each group of ten arrives.
- **More than 50 boards** are shown 50 at a time, with **Next** and **Previous**. Every figure on the page, the group medians included, covers the 50 shown, and the page says which.
- **Each board's name** opens that board's full Sprint scope view on this page, with a link back to the portfolio; **Open in Jira** next to it opens the board in Jira.
- **First visit:** the page says what it does and offers to choose every board you can see; press Show to load them.
- **Columns:**
  - **Most recent sprint:** the closed sprint that closed last, without an [anomaly flag](anomalies.md#left-out), with its close date. With none, the cell says why: "no closed sprint yet", or "6 flagged, left out" (a link to [why](anomalies.md#left-out)); the figures beside it are "–".
  - **Scope change, Taken on, Say/do:** that sprint's [scope change](numbers.md#scope-change) (in semibold), the work [taken on](numbers.md#added) after its start (in the board's unit), and its [say/do](numbers.md#say-do). Under Scope change and Say/do, a small line gives the board's own average over the selected closed sprints (the ratio of sums) and its [trend](#portfolio-trend), for example "avg 18% · ↑ up"; hover it for what it compares.
  - **In progress:** the active sprint in words, for example "6 done of 8 committed · 2 taken on · 1 removed" (or "6 done, nothing committed"), and the sprint's name.
  - **Sprints:** closed sprints counted. Flagged sprints left out are said once per board: a "2 flagged sprints left out" label under the board's name when the board has figures, or the Most recent sprint cell's "6 flagged, left out" when it has none.
  - The averages of velocity ratio, scope change (with its trend) and carry-over are in the snapshot's Portfolio sheet and the group rows. See [velocity ratio](numbers.md#velocity-ratio), [carry-over](numbers.md#carry-over-rate).
  - **CSV / Excel:** that board's exports.
- **Labels on a board row:** "≠ published" (the published totals differ from yours, because they count issues you can't see), "settings unreadable", "computing N", and anomaly flags. Each label links to its explanation.
- **Grouped:** each label or project gets a row with the median (and mean) of its boards' ratios, issue counts added up, and point totals only when every board uses the same unit, otherwise "mixed units". A board with several labels appears under each; boards with none sit under "(no label)". See [units](numbers.md#units).
- **Slides and summary:** "Download for slides" and "Show a summary to copy" work here too (see [exports](exports.md#slides)), in the table's order: each board's most recent sprint, then its average. The summary starts with the median scope change of the most recent sprints, and gives each board's scope change and say/do averages with their trends. The slide's table has Scope change and Avg scope change beside Say/do and Avg say/do, with the trend word in each average cell. Under the trend chart, one sentence gives each board's most recent figure and counts the boards up, steady and down, by the same rule as the table.
- **When boards don't load:** if a group of boards fails to load, for example because Jira was busy, the error names those boards and offers Retry, which asks again for just those boards. They're left out of the table and the group medians rather than shown with old or empty figures.
- **Large selections and the hourly budget:** checking which issues you can see costs about a point of Sprint Pack's [hourly budget for views](admin.md#budget) per issue. When a group of boards needs more than is left this hour, the error says how many points it needs; choose fewer boards or sprints, or try again after the top of the hour.
- **While sprints compute,** the page asks again every 20 seconds, for the boards still computing only.
- **What's missing travels with the figures:** the summary, the slide footer and the snapshot cover each say how many boards were hidden from you, still computing, or failed to load.

<a id="portfolio-trend"></a>
### Trend

**A trend compares the most recent figure with the board's own average,** for scope change and for say/do separately: "↑ up" more than 5 percentage points above, "↓ down" more than 5 below, "→ steady" within 5. Up isn't good or bad by itself: a scope change trending up means the plan moved more than usual. Flagged sprints are left out of both sides.

**The trend chart** (shown when 8 or fewer boards are selected) draws each board's scope change per closed sprint, or its say/do (the **Chart** choice above it), counted back from its last closed sprint ("last", "−1", "−2" …), oldest to newest. Flagged sprints are left out, as in the table.

<a id="burndown"></a>
## Burndown

**The What changed tab tells one sprint's scope story: what changed after its start, who changed it and why, then the burndown, rebuilt from the same list of changes Jira's burndown chart uses.**

- **Pick a sprint:** the sprint the headline leads with by default (the last closed one, or the one you opened from its menu). The burnup toggle beside the Burndown heading shows completed against scope instead.
- **First, "Scope changes in <sprint>":** each issue taken on, removed or re-estimated after the start: when, which issue with its summary and type, what changed, who made the change in Jira ("Changed in Jira by"), and what was remaining after it. Work taken on or removed has room for its [reason](#reasons). **Show completions and reopens too** adds the moves into and out of the last column. With none, it says "No scope changes after the start", and the burndown still shows. Links under the list open the matching `addedAfterSprintStart` / `removedAfterSprintStart` / `reestimatedInSprint` searches. An add or a removal names the person who last added or removed that issue after the start; a status change has no name. On a dashboard, the latest five scope changes are listed.
- **Then the burndown:** the sprint's remaining work per day against the ideal line.
- **Issues or estimates:** the burndown follows the view's switch: issue counts while it's off, the board's estimates (points or hours) while it's on. The title names the unit. The widget's burndown tile and the [published burndown](bi.md#publishing) use the board's estimates.
- **"So far":** the last point of an active sprint is today's value so far, and the subtitle says how much is left, the ideal, and the working days left.
- **Nothing committed at the start:** there's no ideal line to burn down against, so it isn't drawn; the sentence under the chart says so.
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
