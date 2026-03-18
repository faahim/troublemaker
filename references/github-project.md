# GitHub Project Board Reference

## Project
- **Name:** faahim's project #4 (dekhval-erp)
- **ID:** `PVT_kwHOAU15384BRNaL`

## Field IDs

### Status (ID: `PVTSSF_lAHOAU15384BRNaLzg_GpZo`)
| Status | Option ID |
|--------|-----------|
| Backlog | `f75ad846` |
| Ready | `e18bf179` |
| In progress | `47fc9ee4` |
| In review | `aba860b9` |
| Done | `98236657` |

### Priority (ID: `PVTSSF_lAHOAU15384BRNaLzg_GpvY`)
| Priority | Option ID |
|----------|-----------|
| P0 | `79628723` |
| P1 | `0a877460` |
| P2 | `da944a9c` |

### Size (ID: `PVTSSF_lAHOAU15384BRNaLzg_Gpvc`)
| Size | Option ID |
|------|-----------|
| XS | `911790be` |
| S | `b277fb01` |
| M | `86db8eb3` |
| L | `853c8207` |
| XL | `2d0801e2` |

### Iteration (ID: `PVTIF_lAHOAU15384BRNaLzg_Gpvk`)
| Iteration | Option ID | Start Date |
|-----------|-----------|------------|
| Iteration 1 | `381c7c80` | 2026-03-09 |
| Iteration 2 | `54cf5c95` | 2026-03-23 |
| Iteration 3 | `d2c335bc` | 2026-04-06 |
| Iteration 4 | `b6a8f1bb` | 2026-04-20 |
| Iteration 5 | `955c1297` | 2026-05-04 |

## How to Set Fields

### Step 1: Add issue to project
```bash
gh api graphql -f query='
mutation {
  addProjectV2ItemById(input: {
    projectId: "PVT_kwHOAU15384BRNaL"
    contentId: "<ISSUE_NODE_ID>"
  }) {
    item { id }
  }
}'
```

Get issue node ID: `gh issue view <number> --json id -q .id`

### Step 2: Set single-select fields (Status, Priority, Size)
```bash
gh api graphql -f query='
mutation {
  updateProjectV2ItemFieldValue(input: {
    projectId: "PVT_kwHOAU15384BRNaL"
    itemId: "<ITEM_ID>"
    fieldId: "<FIELD_ID>"
    value: { singleSelectOptionId: "<OPTION_ID>" }
  }) {
    projectV2Item { id }
  }
}'
```

### Step 3: Set iteration field
```bash
gh api graphql -f query='
mutation {
  updateProjectV2ItemFieldValue(input: {
    projectId: "PVT_kwHOAU15384BRNaL"
    itemId: "<ITEM_ID>"
    fieldId: "PVTIF_lAHOAU15384BRNaLzg_Gpvk"
    value: { iterationId: "<ITERATION_ID>" }
  }) {
    projectV2Item { id }
  }
}'
```

## Determining Current Iteration

Compare today's date against iteration start dates. Each iteration is 2 weeks. Pick the iteration whose start date <= today and next iteration start date > today.
