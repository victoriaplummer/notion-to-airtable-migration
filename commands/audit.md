---
name: audit
description: Audit a Notion workspace before migration. Discovers all databases, maps schemas, identifies relations and edge cases, and presents a migration plan.
---

# Notion Workspace Audit

Discover and analyze all databases in a Notion workspace before migrating to Airtable.

## What to Do

1. Use ~~source `search` to find ALL databases in the workspace
2. For each database, use ~~source `fetch` to get the full schema (properties, types, options)
3. Present a summary:

| Database | Properties | Relations | Files | Formulas | Rollups | Records |
|---|---|---|---|---|---|---|
| [name] | [count] | [list] | ✓/✗ | [count] | [count] | [count if known] |

4. Map the **relation dependency graph** — which databases link to which
5. Flag edge cases:
   - File attachment fields (URLs expire in ~1 hour)
   - Formula fields (must rewrite syntax for Airtable)
   - Rollup fields (must configure after relations)
   - People fields (will become plain text)
   - Button fields (action won't transfer)
   - Date range fields (Notion supports start+end, Airtable only single date)
6. Suggest a **table creation order** based on relation dependencies
7. Ask the user which databases they want to migrate

## Rules
- **Never hallucinate.** Only report data from actual ~~source tool responses.
- **Be fast.** Fetch all schemas in parallel if possible. Don't re-search between databases.
