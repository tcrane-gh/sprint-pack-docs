---
title: Privacy policy
---

<!-- Status: drafted 2026-10-12 for checklist 4.4. -->

# Privacy policy

Effective 15 October 2026. This policy covers Sprint Pack, an app for Jira Cloud sold on the Atlassian Marketplace by Weave Commons ("we").

## Where your data lives

Sprint Pack runs on Atlassian's Forge platform and has no server of its own. Everything it reads and stores stays in Atlassian's cloud, with your Jira site. We don't receive a copy of it.

## What the app stores

In Atlassian's storage for your site:

- **Sprint history:** for each sprint, its dates, state and goal, and which issues joined, left, were completed or carried over, with their estimates, statuses and issue types. Issues are stored by ID and key; their summaries and descriptions are not stored.
- **Board settings:** each board's name, columns, estimation field and filter, as Jira gives them. Jira writes people in a filter as account IDs (for example `assignee = 712020:…`), so a board filter that names people holds their account IDs; so does the copy of the filter kept with each closed sprint.
- **Who made a change:** the Atlassian account ID of the person who moved an issue in or out of a sprint, changed an estimate or logged time during one, wrote a reason for a scope change, asked for a board's BI data, published BI data or changed its settings, or dismissed an inferred reopen.
- **Reasons for scope changes:** the text people write to explain why an issue was taken on or removed after a sprint's start (at most 500 characters each), with the author's account ID and the time. Anyone who can see the board can read, change or clear them. A reason is deleted when the issue's data for that sprint is: when the issue is deleted or no longer belongs to the sprint, or at the latest by the next weekly tidy. All of them go with the app's storage when the app is uninstalled.
- **For the "from outside the team" guess:** the reporter (as an account ID) of each issue taken on after a sprint's start, and the keys of issues linked to it; corrections with the account ID of who made them, deleted like reasons; and, only while a board has the reporter signal on, the account IDs of the people in its projects' roles, kept for up to six hours and deleted within a week after that.
- **Display names:** the names of the people above, cached so the views, exports and the admin page can show them, read again from Jira after a day, and deleted within a week of that. A name Jira won't give is shown blank, never as an account ID.
- **Per-person caches and choices:** which boards and issues a viewer can see, used for up to an hour so pages load faster and deleted within a week after that; and the portfolio selection each person last chose, kept until they choose again. Both are keyed by account ID.
- **Settings and status:** the app's settings, its point budget and the state of its background work.

In Jira itself: the app's read-only fields on issues, its stored JQL search results, and, if an admin publishes BI data, sprint totals saved as board properties.

The app doesn't collect email addresses, passwords or API tokens.

## Logs

Atlassian's platform keeps the app's logs. They hold issue, sprint and board IDs, search text and error messages. Account IDs are replaced with `<account>` before anything is logged. We read them only to support and fix the app.

## What leaves Atlassian

Nothing, by the app. Files you export are built in your browser and saved by you; they include the names of the people who changed issues, issue assignees, the reasons people wrote with their authors' names, and the name of the person who made the file. A copied summary names the person it was counted for. BI data you pull into another tool with your own credentials is your export.

## What we receive

- From the Atlassian Marketplace: the licence and contact details Atlassian shares with app vendors, such as your site and your billing and technical contacts. We use them for licensing, billing questions and support.
- From you: whatever you send us when you ask for support.

We don't sell personal data and don't use it for advertising.

## Services we use

Support and security email is handled by Fastmail (Fastmail Pty Ltd, Australia), which stores it on servers in the United States. Fastmail processes it only to deliver and store our mail.

## How long data is kept

While the app is installed. When it's uninstalled, the app deletes the BI data it published; on a site with many published boards, use "Unpublish all" on the admin page first, as uninstalling allows the app only a minute. Atlassian keeps the app's storage for 28 days before deleting it. The app's fields on issues and its stored search results stay in Jira; they hold sprint IDs, names and labels, no personal data. Support emails are kept as long as needed to answer them, and at most 2 years.

<a id="closed-accounts"></a>Closed Atlassian accounts: once a week the app reports the account IDs it stores to Atlassian, as Atlassian requires of every app, and erases the ones Atlassian says are closed, usually within a week of the closure. A reason written by a closed account keeps its text and loses its author. A board filter that names a closed account is read from Jira again at once. If the board's filter in Jira still names the account, the app keeps the filter as Jira has it, because the board's numbers depend on it, and doesn't report the account again; the admin page lists those boards so an admin can edit the filter in Jira. The app keeps a one-way hash of each closed account, so the list doesn't hold the account ID itself and the app doesn't store the account again when it reads it from Jira's history. Reporting needs a permission added in a later version of the app: until your admin approves that update, the app keeps erasing old caches but can't hear about closures.

## Your rights

For data in your Jira site, contact your site admin, who can uninstall the app. For anything we hold, email support@sprintpack.weavecommons.com.

## Changes

We'll post changes here with a new effective date.
