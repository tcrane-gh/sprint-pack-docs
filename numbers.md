---
title: How the numbers are counted
---

<!-- Status: written 2026-10-08 (clarity pass C1). -->

# How the numbers are counted

Sprint Pack counts each sprint the way Jira's Sprint Report does, from each issue's history. This page defines every number, says whose view it counts, and lists the cases where a number is less certain.

The same definitions apply on every surface: the Sprint scope views, the exports, the slides, the Sprint portfolio and the published BI data.

<a id="counted-as"></a>
## Whose view is counted

**Each view counts the issues the person looking at it can see.** Two people with different permissions can see different numbers for the same sprint.

| Surface | Counts | Label |
|---|---|---|
| Sprint scope views (project page, sprint menu, dashboard gadget and widget), the Sprint portfolio | Issues the viewer can see | "Counted as you see them" |
| Exports, slides, the copy summary, snapshots | Issues the person who made the file could see | "As visible to you on <date>", or "Counted as <name>" |
| Published BI data (board properties) | Issues the app can see | `countedAs: "app"` |
| BI issue lists (script `--issues`, Sheets `Issues` tab) | Issues the reader's credential can see | `countedAs: "reader"` |

- A file says who it was counted as. A [snapshot](exports.md#snapshot) also links each count to its live Jira search, so whoever receives it can re-run the counts under their own permissions.
- Sprint Pack checks what you can see each time you load a view. The answer is reused for five minutes.

<a id="parity"></a>
## Parity with Jira's Sprint Report

**The scope view matches Jira's Sprint Report as you see it, as long as Sprint Pack can see every issue on the board.**

What makes the numbers match:

- **The same start.** "After the start" means after the sprint's declared start date, the one entered when the sprint was started. A change in the same second as the start counts as before it.
- **The same close.** "At close" means the last close, after any reopens. The report doesn't change after the close.
- **The same "done".** Done means the board's last column at the close, inside the board's filter. Sprint Pack saves the board's columns when a sprint closes.
- **The same exclusions.** Sub-tasks are left out, and issues already done at the start are counted as "completed outside of this sprint", not as committed or completed. See [done at start](#done-at-start) and [sub-tasks](#sub-tasks).
- **The same estimate field.** Points come from the board's current estimation field, at the moments the report uses. See [units](#units).

When the numbers can differ:

- Issues **you** can't see: left out of both, so parity holds. See [hidden issues](#hidden-issues).
- Issues **Sprint Pack** can't see: missing from Sprint Pack only. See [issues the app can't see](#app-unseen).
- Sprints that closed before Sprint Pack saw them close: completion is [approximate](#approximate).
- A board whose settings Sprint Pack can't read: nothing counts as completed. See [settings unreadable](#settings-unreadable).

<a id="hidden-issues"></a>
## Hidden issues: ones you can't see

Issues you don't have permission to view are left out of your counts. Jira's Sprint Report leaves them out for you too.

- The view shows how many were left out, for example "3 issues hidden from you".
- **A rare exception:** Sprint Pack tells which issues you can see by comparing counts. If one issue is hidden from you and a different one is hidden from the app (by a security level that leaves the app out), the counts match and your figures include the issue you can't see. Its key and summary are never shown.
- On a [published](bi.md#publishing) board, the view also shows the published totals where they differ ("Published to BI: 18 committed, 15 completed"), and the portfolio marks the board "≠ published". Published totals count what the app can see, which can be more than you can.

<a id="app-unseen"></a>
## Issues the app can't see

An issue with a security level that leaves Sprint Pack out isn't counted anywhere in Sprint Pack: not in the views, the exports or the published totals.

- **Effect:** for anyone who can see those issues, Sprint Pack's numbers are lower than Jira's Sprint Report. The view says how many issues you can see that the app doesn't count.
- **The JQL functions that only read sprint and status history** (`completedInSprint`, `incompleteInSprint`, `carriedOverFromSprint`, `inSprint` and the date functions) are plain Jira searches run as you, so they still find those issues. The functions that read Sprint Pack's own fields (`addedAfterSprintStart`, `removedAfterSprintStart`, `reestimatedInSprint`, and `committedAtSprintStart`, which uses the added and removed fields) can't.
- **Fix:** include Sprint Pack in the issue security level, if that's acceptable for your site. The admin page says whether the app can see any secured issues. Read [issue security](bi.md#issue-security) before publishing such a board.

<a id="buckets"></a>
## The buckets

Every sprint is split into the same buckets. Each is a Jira search you can open (see [the JQL functions](jql.md)).

| Bucket | In short | Short header (dashboard) |
|---|---|---|
| [Committed](#committed) | In the sprint when it started | Start |
| [Taken on](#added) | Taken on after the start | Add |
| [Removed](#removed) | Removed after the start | Rem |
| [Completed](#completed) | In the last column at close | Done |
| [Not completed](#not-completed) | Still in the sprint at close, not in the last column | Open |
| [Carried over](#carried-over) | Not completed and moved to a later sprint | Carry |
| [Re-estimated](#re-estimated) | Estimate changed during the sprint | Re-est |

Two more appear in files and BI data:

- **Completed from the commitment** (`completedFromCommitment`): completed issues that were also committed. It drives [say/do](#say-do).
- **Completed outside the sprint** (`completedOutside`): issues already done at the start that stayed done. See [done at start](#done-at-start).

An issue can be in several buckets: taken on and completed, or committed, not completed and carried over.

**Counts and points.** Every bucket has an issue count and, on a board with estimates, a point total. Points use the estimate at a fixed moment:

| Bucket | Estimate taken |
|---|---|
| Committed | At the start (also for issues removed later) |
| Taken on | When first taken on |
| Removed | At the start for committed issues, when taken on for the others |
| Completed, not completed, carried over | At the close (the current value while the sprint is active) |

<a id="committed"></a>
### Committed

Issues in the sprint at its start, except issues already in the board's last column then.

- An issue created directly in the sprint before the start counts.
- An issue removed later still counts as committed (and as removed).
- Issues already done at the start are left out. See [done at start](#done-at-start).

<a id="added"></a>
### Taken on after the start

Issues that joined the sprint after its start and before its close (or now, while it's active): requests the team took on once the sprint was under way.

Exports, BI data, the [`addedAfterSprintStart()`](jql.md#addedaftersprintstart) function and the Jira field "Added after sprint start" still call it "added" (`issues_added`, `addedAt`), so files and searches you already use keep working.

- Issues created directly in the sprint after the start count.
- Issues carried into this sprint from another sprint's close, while this one is active, count.
- **Net change:** an issue removed after the start and put back before the close is neither taken on nor removed.

<a id="removed"></a>
### Removed

Issues that left the sprint after its start and were still gone at the close.

- An issue taken on after the start and removed again is both taken on and removed.
- Changes Jira writes when a sprint is reopened or deleted aren't removals: they don't count. The [burndown](views.md#burndown) still lists them, because it follows Jira's own burndown chart, which does.
- **Limit for sprints computed from history:** to find an issue that left a sprint before Sprint Pack was watching, the app searches the sprint's board filters. An issue that has since left every board filter (moved to another project, or the filter edited) can't be found, so it's missing from Removed. Sprints Sprint Pack saw happen live aren't affected.

<a id="completed"></a>
### Completed

Issues in the board's last column at the sprint's close, inside the board's filter.

- **Done at the start doesn't count:** an issue already in the last column at the start and still there at the close is "completed outside of this sprint". An issue done at the start, reopened and done again during the sprint is completed in it.
- **While the sprint is active,** completed means in the last column now ("so far"), inside the board's filter, as at the close. The views read the active sprint's statuses when they load (at most a minute old). If Sprint Pack's hourly budget for views is used up, they use the statuses from the last hourly update and say so; **Refresh** after the top of the hour.
- The board's columns are the ones Sprint Pack saved at the close. For a sprint that closed before Sprint Pack saw it, today's columns are used: see [approximate](#approximate).
- **Completed = committed and done + taken on and done.** The views show the two parts as "9 + 2" when work taken on after the start was done. The first part is [completed from the commitment](#buckets), which drives [say/do](#say-do). The drill-down searches are `issue in completedInSprint(X) AND issue in committedAtSprintStart(X)` and `issue in completedInSprint(X) AND issue not in committedAtSprintStart(X)`.

<a id="not-completed"></a>
### Not completed

Issues still in the sprint at its close and not in the board's last column then. While the sprint is active: in the sprint and not in the last column now.

<a id="carried-over"></a>
### Carried over

Issues not completed at the close that are in a later sprint on the same board: one that started after this sprint, or a future sprint.

- An issue sent back to the backlog at the close is not completed but not carried over.
- The field **Times carried over** is different: it counts the closed sprints an issue was still in, and not done, at the close, over the sprints Sprint Pack has computed only. See [fields](jql.md#fields).

<a id="re-estimated"></a>
### Re-estimated

Issues whose estimate, on the board's estimation field, changed while they were in the sprint: between the start (or the moment they were taken on) and the close.

- A change before the start is "refined before start", not a re-estimate.
- Logging work changes the remaining estimate, never the original, so it's never a re-estimate.
- The drill-down shows each change with when, who, and whether it was before or during the sprint ("Show estimate history").
- **Re-estimate rate** (BI marts): issues re-estimated during the sprint ÷ (committed + added).
- There is no per-person ranking.

<a id="drift"></a>
### Drift

**Drift is how much the sprint's estimates moved: the estimate at the close minus Jira's value, summed over the sprint's work.**

- **Jira's value** is the estimate the reports count: at the start for a committed issue, when first taken on for one taken on later.
- **At the close** means the estimate at the last close, or the current value while the sprint is active.
- **The sprint's work** is the issues in the sprint at the end, except ones done at the start.
- Issues with no estimate at the start are left out of drift; they're counted separately (see [unestimated](#unestimated)).
- Shown in the board's unit (hours on a time-estimate board). Files also carry the absolute drift (the sum of the sizes of the changes), the number of issues whose estimate was removed, and, on time boards, time logged against the original estimate.
- For story points, drift measures **stability, not accuracy**.

<a id="unestimated"></a>
### Unestimated at the start

An issue with no estimate at the start (or when it was taken on) is its own category, never an infinite percentage.

- The table shows the count beside the drift: "(2 unestimated at start)". On a dashboard, `*` after the drift means some issues were unestimated at the start; hover for the count.
- A sprint that's mostly unestimated isn't flagged as an anomaly.

<a id="say-do"></a>
## Say/do

**Say/do = completed from the commitment ÷ committed.** It answers "of what we committed to at the start, how much did we finish?"

- **Say/do is shown once a sprint closes.** While a sprint is active, the views, the summary and the slides give its progress instead ("5 of 11 committed done so far"), and the say/do chart leaves it out.
- Work taken on after the start doesn't count towards say/do, even if completed. The [velocity ratio](#velocity-ratio) includes it.
- Counted in issues or in estimates, following the view's toggle.
- A sprint with nothing committed has no say/do: it shows "—", never 0%. A closed sprint like that is also flagged [nothing committed](anomalies.md#nothing-committed).
- In BI data, `sayDo` uses estimates and `sayDoIssues` uses issue counts. In the Sprints export, `sayDo` uses estimates, so it's empty on a board without estimates; work it out from `issues_completedFromCommitment ÷ issues_committed`.

<a id="scope-change"></a>
## Scope change

**Scope change = (taken on + removed) ÷ committed.** It can be well over 100%.

A big scope change is never flagged as an anomaly.

<a id="velocity"></a>
## Velocity

The velocity chart shows, per sprint, what was committed at the start, how much of that was done, and how much work taken on after the start was done. The last two add up to completed, the bar in Jira's velocity chart.

- **The average** in its title is the mean of completed over the closed sprints shown. The active sprint is left out, and so are [sprints with anomaly flags](anomalies.md#left-out); the note says how many were left out.
- The narrow widget shows the same average as a tile.

<a id="velocity-ratio"></a>
## Velocity ratio

**Velocity ratio = completed ÷ committed.** Unlike say/do it includes work taken on after the start that was completed, so it can be over 100%. It's the ratio behind Jira's velocity chart.

<a id="carry-over-rate"></a>
## Carry-over rate

**Carry-over = carried over ÷ (committed + taken on).** The share of the sprint's work that moved to a later sprint.

**Averages across sprints** (in the portfolio and BI marts) are ratios of sums over the selected closed sprints: a small sprint doesn't weigh as much as a full one. Across boards, see [units](#units).

<a id="approximate"></a>
## Approximate (≈)

**"≈" marks a count that may be slightly off because Sprint Pack didn't see the sprint close.**

- **Which sprints:** sprints that closed before Sprint Pack was installed, or whose close event Sprint Pack missed.
- **Why:** Sprint Pack saves the board's columns and filter at the moment a sprint closes. Without that, it judges "done" against the board's **current** columns and filter. If the columns or the filter changed since, the completion counts can be off.
- **Which numbers:** only the ones that depend on "done at the close": Completed, Not completed and Carried over, plus Completed outside the sprint and [say/do](#say-do), which are built from them. Those cells get "≈".
- **Not affected:** Committed, Taken on and Removed come from the issues' history and are exact.
- A line under the table says how many sprints are approximate. Files carry an `approximate` column (true or false) for each sprint.
- Nothing to do: if the board's columns haven't changed since, the numbers are right.

<a id="computing"></a>
## Computing

**"Computing" means one thing everywhere: Sprint Pack hasn't yet computed this sprint under its current rules.**

- **When it happens:** a sprint shown for the first time (older sprints are computed when first asked about), and every sprint after an update that changes the counting rules, until it's recomputed.
- **What you see:** the row shows "computing" (with progress, such as "120 of 400", when known) and no numbers. Charts leave the sprint out. JQL functions that need it answer with an "indexing" message (see [search errors](jql.md#search-errors)).
- **It clears by itself.** Views refresh themselves while anything is computing.
- <a id="updating"></a>**"Updating"** marks a sprint being computed again after an update to the counting rules or a [rebuild](admin.md#rebuild). It keeps its earlier numbers until the new ones are ready, and exports leave it out until then.
- **How long:** usually minutes. On a big site, work runs within an hourly [budget](admin.md#budget): right after install or a rules update, history can take days to fill in, newest sprints first (see [backfill](admin.md#backfill)).
- The admin page counts sprints computing or queued, older sprints that wait for someone to view them, and stuck sprints (no progress for over 2 hours). Only stuck ones are listed under "Needs attention", each with a [rebuild](admin.md#rebuild).

<a id="history-refreshing"></a>
## History refreshing

The burndown needs each issue's sprint and status history. For issues Sprint Pack recorded before it kept that history, the burndown uses the moments it already knows (when the issue was taken on or removed) and its current status instead.

- The chart says "history still being refreshed for some issues", and the narrow widget shows "history refreshing".
- It clears when the sprint is recomputed under the current rules. Nothing to do.

<a id="estimation-change"></a>
## When a board's estimation changes

**When a board's estimation field changes, Jira recalculates every past sprint with the new field, and so does Sprint Pack.**

- The view shows a warning with the date and the old and new fields.
- A change during a sprint flags that sprint [estimation field changed](anomalies.md#estimation-changed-mid-sprint).
- **Turned on time tracking by accident?** In a team-managed space, turning on time tracking can switch the board to Original estimate and zero past velocity. Switch it back under Board settings → Estimation → Story point estimate; Sprint Pack re-derives every sprint.

<a id="settings-unreadable"></a>
## Board settings Sprint Pack can't read

**If Sprint Pack can't read a board's settings, it doesn't know the board's columns, so nothing counts as completed and the totals are not Jira's.**

- **Cause:** on a company-managed board, the board's filter isn't shared with the app.
- **Where it shows:** a warning on the Sprint scope view, "settings unreadable" in the portfolio, an error from the completion functions (see [search errors](jql.md#search-errors)), the published data's notices and the BI handoff.
- **Fix:** share the board's filter with its project, or with a group that includes the app (filter details → Edit permissions). Sprint Pack re-reads the settings within six hours, or at once after the next change to the board.
- `committedAtSprintStart` still works on such a board, without the done-at-start rule.
- **A board Sprint Pack can't see at all:** when the filter isn't shared, Jira usually leaves the board out of the app's board list too. Its sprints are still recorded, but searches with no board argument and the portfolio leave the board out. The admin page lists it under "Boards Sprint Pack can't see"; after sharing the filter, **Check again** there reads the board list at once.

<a id="done-at-start"></a>
## Done at the start

An issue already in the board's last column when the sprint started is, in the Sprint Report's words, "completed outside of this sprint".

- It's **not committed** and **not completed**, if it stayed done. It's counted as completed outside the sprint in files.
- If it moved out of the last column and back during the sprint, it's completed in the sprint.
- **The burndown is different, as Jira's is:** issues done at the start are in the burndown's scope (not in what's remaining).

<a id="sub-tasks"></a>
## Sub-tasks

Sub-tasks are left out of every count and every JQL function, as Jira's Sprint Report and burndown leave them out. A sub-task belongs to its parent's sprint.

<a id="units"></a>
## Units

A board estimates in one of three ways, and every point total is in that board's unit:

- **Story points** (or any number field): shown as points.
- **Original estimate** (time): stored in seconds, shown and exported as hours.
- **Issue count:** no point totals; everything is in issues.

The toggle on each view switches between issue counts and the board's estimates.

**Across boards** (the portfolio, slides, BI marts):

- Ratios (say/do, velocity ratio, scope change, carry-over, re-estimate rate) are computed per board, then shown across boards as the median and the mean.
- Issue counts can always be added up.
- Point totals are added up only when every board in the selection uses the same unit. Otherwise you see "mixed units".
