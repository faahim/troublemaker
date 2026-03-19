---
name: troublemaker
description: "Investigate bugs, plan features, create verified GitHub issues, merge PRs with full review checks, and clean up branches. Use when: (1) a bug is reported or discovered, (2) a feature or change is requested, (3) any implementation work needs planning and issue creation, (4) a PR needs to be reviewed and merged, (5) branches need cleanup after merge. Handles the full workflow: investigate codebase → verify findings → create detailed issue → assign → notify team → merge PRs → delete stale branches. Every claim is code-verified, never assumed."
---

# Troublemaker

Investigate, verify, plan, and file GitHub issues. Merge PRs safely. Clean up after yourself.

## Core Principle

Every claim in an issue must be verified against actual code. Wrong information in an issue leads to wrong implementation — the implementer trusts what you write, so earn that trust by reading the code, not guessing. You are the planner, not the implementer — gather enough verified context for whoever picks it up.

## Part 1: Issue Creation

### Step 1: Investigate

1. **Understand the problem** — what's broken or what's needed
2. **Trace the code path** — find relevant files, data flow, API calls
3. **Verify every finding** — grep, read actual code, check types, confirm behavior
4. **Stop at the right depth** — enough context for an implementer, not a full fix

Before including any claim, verify it:
- Read the actual code (don't rely on memory — code changes)
- Confirm functions, variables, types exist exactly as described
- Verify file paths and line numbers are current
- Check for related code that might contradict your finding
- If claiming "X doesn't exist" — actually search for it first

### Step 2: Plan

Structure findings into an actionable issue:

- **Problem** — clear description of what's wrong or what's needed
- **Root cause / Analysis** — verified findings with file paths and line references
- **Impact** — what's affected and why it matters
- **Fix approach** — high-level direction (not full implementation)
- **Files involved** — table of relevant files and their roles
- **Fix checklist** — concrete actionable items

### Step 3: Create Issue

Use `gh issue create` with a clear, specific title and the full body from Step 2. Check available labels first (`gh label list`) — using nonexistent labels breaks the command.

### Step 4: GitHub Project Board

The project board is how the team tracks work — an issue without board fields is invisible to the workflow.

1. **Add issue to project** — get issue node ID, add to project, get item ID
2. **Status** → `Ready` (default for new actionable issues)
3. **Priority** → P0 (critical/blocking), P1 (urgent/important), P2 (normal). If unclear, ask.
4. **Size** → estimate: XS (< 1h), S (1-3h), M (3-8h), L (1-3d), XL (3d+)
5. **Iteration** → current iteration (compare today's date against iteration start dates)

See `references/github-project.md` for field IDs, option IDs, and GraphQL mutations.

### Step 5: Assign

- If the operator specified who → assign directly via `gh issue edit <number> --add-assignee <username>`
- If not specified → ask: "কাকে assign করবো? নাকি backlog এ রাখবো?"

### Step 6: Create ERP Task

Sync to the ERP task board via MCP (`dekhval-erp-mcp` skill) so work is visible in both systems:

1. Create a task via `tasks.task.create` — title matching the GitHub issue, description with issue link + brief summary
2. Set priority (URGENT/HIGH/MEDIUM/LOW) and status (TODO if assigned, BACKLOG if not)
3. Assign to the same person as on GitHub (find their ERP user)
4. Pick the correct ERP project

### Step 7: Notify

If assigned to a team member, send a short WhatsApp message — issue title + link + one-line context. Use their language preference.

---

## Part 2: PR Merge

Merging is the last gate before code hits production. Rushing past unresolved feedback creates tech debt and erodes the review process.

### Step 1: Review Check

1. List reviews and CI status: `gh pr view <number> --comments`, `gh pr checks <number>`
2. Check both AI bot reviews and human reviews for unresolved feedback
3. For each unresolved comment, check:
   - Addressed in a subsequent commit?
   - Reply clarifying why it doesn't need fixing?
   - If neither → it's unresolved

### Step 2: Block or Proceed

**Unresolved items exist:**
- Message the PR author (WhatsApp for team, GitHub comment for external)
- List the specific unresolved items
- Do not merge until resolved or clarified

**Everything clear:** proceed to merge.

### Step 3: Merge

```bash
gh pr merge <number> --squash --delete-branch
```

The `--delete-branch` flag removes the feature branch after merge. Stale branches accumulate fast and make the repo messy — cleaning up immediately prevents that. If for some reason you need to preserve the branch, omit the flag, but the default is always delete.

If `--delete-branch` fails (e.g., branch protection), delete manually:
```bash
gh api -X DELETE repos/{owner}/{repo}/git/refs/heads/{branch-name}
```

### Step 4: Summary & Notify

Create a brief summary: what changed, key files affected, any breaking changes or things to watch.

Send to:
- **The person who requested the merge** (always)
- **Faahim (+8801753303880)** (always — even if he requested it, he still gets the summary)

### Step 5: Update ERP Task

1. Find the related ERP task (match by issue title or link)
2. Update status to DONE via MCP
3. If no matching task found, note it but don't fail

---

## Rules

- **Verify before claiming** — read the code, don't assume from memory. If you can't verify, say so explicitly ("needs confirmation: ...")
- **One issue per distinct problem** — unless problems are tightly coupled
- **Always delete merged branches** — keeps the repo clean, `--delete-branch` is the default
- **Don't go deeper than needed** — you're planning, not implementing
