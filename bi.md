---
title: BI tools
---

<!-- Status: written 2026-10-08 (clarity pass C1); the Power Query sections rewritten 2026-10-10 for the six-table snippet and its measures (release checklist 1.8). Moved from docs/help-bi.md (BI build, bi-design D1–D13, and BI gaps build B1–B8); the publishing statement's wording is final (D4 amendment 6). -->

<a id="bi"></a>
# Use Sprint Pack data in Power BI, Excel and other BI tools

**Sprint Pack can publish each board's sprint totals into a board property inside Jira. BI (Business Intelligence) tools and spreadsheets read it through Jira's REST API with credentials you already control. Nothing leaves Atlassian: the app has no server of its own and never sends your data anywhere.**

- Off by default. A Jira admin turns it on and chooses the boards.
- Totals only: no issue keys and no people.
- Read with a Power Query snippet (Excel, Power BI), a Python script, a Google Sheets script, or a warehouse load.

<a id="publishing"></a>
## What is published

This statement appears wherever you publish, word for word:

> Publishing writes a board's sprint totals to a property on that board. Anyone who can see the board can read it through Jira's REST API. Totals count only issues the app can see. Issues with a security level that excludes the app are left out, so published totals can be lower than Jira's Sprint Report. No issue keys or names are published. Unpublish before uninstalling.

**In the rollup property (`sprint-pack-rollup`),** per sprint: counts and estimates for committed, added, removed, completed, completed from the commitment, not completed, carried over, completed outside the sprint and re-estimated; the estimate change totals; [anomaly flags](anomalies.md); the sprint's name, dates and goal. Per board: its project, [labels](#labels-bi), unit and notices. Definitions: [how the numbers are counted](numbers.md).

**In the burndown property (`sprint-pack-burndown`),** for the active sprint and the last 12 closed sprints: one row per calendar day with `remaining`, `completed`, `added`, `removed`, `scope` and `ideal` in the board's unit, the same as issue counts, and `partial` on today's row of an active sprint. An active sprint's rows run to its planned end: the days after today carry the ideal line only, with the other values empty. Plus the [working-day settings](views.md#working-days) the ideal line used. About 1 KB per two-week sprint. See [burndown](views.md#burndown).

**Refreshed by the app:** after sprint changes, at most every 15 minutes for the active sprint. The board panel says when the BI copy was written and whether it's up to date, has a refresh queued, or will refresh when sprints finish [computing](numbers.md#computing).

**Who publishes:**

- **Jira admins:** Sprint Pack admin page → BI data. Turn it on for the site, then publish chosen boards, or all boards including ones created later. Turning it off unpublishes every board.
- **Board admins** can publish their own boards when a Jira admin allows it, from the "BI data for this board" panel on the Sprint scope page.

**Unpublish before uninstalling.** Board properties survive an uninstall. Use **Unpublish all** on the admin page first. On a site with many boards the deletion carries on in the background, and the page shows how many boards are left. Uninstall once it says **It's safe to uninstall**. The app also tries to delete every published property as it's uninstalled, but that step has about a minute, which may not be enough on a site with many boards. Copies already loaded into BI tools stay there.

<a id="who-can-read"></a>
## Who can read it

**Anyone who can see the board can read its published property through Jira's REST API.** The property can't be filtered per reader.

- A reader needs a credential that can see the board: a person's API token, or better, a [service account](#service-account) you can name and revoke.
- On a company-managed board the reader must also be allowed to see the board's filter.

<a id="whose-view"></a>
## Whose view the published totals count

**Published totals count the issues the app can see; the views inside Jira count what each viewer can see.**

| Surface | Counts | Label |
|---|---|---|
| Sprint scope views | Issues the viewer can see | "Counted as you see them" |
| Exports | Issues the person exporting can see | "As visible to you on <date>" |
| Published board property | Issues the app can see | `"countedAs": "app"` |
| Issue lists through search | Issues the reader can see | `countedAs: "reader"` |

So a viewer who can't see every issue sees lower numbers in Jira than in the published data. The view then shows the published totals beside theirs ("Published to BI: 18 committed"). See [whose view is counted](numbers.md#counted-as).

<a id="issue-security"></a>
## Issue security

On a site with issue security levels, the admin page says whether Sprint Pack can see secured issues:

- **It can't** (the usual case): those issues aren't counted anywhere in Sprint Pack, so the published totals and the views are lower than Jira's Sprint Report for people who can see them. The view says how many.
- **It can** (a level that includes the app): published totals count them, and anyone who can see the board reads totals that include issues they may not be allowed to see. If that's not acceptable for a board, don't publish it.

<a id="multi-project"></a>
## Boards that span several projects

A company-managed board's filter can cover several projects. **Published totals then include issues from all of them,** as the app sees them, and anyone who can see the board can read those totals. The publish panel warns you and names the projects before you publish.

Only the board's own location (the project it lives in) is published as `projectKey` and `projectName`. The other projects its filter covers aren't.

<a id="other-board-sprints"></a>
## Sprints created on another board

A board can show a sprint that was created on another board, for example when two boards share a filter. **Each sprint is published once, under the board it was created on** (its `boardId` is that origin board), with all its figures there. It isn't repeated under the other boards that show it. To get its figures, publish the board it was created on.

<a id="bi-license"></a>
## When the license lapses

**Published data is frozen, never deleted.** While Sprint Pack's license isn't active, it stops refreshing the published properties. They keep the data as of the last refresh, and each board's index gets a notice saying so, with the date. Readers show it in `Boards[notices]` (the `notices` column of the `boards` table). Publishing resumes on renewal, and the notice goes with the next refresh. See [the license](admin.md#license).

<a id="service-account"></a>
## Set up a service account

**A service account is a reader you can name and revoke.** It doesn't take a Jira seat, and each organisation gets 5 free. You need org admin access in **admin.atlassian.com**. Allow about 15 minutes, and do steps 3 and 4 in one sitting: Atlassian shows each secret once.

1. **Create the service account.** Directory → Service accounts → Create. Note its email address (it ends in `@serviceaccount.atlassian.com`). It's not a secret.
2. **Let it see the boards.** Give it Jira access, and add it to a group that can **browse the projects of the published boards**. On a company-managed board, the board's **filter must be shared** with that group or the project. The service account sees exactly the boards that group sees.
3. **Choose the scopes.** The credential screen lists hundreds of scopes with similar names. Search for and select exactly these three:

   ```
   read:board-scope:jira-software
   read:board-scope.admin:jira-software
   read:project:jira
   ```

   `read:board-scope.admin:jira-software` is the easy one to miss: without it the boards are listed but every read of the published data fails with "scope does not match". It's read-only, despite "admin" in its name. No `write` scope is needed.

   **Only if this reader will also read issue lists** (see [issue lists](#issue-lists)), add a fourth: `read:jira-work`. It lets the account read every issue it can browse, so leave it out for a reader that needs only the totals.
4. **Create the credential, and save everything before leaving the screen.**
   - **Scoped API token** (for Power Query in Excel or Power BI, the script and Google Sheets): save the token. It's used with the **service account's email** from step 1, not yours.
   - **OAuth 2.0 client credential** (for the script): save **both the client ID and the secret**. The client ID is shown only once too.
   - Put them straight into your password manager or where the reader will use them (Power BI's credential dialog, the OS keyring for the script). Not a text file or a chat.
5. **Test it before you hand it over,** with the [script](#script). Its `boards.csv` lists every scrum board the account can see, published or not; a missing board is one it can't see (step 2).
6. **Record the expiry.** A scoped API token expires (at most a year). Enter the date on the Sprint Pack admin page: from 30 days before, a notice with the date (nothing else) appears there and in the published data's `notices`. OAuth client credentials don't expire until revoked.

**If it doesn't work:**

| What the reader sees | Cause | Fix |
|---|---|---|
| 401 "Unauthorized" | Wrong email for the token (often the admin's own), or a revoked or mistyped token | Use the service account's `@serviceaccount.atlassian.com` email with its token |
| 401 "Unauthorized; scope does not match" | A scope is missing, most often `read:board-scope.admin:jira-software` | Create a new credential with the three scopes in step 3 |
| 401 on the site address (`your-site.atlassian.net`) | Scoped tokens work only through `api.atlassian.com/ex/jira/{cloudId}` | Use the "Scoped API token or service account" snippet |
| The script says "token request refused (403: client is disabled)" | The OAuth credential was revoked | Use another credential, or create a new one (steps 3–4) |
| A board is missing | The account can't see it: no browse permission, or a filter not shared with its group | Step 2 |
| A board shows `published = false` | The board isn't published | A Jira admin, or the board's admin if allowed, publishes it |

<a id="power-bi"></a>
## Excel and Power BI (Power Query)

**Open the board's "Use in Power BI / Excel" panel in the Sprint scope view (or "Show setup for BI tools" on the admin page), choose your kind of token, copy the Power Query (M) snippet into a blank query, and sign in with Basic authentication.**

| Kind of token | Address | Sign in with |
|---|---|---|
| **API token without scopes** | Your site, `https://your-site.atlassian.net` | Your Atlassian email and the token |
| **Scoped API token or service account** | `https://api.atlassian.com/ex/jira/<cloud ID>` | The account's email and the scoped token |

**One query, six tables.** The snippet gives the same tables and [columns](#columns) as the script and Google Sheets: `Boards`, `BoardGroups`, `Sprints`, `SayDo`, `EstimateDrift` and `Burndown`. Issue lists aren't included; see [issue lists](#issue-lists).

1. Paste the snippet into a blank query (Power BI: Get data → Blank query → Advanced editor; Excel: Data → Get Data → From Other Sources → Blank Query → Advanced Editor) and name the query **SprintPack**.
2. Don't load SprintPack itself. In Power BI, clear its **Enable load**. In Excel, use **Close & Load To → Only Create Connection**.
3. For each table you want, right-click **Table** next to its name → **Add as New Query**. Or make a blank query that says `= SprintPack[Sprints]`.

- **Typed columns:** numbers, dates and true/false come typed, so Power BI can sum and chart them straight away. The burndown's `date` is a date, for a date axis.
- **Which boards:** the admin page's snippet reads **every scrum board the credential can see** (`BoardIds = null`), so boards published later appear on the next refresh, and unpublished ones show in `Boards` with `published = false`. A board's own panel gives a snippet for that board (`BoardIds = {12}`). Change the line either way.
- **Calls:** each table reads only what it needs, but each table query reads the published data itself, and finding boards reads every board you can see. On a site with hundreds of boards, list the boards you need in `BoardIds` and load only the tables you use.
- **Consistent copies:** the snippet checks that it read a consistent copy of each board's data, and reads again if a write was in progress. `Boards[status]` says `torn` if it still wasn't.
- **Refresh:** Excel for Windows and Mac refreshes on open or by hand; Excel on the web refreshes too. In the Power BI service, scheduled refresh should work without a gateway (the snippet calls `Web.Contents` only with the fixed address, and puts each path in `RelativePath`). With a scoped token, turn on **Skip test connection** in the data source credentials: the bare `api.atlassian.com/ex/jira/<cloud ID>` address answers 401 even with a valid token.
- The cloud ID is shown on the admin page and in the board's panel.
- **Not in this version:** a Power BI custom connector. Use the snippet.

<a id="power-bi-measures"></a>
## Measures in Power BI

**Compute say/do and the other ratios as a ratio of sums, over closed sprints, leaving flagged sprints out and never mixing units.** That's how the views and the dbt package count. An average of the per-sprint `sayDo` column gives a two-point sprint the same weight as a forty-point one; use the column only in tables of single sprints.

Copy these measures into the `Sprints` table (Modeling → New measure):

```
Say/do =
VAR Counted = FILTER(Sprints, Sprints[state] = "closed" && ISBLANK(Sprints[anomalies]))
RETURN
    IF(
        DISTINCTCOUNT(Sprints[unit]) > 1,
        BLANK(),
        DIVIDE(SUMX(Counted, Sprints[points_completedFromCommitment]), SUMX(Counted, Sprints[points_committed]))
    )

Say/do (issues) =
VAR Counted = FILTER(Sprints, Sprints[state] = "closed" && ISBLANK(Sprints[anomalies]))
RETURN DIVIDE(SUMX(Counted, Sprints[issues_completedFromCommitment]), SUMX(Counted, Sprints[issues_committed]))
```

The others follow the same pattern as `Say/do`, with the same unit check:

| Measure | Numerator (`SUMX(Counted, …)`) | Denominator (`SUMX(Counted, …)`) |
|---|---|---|
| Velocity ratio | `Sprints[points_completed]` | `Sprints[points_committed]` |
| Scope change | `Sprints[points_added] + Sprints[points_removed]` | `Sprints[points_committed]` |
| Committed, completed (totals) | `Sprints[points_committed]`, `Sprints[points_completed]` | none |

- **Units:** `points_…` columns are in each board's unit: story points, seconds for Original estimate, or empty on an issue-count board (see [units](numbers.md#units)). The `DISTINCTCOUNT(Sprints[unit]) > 1` check blanks a points measure when a visual spans boards with different units, such as a label covering a story-point board and an issue-count board. Use `Say/do (issues)` there, which works across units.
- **Flagged sprints** stay in the tables with their `anomalies` codes; the measures leave them out, like the views. See [why flagged sprints are left out](anomalies.md#left-out).
- **Filtering by label:** relate `Boards[boardId]` to `boardId` in each other table (one to many), with `Boards`–`BoardGroups` filtering both ways, then filter on `BoardGroups[label]`.

<a id="get-the-files"></a>
## Get the files

**The script (`sprint_pack_bi.py`), the dbt package and the Fivetran connector are downloads, not part of the Jira app.**

<!-- TODO: where the files are hosted isn't decided yet (release-checklist 4.9, which blocks this section). Add the links and how to check you have the current version. -->

The setup panel and "Send to your BI team" generate everything you paste (the Power Query snippet, the Google Sheets script and the command lines), so those routes need no download.

<a id="script"></a>
## The script

**`sprint_pack_bi.py` (Python 3, standard library only) writes the published tables as CSV and JSON,** for Fabric notebooks, dbt, warehouses and scheduled jobs. The secret comes from an environment variable or the OS keyring, never the command line:

```
JIRA_API_TOKEN=… python3 sprint_pack_bi.py --site https://your-site.atlassian.net --email you@example.com --out ./sprint-pack
JIRA_API_TOKEN=… python3 sprint_pack_bi.py --cloud-id <cloud id> --email <name>@serviceaccount.atlassian.com --out ./sprint-pack
SPRINT_PACK_CLIENT_SECRET=… python3 sprint_pack_bi.py --cloud-id <cloud id> --client-id <client id> --out ./sprint-pack
```

To keep the secret off the screen and out of shell history, store it in the OS keyring (`pip install keyring`) while it's still shown in admin.atlassian.com. This asks for the secret first, then the client ID or email it belongs to:

```
python3 -c "import getpass, keyring; s=getpass.getpass('Secret or token: '); u=input('Client ID or service account email: ').strip(); keyring.set_password('sprint-pack', u, s); print('stored for', u)"
```

- **Tables:** `boards`, `boardgroups`, `sprints`, `saydo`, `estimatedrift`, `burndown`, and `issues` with `--issues`. See [columns](#columns).
- **`--issues [N]`** also writes the issue lists for the last N closed sprints per board (default 6) plus the active one. Needs `read:jira-work`.
- **`--fields`** prints your site's IDs of Sprint Pack's fields. Needs `read:jira-work`.
- **Which boards:** without `--boards`, every scrum board the credential can see, so boards published later are picked up on the next run. The admin page's command has no `--boards`; a board's own panel adds `--boards` with that board.
- **Every run is a full refresh** and writes `schema.json` (every table's columns and types) next to the files.

<a id="sheets"></a>
## Google Sheets

**The setup panel generates an Apps Script for your kind of token. It fills one tab per table and refreshes on a timer.**

1. In your sheet: Extensions → Apps Script. Paste the script and save.
2. Run `setToken()` once. It asks for the email and the token and stores them in **your user properties**, which nobody else who edits the script or the sheet can read.
3. Run `refreshAll()` once and authorise it.
4. Run `installDailyTrigger()` (or the hourly one).

- Each run replaces every tab in full: `Boards`, `BoardGroups`, `Sprints`, `SayDo`, `EstimateDrift`, `Burndown`, and `Issues` when generated with issue lists (needs `read:jira-work`). It also writes an **About** tab with links to these definitions (see [links in spreadsheets](exports.md#links-in-spreadsheets)).
- **Which boards:** the admin page's script reads every scrum board the credential can see (`BOARD_IDS = []`), so boards published later appear on the next run. A board's own panel gives a script for that board.
- For a sheet a team shares, use the service account's token.
- **Text that starts like a formula** (`=`, `+`, `-`, `@`), such as a sprint goal, is written as text, as in the [exports](exports.md), so it never runs in your sheet.
- **Know this:** anyone with edit access to an Apps Script can change what it does with the data it fetches. And the data goes from Jira into your Google account under your own credential, like any export you make: "no egress" describes the app, not what you do with your credentials.
- A revoked token shows as "401 from Jira: the token was revoked or lacks a scope" on the next run.

<a id="looker"></a>
## Looker Studio

**Looker Studio reads Google Sheets, so it works on top of the Sheets route.** Add the sheet the [Google Sheets script](#sheets) fills as a data source, one per tab, and build your report from those tables. Compute ratios such as say/do per board first, then compare boards with the median, as the [unit rule](numbers.md#units) explains.

<a id="tableau"></a>
## Tableau and Qlik

**Tableau** (including the free Tableau Public) connects to Google Sheets and to CSV files, so use the Sheets route for scheduled refresh, the script's CSVs for a one-off, or the [warehouse](#warehouse) if IT already runs one. The column definitions are the same as everywhere: see [columns](#columns).

**Qlik:** a page for its REST connector will follow if customers ask for it.

<a id="warehouse"></a>
## A warehouse: the script on a schedule, dbt and Fivetran

**Full refresh, every time.** Past sprints can change after they close (a rules update, a board's estimation field changing, a reopened sprint), so an incremental load would miss changes. The data is small; each load replaces the tables. Every row carries `ruleVersion`, `generatedAt`, `generation` and `loadedAt`, so a load can be traced, and `schema.json` gives typed columns.

**Where the script runs:**

- **A scheduled job** (GitHub Actions, cron): the service account's scoped token as a secret, then `python3 sprint_pack_bi.py --cloud-id <cloud id> --email <service account email> --out ./sprint-pack`, followed by your loader (`COPY`, `bq load`, `snowsql PUT` …) with full replace. In GitHub Actions: `env: JIRA_API_TOKEN: ${{ secrets.SPRINT_PACK_TOKEN }}` and a daily `schedule: cron`.
- **A Fabric notebook:** the script in a notebook cell with the token from a Key Vault secret, writing the CSVs to a lakehouse. The Power BI service reads the lakehouse without a gateway.
- **Fivetran:** the Sprint Pack connector (Connector SDK) runs the script's reader inside Fivetran and upserts the same tables. Test it locally with `fivetran debug --configuration configuration.json` (no account needed; it writes a DuckDB file), then deploy it with your account. Rows that are no longer published (a sprint that left the rollup, an unpublished board, a label taken off a board) are deleted on the next sync. Fivetran marks them `_fivetran_deleted = true` rather than removing them, so filter on that column or read only the latest load. Burndown days and issue lists of sprints that left the published window stay, with an older `loadedAt`. The connector's column names arrive in snake_case (`board_id`, `loaded_at`).

**The dbt package** puts the metric definitions in one place, the same as the views:

| Measure | Definition | Models |
|---|---|---|
| `say_do` | completed from the commitment ÷ committed | `sprint_metrics`, `board_metrics` (ratio of sums over the last N closed sprints), `label_metrics` (median and mean across boards) |
| `velocity_ratio` | completed ÷ committed | same |
| `scope_change` | (added + removed) ÷ committed | same |
| `reestimate_rate` | issues re-estimated during the sprint ÷ (committed + added) | same |
| `carry_over_issues` | carried over ÷ (committed + added) | same |
| `burndown_daily` | the published burndown, typed | one row per sprint and day |

Point columns are never added across boards with different units: `label_metrics` gives point totals only when a label's boards share a unit, and `board_metrics` gives unit `mixed` and no point totals when a board's sprints use different units. Every mart reads only the latest load (the highest `loadedAt`), so rows an upserting loader keeps for sprints no longer published don't count. IDs are text in the staging models. A test checks that `sprint_metrics.say_do` equals the script's `saydo` table.

**Join keys with Jira copies in your warehouse.** `sprintId` is Jira's sprint ID and `boardId` is the sprint's *origin* board. In Fivetran's Jira connector, `sprintId` ↔ `sprint.id` and `boardId` ↔ `sprint.board_id`. In Airbyte's Jira source, the `sprints` stream carries `boardId` and `boards` carries `id`. Neither copies board properties, so Sprint Pack data gets there only through the script or the Fivetran connector. Check the column names against each vendor's current schema before relying on them.

**Join-key types.** `boardId` and `sprintId` are text in every Sprint Pack table, as Jira's REST API returns IDs. A Jira copy may store them as numbers. Cast one side so the types match, for example on BigQuery with Fivetran's Jira connector: `on CAST(sp.sprint_id AS INT64) = sprint.id`, or `on sp.sprint_id = CAST(sprint.id AS STRING)`. Whether they copy Forge app fields on issues is unverified; the `issues` table doesn't depend on it.

<a id="requests"></a>
## Requests for BI data

**Everyone sees "Use in Power BI / Excel" on the Sprint scope view.** On a published board it opens the setup. On an unpublished board it says what publishing gives and who can turn it on, and offers **Request BI data for this board**.

- The request stores your account and the date, nothing else.
- Jira admins see requests on the admin page ("2 requests, latest 8 Oct", with Publish and Dismiss); the board's admins see them in the board panel. You see "Requested on 8 Oct".
- Requests are kept once per person and board, removed when the board is published or the request dismissed, and expire after 90 days.
- There's also text to copy into chat or email. The app sends nothing itself.

<a id="handoff"></a>
## Send to your BI team

**"Send to your BI team"** (admin page for all boards, or the board panel for one) builds a Markdown handoff, shown on screen and downloaded. It contains:

- the site and cloud ID; every board with its project, labels, state, last write and health;
- what the app can check: the site switch, which boards are published, and whether it can read each board's filter (if it can't, the service account probably can't either, so the handoff names the board and says to ask the filter's owner to share it);
- what the app can't check (the service account, its group and its token), as unchecked steps for the org admin, with the symptom of each missed step;
- both Power Query snippets, the script command and the routes in this version;
- a link to these definitions.

It never contains a credential. Before it's produced, the app checks the generated snippets and scripts for anything that looks like a token, and the whole document for an Atlassian API token. If a board's name or one of its labels looks like one, the handoff isn't produced and the message names the board: rename it or remove the label.

<a id="labels-bi"></a>
## Labels and location in BI data

Every published board carries:

- **Its location:** `projectKey` and `projectName`, the project the board lives in (neither for a board in a user's profile). See [boards that span several projects](#multi-project).
- **Its labels:** free text set in Sprint Pack, several per board. See [labels](views.md#labels) for who sets them and the rules.

**Labels are current, not historical.** Relabelling a board changes the labels on every past sprint of that board in BI data. `groupsChangedAt` says when the labels last changed. Labels are kept whether or not a board is published.

| Reader | Location | Labels |
|---|---|---|
| Script, Sheets and the Power Query snippet (`boards`, `boardgroups`, and every `sprints`/`saydo` row) | `projectKey`, `projectName` | `groups` joined with `; `, and `boardgroups` with one row per board and label |
| The property itself | `projectKey`, `projectName` | `groups` (a list), `groupsChangedAt` |

For one row per label, use `boardgroups` (`BoardGroups` in Power Query).

<a id="columns"></a>
## Columns

**One column contract (`columns.json`) defines every table and column that the script, the Sheets script, the dbt package and the Fivetran connector produce. Each column's meaning is defined once, in [how the numbers are counted](numbers.md), and every column in the contract links to its definition there.**

| Table | One row per | Key columns |
|---|---|---|
| `boards` | board | `published`, `status`, `complete`, `pendingSprints`, `countedAs` (always `app`), `unit`, `notices`, `groups` |
| `boardgroups` | board and label | `label` |
| `sprints` | sprint | `issues_<bucket>` and `points_<bucket>` for every [bucket](numbers.md#buckets), `approximate`, `anomalies`, `countedAs`, `dataAsOf` |
| `saydo` | closed or active sprint | `sayDo`, `velocityRatio`, `scopeChange` (estimates), `sayDoIssues` (issue counts) |
| `estimatedrift` | sprint | `reestimated`, `refinedBeforeStart`, `unestimatedAtStart`, `estimateRemoved`, `driftNet`, `driftAbsolute`, `timeLoggedVsOriginal` |
| `burndown` | sprint and day | `remaining`, `completed`, `added`, `removed`, `scope`, `ideal`, the same as issue counts, `partial` |
| `issues` | issue and bucket | `bucket`, `issueKey`, `countedAs` (always `reader`) |

- **Units:** `points_…` columns are in the board's unit: points, seconds for Original estimate (the views show hours), or empty on an issue-count board. See [units](numbers.md#units).
- **Every row** carries `ruleVersion`, `generatedAt`, `generation` and `loadedAt`.
- **Stable:** columns are only ever added while the schema version stays 1. Readers that pick columns by name keep working.
- **`anomalies`** holds the [anomaly codes](anomalies.md) joined with `; `. Flagged sprints are in the data; leave them out of averages as the views do (see [left out](anomalies.md#left-out)).
- The published index also carries `helpUrl`, the address of this help.

<a id="issue-lists"></a>
### Issue lists

The published property holds totals only. Issue lists come from Jira's own search, under the reader's permissions, so a reader who can't see every issue gets shorter lists than the totals. Both routes need `read:jira-work`.

- **Recommended: the JQL functions, one search per sprint and bucket:** `issue in committedAtSprintStart(10)`, `addedAfterSprintStart(10)`, `removedAfterSprintStart(10)`, `completedInSprint(10)`, `incompleteInSprint(10)`, `carriedOverFromSprint(10)`. Always works and matches the views. The script's `--issues` does this. See [the functions](jql.md).
- **One search per board on a field,** such as `"Added after sprint start" is not EMPTY AND project = X`, returns every issue with its sprint IDs in fewer calls. In a team-managed project it works only once the fields are added to the space (step 1 in [fields not showing](admin.md#fields-not-showing)); before that the field is left out of the response with no error, so you'd see blanks. Company-managed projects return the field whether or not it's on a screen.

**Field IDs differ per site.** Look them up through `GET /rest/api/3/field`, matching `schema.custom` ending in `/static/<key>` (`added-after-start`, `removed-after-start`, `times-carried-over`, `reestimated-in-sprint`, `sprint-start`, `sprint-end`, `last-added-after-start`, `last-removed-after-start`), or run `sprint_pack_bi.py --fields`.

**Power Query example:** the issues added after the start of each sprint in the `Sprints` table.

```
let
    Site = "https://your-site.atlassian.net",
    Sprints = <the Sprints table from the snippet>,
    Search = (jql as text) =>
        let
            Body = Json.FromValue([jql = jql, fields = {"key"}, maxResults = 1000]),
            Response = Web.Contents(Site, [RelativePath = "rest/api/3/search/jql", Content = Body, Headers = [#"Content-Type" = "application/json", Accept = "application/json"]]),
            Issues = Json.Document(Response)[issues]
        in
            List.Transform(Issues, each _[key]),
    Added = Table.AddColumn(Sprints, "addedIssues", each Search("issue in addedAfterSprintStart(" & [sprintId] & ")")),
    Rows = Table.ExpandListColumn(Added, "addedIssues")
in
    Rows
```

More than 1,000 issues continue with `nextPageToken`; the script handles that, this example doesn't.
