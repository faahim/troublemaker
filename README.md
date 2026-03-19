# Troublemaker

An [OpenClaw](https://openclaw.ai) skill for investigating bugs, planning features, creating verified GitHub issues, merging PRs, and keeping repositories clean.

## What It Does

Troublemaker handles the full lifecycle from problem discovery to merge cleanup:

1. **Investigate** — traces code paths, verifies findings against actual source code
2. **Plan** — structures findings into actionable issues with root cause, impact, and fix approach
3. **Create Issues** — files GitHub issues with labels, project board fields, and ERP task sync
4. **Merge PRs** — reviews all feedback (human + bot), blocks on unresolved items, merges cleanly
5. **Clean Up** — deletes merged branches automatically, updates ERP task status

Every claim in an issue is code-verified. No assumptions, no fabricated paths or types.

## Installation

Copy the `troublemaker/` directory into your OpenClaw skills folder:

```bash
# Clone
git clone https://github.com/faahim/troublemaker.git

# Option A: User skills directory
cp -r troublemaker ~/.openclaw/skills/troublemaker

# Option B: Workspace skills (project-scoped)
cp -r troublemaker /path/to/workspace/projects/troublemaker
```

OpenClaw will auto-detect the skill from the `SKILL.md` frontmatter.

## Structure

```
troublemaker/
├── SKILL.md                          # Skill definition + workflow
└── references/
    └── github-project.md             # Project board field IDs and GraphQL mutations
```

## Configuration

### GitHub Project Board

Edit `references/github-project.md` with your project's field IDs, option IDs, and iteration dates. These are used for setting Status, Priority, Size, and Iteration on new issues.

To find your project's field IDs:
```bash
gh api graphql -f query='{ node(id: "YOUR_PROJECT_ID") { ... on ProjectV2 { fields(first: 20) { nodes { ... on ProjectV2SingleSelectField { id name options { id name } } ... on ProjectV2IterationField { id name configuration { iterations { id title startDate } } } } } } } }'
```

### ERP Integration

Troublemaker syncs issues to an ERP task board via MCP. This requires the `dekhval-erp-mcp` skill to be installed and configured. If not available, the skill works fine without it — it just skips the ERP sync step.

### Notifications

Team notifications go via WhatsApp through OpenClaw's message tool. Requires WhatsApp channel to be configured in your OpenClaw setup.

## Triggers

The skill activates when you:
- Report a bug or issue
- Request a feature or change
- Ask to merge a PR
- Ask to clean up branches

## License

MIT
