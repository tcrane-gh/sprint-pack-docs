---
title: Request kit
---

<!-- Status: drafted 2026-10-13 for checklist 1.21 and docs/marketing-plan.md §5.3. The prices follow design.md "Pricing"; check them against the listing before each publish. -->

<a id="request-kit"></a>
# Request kit: Sprint Pack for your Jira site

For whoever approves apps on a Jira Cloud site: the Jira admin, security reviewer or budget holder. Someone on your site wants Sprint Pack. This page covers what it does, what it costs, what it can touch and what changes in Jira.

## What it does

Sprint Pack shows what happened to each sprint's scope: what was committed at the start, what was taken on and removed after it (and by whom), what was completed, and what was carried over. The numbers match Jira's own Sprint Report, and every number opens as a Jira search. Managers use it to explain a sprint's result to the people they report to, on a dashboard or in slides.

## What it costs

Sprint Pack is sold on the Atlassian Marketplace and billed by Atlassian with your Jira subscription. Like every Marketplace app, it's licensed for every user on the Jira site, not only the people who use it. Atlassian sets that rule; it isn't ours.

| Jira users on the site | Monthly | Annual |
|---|---|---|
| Up to 10 | Free | Free |
| 100 | $100 | $1,000 |
| 250 | $205 | $2,050 |
| 1,000 | $505 | $5,050 |
| 2,500 | $955 | $9,550 |
| 5,000 | $1,555 | $15,550 |

Each band of users has its own rate: $1.00 a user a month for the first 100, $0.70 for users 101–250, $0.40 for 251–1,000, $0.30 for 1,001–2,500 and $0.24 for 2,501–5,000. The [Marketplace listing](https://marketplace.atlassian.com/) has the current prices. The trial is free for 30 days, and Atlassian refunds in full within 30 days of purchase.

## Security and data

- **Runs on Atlassian.** Sprint Pack runs on Atlassian's Forge platform. It has no server of its own and makes no outbound network calls, so your data doesn't leave Atlassian. It keeps its data in Atlassian's storage for your site, which follows your site's data residency.
- **Read-only on issues.** It never edits issues. It writes only its own 8 read-only fields, its stored search results and, if an admin turns on BI publishing, its own board properties.
- **Your permissions apply.** Views and files count only the issues the viewer can see, as Jira's Sprint Report does.
- **No credentials.** It doesn't ask for or store passwords or API tokens.
- **Personal data.** It stores Atlassian account IDs (for example, who added an issue to a sprint) and caches the matching display names for a day. It doesn't store issue summaries or descriptions. People can write a short reason for a scope change; that text is stored in Atlassian's Forge storage with its author's account ID.
- The full answers are in the [security page](security.md), the [privacy policy](privacy.md) and the Privacy and Security tab of the Marketplace listing.

## What changes in your Jira

- **8 read-only fields on issues,** such as Sprint Start and "Added after sprint start". They show only where an admin adds them to screens or layouts. See [fields](jql.md#fields).
- **12 JQL functions** that work in search, filters, boards, quick filters and automation. See [JQL functions](jql.md#jql).
- **A Sprint scope page** in each Scrum project, a **Scope changes** item in each sprint's menu, a **Sprint portfolio** page across boards, and a **dashboard gadget and widget**. See [views](views.md#views).
- **An admin page** under Jira settings → Apps. See [admin page](admin.md#admin).
- Nothing changes in your workflows, boards or issues.

## Who supports it

Sprint Pack is built and supported by one developer at Weave Commons. Before Weave Commons, its developer spent seven years at Atlassian, including two building role-based access control for Confluence and five on Growth: onboarding and activation for Jira and Confluence, and guest access for external collaborators. Email [support@sprintpack.weavecommons.com](mailto:support@sprintpack.weavecommons.com) for help; we reply within 2 business days. Security reports go to security@sprintpack.weavecommons.com, answered within 3 business days. See [support](support.md).

If Sprint Pack ever stopped being supported, nothing in your Jira would be lost. The app reads sprint history from Jira's own records and never edits issues. Its fields and pages go away when it's uninstalled, and **Unpublish all** on the admin page removes the board properties it published (it also does this itself on uninstall).
