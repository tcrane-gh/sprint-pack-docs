---
title: Anomalies
---

<!-- Status: written 2026-10-08 (clarity pass C3); start moved clears when the start date is corrected, approved 2026-10-09; a wrong inferred reopen can be dismissed, approved 2026-10-09. -->

<a id="anomalies"></a>
# Anomalies

**Sprint Pack flags a sprint when its data is wrong or unusual in a way that would distort averages: dates in the wrong order, a start date moved by days, a sprint closed within an hour, a sprint started empty, a reopen, or an estimation field changed during the sprint.**

Flags are about bad data, not performance. A sprint with a 450% scope change, a low say/do or many carry-overs is not flagged. See [why flagged sprints are left out](#left-out).

**Where flags show:**

- One grey label per flagged sprint row, such as "dates reversed", linking to its section below. A sprint with several flags shows the first and "+1". The label's tooltip says the sprint is left out of averages, so they reflect the team's sprints rather than this data, and gives the fix.
- Exports and snapshots: an `anomalies` column, with the codes below joined by `; `.
- Published BI data: the same `anomalies` column.
- The admin page lists flagged sprints under "Needs attention".

Flags are worked out each time a view or rollup is built, so fixing the data in Jira clears the flag.

<a id="dates-reversed"></a>
## Dates reversed

Code: `dates-reversed`.

**The sprint's start date is after its close date, or, for a sprint that hasn't closed, after its planned end.**

- Usually a typo when the sprint was started or edited, or a sprint started with a far-future date.
- Effects: the burndown has nothing to draw ("Jira records this sprint's start after its close"), and "after the start" means very little.
- Fix: correct the sprint's dates in Jira where Jira allows it; the flag clears on the next refresh. If the dates can't be changed, the flag stays and the sprint stays out of averages.

<a id="start-moved"></a>
## Start date moved

Code: `start-moved`.

**The sprint's start date differs by more than 24 hours from the moment it was actually started.**

- The start date is the one entered in Jira's "Start sprint" dialog, or set later by editing the sprint. It can be backdated ("we forgot to press Start on Monday") or set in the future.
- Sprint Pack counts from the start date, as Jira's Sprint Report does. With a future start, issues added between pressing Start and the start date count as committed, not taken on. With a backdated start, issues added before pressing Start but after the start date count as taken on.
- **Only flagged when Sprint Pack saw the sprint start.** Never set for sprints started before Sprint Pack was installed.
- Fix: edit the sprint in Jira and set its start date to within a day of when it was really started. Sprint Pack then counts committed and taken on from the corrected date, and the flag clears on the next refresh. If Jira won't let you change the dates, for example after the sprint has closed, the flag stays and the sprint stays out of averages.

<a id="very-short"></a>
## Closed within an hour

Code: `very-short`.

**The sprint closed less than an hour after it started.**

Usually a test sprint, or a sprint started and closed by mistake.

- Fix: none in Jira, which doesn't let you change a closed sprint's dates. A test sprint is the reason the flag exists, so it stays, and the sprint stays out of averages while remaining in the table and exports.

<a id="nothing-committed"></a>
## Started empty

Code: `nothing-committed`.

**A closed sprint had nothing committed at its start.**

- Say/do can't be worked out, so it shows "—", never 0%.
- Common causes: the sprint was started empty and filled afterwards, or every issue was already done at the start (see [done at the start](numbers.md#done-at-start)).
- Only closed sprints are flagged.
- The flag looks at all of the sprint's issues, not only the ones you can see. A sprint whose committed issues are all hidden from you shows 0 committed, but it isn't flagged.
- Fix: the issues were added at or after Start, so a start date set after the adds would make them committed and clear the flag. Jira doesn't let you change a closed sprint's dates, though, so for a closed sprint the flag stays and the sprint stays out of averages. Next time, add the issues before pressing Start, or set the start date to when the work really began.

<a id="reopened"></a>
## Reopened

Code: `reopened`.

**The sprint was closed and then reopened.**

- **Observed:** Sprint Pack saw the reopen happen.
- **Inferred:** the reopen happened before Sprint Pack was installed, and Sprint Pack worked it out from the issues' sprint history: many issues gaining a later sprint at one moment (the close), then losing this sprint at another (the reopen). Inferred reopens say "inferred", and the admin page lists them so you can check them against the Sprint Report. See [inferred reopens](admin.md#inferred-reopens).
- Changes Jira writes at a reopen don't count as removals, and "at close" means the last close.
- Fix: an inferred reopen can be wrong, for example when many issues were moved between sprints at once. If the Sprint Report shows no reopen, a Jira admin (on the admin page) or one of the board's admins (in the board panel on the Sprint scope page) can dismiss it, and the flag clears. See [inferred reopens](admin.md#inferred-reopens). An observed reopen really happened, so it stays.

<a id="estimation-changed-mid-sprint"></a>
## Estimation field changed

Code: `estimation-changed-mid-sprint`.

**The board's estimation field changed between the sprint's start and its close.**

- Jira and Sprint Pack recount every past sprint with the board's current field, so this sprint's estimates are in a field the team wasn't using for all of it.
- The view also shows a warning naming the old and new fields. See [when a board's estimation changes](numbers.md#estimation-change).
- Fix: none. Changing the field back is a second change during the sprint, so the flag stays. The sprint's issue counts are unaffected; only its estimates are in a field the team didn't use for all of it, which is why it stays out of averages.

<a id="left-out"></a>
## Why flagged sprints are left out of averages

**Flagged sprints stay in every table and export, with their flag, but they're left out of averages, medians, trend arrows and the "last say/do".**

- **Left out of:** the velocity chart's average, the narrow widget's tiles, the portfolio's averages, medians and trend arrows, and the portfolio's "Last say/do", which uses the last **unflagged** closed sprint (its name is in the tooltip).
- **Kept in:** the sprint table, the charts' bars and lines, the drill-downs, the exports and snapshots (with an `anomalies` column), and the published BI data.
- A note says how many were left out, such as "(1 sprint left out)", and links here.

**Why:** one bad sprint can dominate an average. A sprint with its start after its close once produced "Last say/do 0% ↓" as a portfolio headline: a number about a data mistake that read like a verdict on the team. Leaving it out of summaries keeps the headline about the team's work; keeping it in the table keeps it visible and fixable.

**Not flagged either:**

- **Mostly unestimated sprints:** shown in the [drift](numbers.md#unestimated) column instead.
- **Closed sprints with no close date:** no view shows them; the admin page lists them instead. See [closed without a date](admin.md#closed-without-date).
