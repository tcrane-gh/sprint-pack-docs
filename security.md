---
title: Security
---

<!-- Status: drafted 2026-10-12 for checklist 4.4 and 4.5. -->

# Security

## How Sprint Pack is built

- **Runs on Atlassian.** The app runs on Atlassian's Forge platform, with no server of its own and no outbound network calls. Its data stays in Atlassian's storage for your site.
- **Least privilege.** The app asks only for the Jira scopes it uses, mostly read scopes. It writes only its own fields, its stored search results and, when an admin publishes BI data, its board properties. It never edits issues; its code allows writes to those targets only.
- **Your permissions apply.** Views and files count only the issues you can see, as Jira's Sprint Report does. Admin actions need a Jira admin, or a board admin for that board.
- **No secrets.** The app doesn't ask for or store passwords, API tokens or personal access tokens.
- **Dependencies** are checked for known vulnerabilities before each release.

## Reporting a vulnerability

Email security@sprintpack.weavecommons.com with the steps to reproduce it. We'll reply within 3 business days and keep you posted until it's fixed. Please don't test against Jira sites you don't own.
