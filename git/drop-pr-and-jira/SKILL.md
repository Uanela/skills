---
name: drop-pr-and-jira
description: Opens a merge request (or reuses the existing one), comments MR: <url> on the Jira ticket, and moves it to QA Pending. User-invoked via /drop-pr-and-jira.
disable-model-invocation: true
---

# drop-pr-and-jira

Attach the branch MR/PR to Jira and park the ticket in QA.

## 1. MR/PR

Read and execute [../drop-pr/SKILL.md](../drop-pr/SKILL.md) (including `pr-body.md`).

If an open MR/PR already exists: **keep it.** Use that URL. Do not create another.

Do not continue until you have an MR/PR URL.

## 2. Ticket

Resolve the Jira key, in order:

1. Key the user gave
2. Branch name (`ABC-123`)
3. This conversation
4. Ask — do not guess a project

## 3. Jira (Atlassian MCP)

Discover tools first (`GetDynamicTools` on the Atlassian namespace), then:

1. `getAccessibleAtlassianResources` → `cloudId` (ask if more than one site fits)
2. `getJiraIssue` — confirm it is the right ticket
3. `addOrEditJiraIssueComment` — comment body **exactly** `MR: <url>` using the **existing** MR/PR URL (nothing else). If that comment is already on the ticket, leave it.
4. `transitionJiraIssue` — `transitionName` **QA Pending**, or the closest real transition (`Ready for QA`, `Pendente QA`, `In QA`, `QA`). If none exist, say so and leave status unchanged.

If the MCP is missing or `needsAuth`, stop and tell the user to run `agent mcp login atlassian`. Do not invent a REST fallback unless they ask.

## Done

Reply with:

- MR/PR URL (and that it already existed, if so)
- ticket key
- new status (or that no QA-like transition existed)

No extra Jira commentary. No diff recap.
