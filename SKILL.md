---
name: troublemaker
description: "Investigate bugs, plan features, and create verified GitHub issues with full implementation context. Use when: (1) a bug is reported or discovered, (2) a feature or change is requested, (3) any implementation work needs planning and issue creation. Handles the full workflow: investigate codebase → verify findings → create detailed issue → assign → notify team. Never includes assumptions — every claim is code-verified."
---

# Troublemaker

Investigate, verify, plan, and file GitHub issues for bugs and features. Then assign and notify.

## Core Principle

**Zero assumptions.** Every claim in an issue must be verified against actual code. Wrong information in an issue leads to wrong implementation. You are not the implementer — gather enough verified context for whoever will implement it.

## Workflow

### Step 1: Investigate

When a bug or feature is reported:

1. **Understand the problem** — what's broken or what's needed
2. **Trace the code path** — find relevant files, data flow, API calls
3. **Verify every finding** — grep, read actual code, check types, confirm behavior
4. **Stop at the right depth** — enough context for an implementer, not a full fix

Verification checklist before including any claim:
- [ ] Did I read the actual code, not assume from memory?
- [ ] Did I confirm the function/variable/type exists exactly as I describe?
- [ ] Did I verify file paths and line numbers?
- [ ] Did I check for related code that might contradict my finding?
- [ ] If I mention "X doesn't exist" — did I actually search for it?

### Step 2: Plan

Structure findings into an actionable issue:

- **Problem** — clear description of what's wrong or what's needed
- **Root cause / Analysis** — verified findings with file paths and line references
- **Impact** — what's affected and why it matters
- **Fix approach** — high-level direction (not full implementation)
- **Files involved** — table of relevant files and their roles
- **Fix checklist** — concrete actionable items

### Step 3: Create Issue

Use `gh issue create` with:
- Clear, specific title
- Appropriate labels (check available labels first: `gh label list`)
- Full body from Step 2

### Step 4: Assign

- If the operator already specified who → assign directly
- If not specified → ask: "কাকে assign করবো? নাকি backlog এ রাখবো?"
- Assign via `gh issue edit <number> --add-assignee <username>`

### Step 5: Create ERP Task

After the GitHub issue is created, sync it to the ERP task board via MCP (`dekhval-erp-mcp` skill):

1. Read the `dekhval-erp-mcp` skill for connection details
2. Create a task card using `tasks.task.create` with:
   - Title matching the GitHub issue title
   - Description including the GitHub issue link + brief summary
   - Link the GitHub issue URL in the task
   - Set appropriate priority (URGENT/HIGH/MEDIUM/LOW based on severity)
   - Set status to TODO (or BACKLOG if unassigned)
3. If someone is assigned on GitHub, find their ERP user and assign the task to them
4. Pick the correct project (match to the relevant ERP project)

### Step 6: Notify

If assigned to a team member, message them on WhatsApp:
- Keep it short — issue title + link + one-line context
- Use their language preference
- Use `message` tool with action `send`

---

## Part 2: PR Merge

### Step 1: Review Check

Before merging any PR:

1. List all reviews and review comments: `gh pr view <number> --comments`, `gh pr checks <number>`
2. Check AI bot reviews (automated code review bots) — look for unresolved feedback
3. Check human reviews — any "changes requested" still open?

For each unresolved review comment:
- Is it addressed in a subsequent commit?
- Is there a reply clarifying why it doesn't need to be resolved?
- If neither → **do not merge**

### Step 2: Block or Proceed

**If unresolved items exist:**
- Message the PR author (WhatsApp if team member, GitHub comment if external)
- List the specific unresolved items
- Ask them to either fix or clarify with a comment
- Do NOT merge until resolved/clarified

**If everything is resolved/clarified:**
- Proceed to merge

### Step 3: Merge

Merge the PR: `gh pr merge <number> --squash` (or merge strategy as appropriate)

### Step 4: Summary & Notify

After successful merge, create a brief summary:
- What was added/changed/fixed
- Key files affected
- Any breaking changes or things to watch

Send this summary to:
- **The person who requested the merge** (always)
- **Faahim (+8801753303880)** (always — even if he is the one merging, he still gets the summary)

If Faahim is the merger → one message to Faahim.
If someone else is the merger → message to them + separate message to Faahim.

### Step 5: Update ERP Task

After merge:
1. Find the related task on the ERP task board (match by issue title/link)
2. Update task status to DONE via MCP
3. If no matching task found, note it but don't fail

---

## Rules

- **Never fabricate API endpoints, types, or file paths** — verify they exist
- **Never assume a status value, enum, or default** — check the schema/code
- **If you can't verify something, say so explicitly** in the issue ("needs confirmation: ...")
- **Don't go deeper than needed** — you're planning, not implementing
- **Always check `gh label list`** before applying labels — don't use labels that don't exist
- **One issue per distinct problem** — unless problems are tightly coupled
