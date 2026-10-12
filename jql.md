---
title: JQL functions and fields
---

<!-- Status: written 2026-10-08 (clarity pass C1, C5). -->

<a id="jql"></a>
# JQL functions and fields

**Sprint Pack adds 12 JQL functions and 8 read-only fields to Jira. Every number in Sprint Pack's views is one of these searches, and you can use them anywhere Jira takes JQL: issue search, saved filters, board quick filters, card colours, swimlanes and dashboards.**

- Write each function as `issue in f(...)` or `issue not in f(...)`.
- No setup is needed: the functions work as soon as Sprint Pack is installed. The fields need adding to screens to be visible on issues (see [fields not showing](admin.md#fields-not-showing)).
- **Sub-tasks** are left out of every `issue in` form except `duringSprint` and `statusChangedDuringSprint`, which go by dates and statuses and can return them. An `issue not in` form returns everything the matching `in` form doesn't, sub-tasks included; add `AND issuetype not in subTaskIssueTypes()` to leave them out.
- From any count in a Sprint Pack view, the [drill-down](views.md#drill-down) shows the exact query and opens it in issue search.

| Function | Finds issues… |
|---|---|
| [`addedAfterSprintStart`](#addedaftersprintstart) | added after the sprint started |
| [`removedAfterSprintStart`](#removedaftersprintstart) | removed after the sprint started |
| [`committedAtSprintStart`](#committedatsprintstart) | in the sprint when it started |
| [`completedInSprint`](#completedinsprint) | in the board's last column at the close |
| [`incompleteInSprint`](#incompleteinsprint) | in the sprint at the close, not done |
| [`carriedOverFromSprint`](#carriedoverfromsprint) | not done at the close and moved to a later sprint |
| [`reestimatedInSprint`](#reestimatedinsprint) | whose estimate changed during the sprint |
| [`inSprint`](#insprint) | in a sprint, by name, ID or relative reference |
| [`inSprintsStartedBetween`](#insprintsstartedbetween) | in sprints that started in a date range |
| [`inSprintsEndedBetween`](#insprintsendedbetween) | in sprints that ended in a date range |
| [`duringSprint`](#duringsprint) | whose date field falls inside a sprint |
| [`statusChangedDuringSprint`](#statuschangedduringsprint) | moved to a status during a sprint |

<a id="arguments"></a>
## Arguments: sprints and boards

Most functions take `(sprint, board)`, both optional.

**The sprint argument** can be:

| You write | It means |
|---|---|
| nothing, `""`, `"active"` or `"current"` | Active sprints |
| `4123` | The sprint with that ID |
| `"Sprint 12"` | The sprint with that name (any case) |
| `"previous"` | The most recently closed sprint |
| `"previous 2"` | The second most recently closed sprint |
| `"last 3"` | The three most recently closed sprints, together |
| `"next"` | The future sprint with the earliest planned start; if no future sprint has dates yet, the first future sprint in the board's backlog order |

**The board argument** narrows the sprint to one board: a board ID such as `12`, or a board name such as `"Team A board"`.

- **Without a board,** relative references apply to each board separately: `"previous"` means every board's most recently closed sprint, and no argument means every active sprint on the site. That's what makes `issue in addedAfterSprintStart()` work as a card colour or quick filter on any board without editing it every sprint.
<a id="site-wide-skips"></a>
- **Boards that aren't ready are left out, not the whole search.** Without a board, a search returns what the ready boards give and leaves out a board when its sprint is still being computed, or (for the completion functions) when Sprint Pack can't read the board's settings. The admin page lists the boards left out, with the fix. A board still computing is added by itself when it's done: Sprint Pack evaluates the search again. With a sprint or board argument, the search gives the precise error instead.
- **A sprint belongs to the board it was created on** for relative references, even if other boards show it.
- **Quote** names and words: `"Sprint 12"`, `"last 3"`. IDs can be bare.

**Names are looked up once.** The first time a saved search with a sprint or board name runs, Sprint Pack keeps the ID the name matched, and uses that ID for the search from then on. Renaming the sprint or board later, or creating another with the same name, doesn't change the saved search. Editing the search's arguments looks the name up again.

**A name that isn't found** may be on a board created in the last few hours: Sprint Pack reads the board list again before saying the name doesn't exist.

**IDs are the safe choice.** A name that matches sprints on several boards is an error that lists their IDs.

- **Deleted sprints:** `addedAfterSprintStart`, `removedAfterSprintStart` and `reestimatedInSprint` still answer for a deleted sprint's ID, because the fields keep it. The other functions have nothing to answer for a deleted sprint (it has no issues or board columns any more), so they say so instead of returning an empty result.
- **A sprint ID Jira says doesn't exist** may be a sprint deleted before Sprint Pack saw it, or one on a board whose filter isn't shared with the app. The error says both; Sprint Pack checks again within the hour, then less often.

**Finding a sprint's ID:**

- In Sprint Pack, open any count of the sprint: the query shown in the [drill-down](views.md#drill-down) contains the ID, for example `issue in completedInSprint(4123)`.
- In Jira, the Sprint Report's address ends in `sprint=<ID>`.
- An error about an ambiguous name lists the matching IDs.

**Board IDs** are in the board's address: `…/boards/12`.

**Limits:**

- A reference that expands to more than 900 sprints (for example `"last 50"` across a large site) is an error asking for a board argument.
- Dates in the two `…Between` functions: `yyyy-MM-dd` (a whole day, in UTC), or an offset from today such as `-90d`, `-2w`, `-1m`, `1y` (days, weeks, months, years).

<a id="addedaftersprintstart"></a>
## addedAfterSprintStart

```
issue in addedAfterSprintStart(sprint?, board?)
```

**Issues added to the sprint after it started**, including issues created in it after the start and issues carried into it from another sprint's close. An issue removed and added back is not added (net change). Same rule as the [Added](numbers.md#added) column.

```
issue in addedAfterSprintStart()
issue in addedAfterSprintStart("previous", 12) ORDER BY "Last added after start" DESC
```

Uses the "Added after sprint start" field, so it can't see issues Sprint Pack isn't allowed to see (see [issues the app can't see](numbers.md#app-unseen)).

<a id="removedaftersprintstart"></a>
## removedAfterSprintStart

```
issue in removedAfterSprintStart(sprint?, board?)
```

**Issues removed from the sprint after it started** and still out at the close. Changes Jira writes when a sprint is reopened or deleted don't count. Same rule as the [Removed](numbers.md#removed) column.

```
issue in removedAfterSprintStart(4123)
issue in removedAfterSprintStart("last 3", "Team A board")
```

<a id="committedatsprintstart"></a>
## committedAtSprintStart

```
issue in committedAtSprintStart(sprint?, board?)
```

**Issues in the sprint when it started**, whether still there or removed since, except issues already in the board's last column at the start. Same rule as the [Committed](numbers.md#committed) column.

```
issue in committedAtSprintStart("Sprint 14")
issue in committedAtSprintStart("previous", 12) AND issue in completedInSprint("previous", 12)
```

The second example is the "completed from the commitment" half of [say/do](numbers.md#say-do).

<a id="completedinsprint"></a>
## completedInSprint

```
issue in completedInSprint(sprint?, board?)
```

**Issues in the board's last column at the sprint's close, inside the board's filter,** except issues done at the start that stayed done. While the sprint is active: in the last column now. Same rule as the [Completed](numbers.md#completed) column.

```
issue in completedInSprint("previous", 12)
issue in completedInSprint(4123) AND issue in addedAfterSprintStart(4123)
```

- Uses the columns saved at the close. For a sprint that closed before Sprint Pack saw it, today's columns: see [approximate](numbers.md#approximate).
- Needs the board's settings: see [settings unreadable](numbers.md#settings-unreadable).

<a id="incompleteinsprint"></a>
## incompleteInSprint

```
issue in incompleteInSprint(sprint?, board?)
```

**Issues in the sprint at its close and not in the board's last column then.** While the sprint is active: in the sprint and not done now. Same rule as the [Not completed](numbers.md#not-completed) column.

```
issue in incompleteInSprint("previous")
issue in incompleteInSprint() AND assignee = currentUser()
```

<a id="carriedoverfromsprint"></a>
## carriedOverFromSprint

```
issue in carriedOverFromSprint(sprint?, board?)
```

**Issues not done at the sprint's close that are in a later sprint on the same board** (one that started after it, or a future sprint). Same rule as the [Carried over](numbers.md#carried-over) column.

```
issue in carriedOverFromSprint("previous", 12)
issue in carriedOverFromSprint("last 3", 12) AND "Times carried over" >= 2
```

<a id="reestimatedinsprint"></a>
## reestimatedInSprint

```
issue in reestimatedInSprint(sprint?, board?)
```

**Issues whose estimate, on the board's estimation field, changed while they were in the sprint.** Changes before the start, and logged work, don't count. Same rule as the [Re-estimated](numbers.md#re-estimated) column.

```
issue in reestimatedInSprint("previous", 12)
issue in reestimatedInSprint() ORDER BY updated DESC
```

<a id="insprint"></a>
## inSprint

```
issue in inSprint(sprint?, board?)
```

**Issues in the sprint now, or at its close for a closed sprint.** It's Jira's `sprint in (…)`, with the [relative references](#arguments) Jira's own JQL doesn't have. Deleted sprints are skipped.

```
issue in inSprint("previous", 12)
issue in inSprint("last 3", 12) AND resolution is EMPTY
```

Issues removed from the sprint mid-way aren't found: use `removedAfterSprintStart` for those.

<a id="insprintsstartedbetween"></a>
## inSprintsStartedBetween

```
issue in inSprintsStartedBetween(from, to, board?)
```

**Issues in any sprint whose start date falls between `from` and `to`** (both days included).

```
issue in inSprintsStartedBetween("2026-07-01", "2026-09-30", 12)
issue in inSprintsStartedBetween("-90d", "0d")
```

Like `inSprint`, it misses issues removed mid-sprint.

<a id="insprintsendedbetween"></a>
## inSprintsEndedBetween

```
issue in inSprintsEndedBetween(from, to, board?)
```

**Issues in any sprint that ended between `from` and `to`:** the close date for a closed sprint, the planned end for an open one. So it also answers "what's planned to finish this month".

```
issue in inSprintsEndedBetween("-1m", "0d", 12)
issue in inSprintsEndedBetween("2026-10-01", "2026-10-31")
```

<a id="duringsprint"></a>
## duringSprint

```
issue in duringSprint(field, sprint?, board?)
```

**Issues whose date field falls between the sprint's start and its close** (or after the start, while it's active). `field` is one of `created`, `resolved`, `updated`, `due`, `duedate`, `statusCategoryChangedDate`, `lastViewed`.

```
issue in duringSprint("created", "previous", 12) AND project = PAY
issue in duringSprint("resolved", "active") AND issue in inSprint("active")
```

**It only compares dates; it doesn't limit the search to the sprint's issues.** Combine it with a project or `inSprint`, as above, or it matches every issue on the site with a date in that window.

<a id="statuschangedduringsprint"></a>
## statusChangedDuringSprint

```
issue in statusChangedDuringSprint(status, sprint?, board?)
```

**Issues moved to `status` between the sprint's start and its close** (or after the start, while it's active).

```
issue in statusChangedDuringSprint("In Review", "active", 12)
issue in statusChangedDuringSprint("Reopened", "previous", 12) AND issue in inSprint("previous", 12)
```

Like `duringSprint`, it doesn't limit the search to the sprint's issues: add a project or `inSprint`.

<a id="fields"></a>
## The fields

Sprint Pack writes 8 read-only fields on issues. You can show them like any field, and search and sort them as below.

| Field | Type | Value |
|---|---|---|
| **Sprint Start** | date-time | Start of the issue's most recent sprint |
| **Sprint End** | date-time | End of the issue's most recent sprint: the planned end while open, the close once closed |
| **Added after sprint start** | sprint list | Sprints the issue was added to after they started |
| **Removed after sprint start** | sprint list | Sprints the issue was removed from after they started |
| **Last added after start** | date-time | When the issue was most recently added to a sprint after it started |
| **Last removed after start** | date-time | When the issue was most recently removed from a sprint after it started |
| **Times carried over** | number | Closed sprints the issue was still in, and not done, at the close |
| **Re-estimated during sprint** | sprint list | Sprints during which the issue's estimate changed |

- **"Most recent sprint"** for Sprint Start and End: the active sprint (the latest-started, if several); otherwise the future sprint with the earliest planned start (if it has dates); otherwise the closed sprint that closed last. An issue carried into a future sprint shows that sprint's planned dates.
- **Sprint lists** show the sprint names on the issue. To search them, use the matching function (`addedAfterSprintStart`, `removedAfterSprintStart`, `reestimatedInSprint`); it handles `not in` and very long histories correctly.
- **Times carried over** counts only sprints Sprint Pack has computed. Older sprints count once they're computed. It's empty, not 0, for an issue never carried over: search `"Times carried over" is EMPTY` for those, and `"Times carried over" > 0` for the rest.
- **Searching and sorting:** the date fields and Times carried over search with the usual operators (`>`, `<`, `is EMPTY`) and sort with `ORDER BY`. The sprint lists search through their functions; `ORDER BY` accepts them, but the order isn't useful.
- **The date in a sprint list** ("Sprint 14 (3 Oct 2026)") is the day the issue was added, removed or last re-estimated, in the time zone of Sprint Pack's Jira user (the site's usual zone). Labels written before October 2026 may lack the year until the issue is next updated.
- **Very long histories:** a sprint list keeps an issue's newest 600 sprints; the functions still find the issue for older ones.
- **Field IDs differ per site.** Never hard-code one; scripts can look them up (see [BI tools](bi.md#columns)).
- **Just installed?** Jira can take a few minutes before its search knows new fields (it has taken from under a minute to over 20). Until then a search on the fields gives Jira's "field does not exist" error, and four functions ask you to wait (see [search errors](#search-errors)).

<a id="freshness"></a>
## How fresh the fields and functions are

**Fields update a median of about 12 seconds after a sprint change, and JQL sees the change about a second later.**

- Sprint Pack waits 10 seconds after a change before writing, so a burst of changes (moving ten issues at once) is written together.
- Jira sometimes delivers its events late, occasionally by a few minutes. An hourly sweep catches anything missed. A sprint start Sprint Pack never heard about can take a few hours to be picked up on sites with more than 50 boards.
- The admin page shows the measured median, 90th percentile and worst case over recent changes. See [freshness on the admin page](admin.md#freshness-admin).
- **Re-estimated during sprint** on an active sprint can lag by up to an hour.
- **Functions** read sprint dates and Sprint Pack's own records, not issues. Jira keeps a function's result and asks Sprint Pack again when sprints change.
- **Offsets from today** (`-90d` in the `…Between` functions) are counted from when the search was last evaluated. Sprint Pack re-evaluates these searches once a day, early in each UTC day, so a range can be up to a day behind.

<a id="search-errors"></a>
## When a search shows an error

**Issue search shows the error message; anything reading results over the REST API (gadgets, other apps, scripts) gets no issues and no message.** If a dashboard or script shows an unexpectedly empty result, run the same query in issue search to see why.

Jira keeps a function's error until Sprint Pack replaces it. Sprint Pack does that itself when it can; the admin page lists every stored error, grouped by what fixes it (see [failed searches](admin.md#failed-searches)).

| What it says | Kind on the admin page | What to do |
|---|---|---|
| "Sprint Pack's license isn't active…" | **Searches blocked while the license isn't active** | A Jira admin renews the license; every such search clears within the hour. See [the license](admin.md#license). |
| "Sprint Pack is waiting for Jira to make its fields searchable…" | **Waiting for Jira to make the fields searchable** | Nothing. It happens once, a few minutes after install, and only to `addedAfterSprintStart`, `removedAfterSprintStart`, `committedAtSprintStart` and `reestimatedInSprint`: their searches use the fields. The other functions work straight away. Sprint Pack replaces the error with the result once Jira can search the fields. |
| "Sprint Pack is indexing "Sprint 12" (4123). Run the search again in a few minutes." | **Waiting for sprints to be indexed** | Nothing: the sprint is being [computed](numbers.md#computing), and Sprint Pack replaces the error with the result when it's done. If it doesn't clear, an admin can rebuild the sprint. |
| "Sprint Pack can't read board 12's settings (its filter isn't shared with the app)…" | **Board settings Sprint Pack can't read** | Share the board's filter with its project, or with a group that includes the app. See [settings unreadable](numbers.md#settings-unreadable). |
| "No sprint named "…"", "No sprint with ID …", "No board named "…"", "No board with ID …" | **Searches that name something that doesn't exist** | Fix the argument. Whoever saved the search (a filter, board or gadget) needs to change it. |
| ""Sprint 12" matches 3 sprints: …" or ""…" matches 2 boards: …" | Same | Use the sprint ID it suggests, or add the board. |
| ""…" is a deleted sprint (ID …). Pass the ID to query it." | Same | Use the ID. |
| ""last 50" expands to 1200 sprints; add a board argument." or "The range matches … sprints" | Same | Add a board argument. |
| "… is not a date. Use yyyy-MM-dd or an offset such as -90d." | Same | Fix the date. |
| "duringSprint: the field must be one of …" or "statusChangedDuringSprint needs a status name…" | Same | Fix the first argument. |
| "… the count must be 1 or more." | Same | Use `"previous 1"` or more. |
| "Sprint Pack couldn't evaluate this yet (…). It will retry." | **Searches Sprint Pack couldn't evaluate** | The hourly sweep retries these. An admin can retry at once. |
| Anything else | **Other errors** | An admin can retry; if it comes back, the message says what went wrong. |
