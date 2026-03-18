---
name: migrate
description: Run a full Notion to Airtable migration. Audits your workspace, creates tables, transfers records, wires relations, and verifies. Ask for the target Airtable base ID, then execute all 5 phases.
---

# Notion → Airtable Migration

Migrate a Notion workspace to Airtable in one go.

## Before Starting

Ask the user for their **Airtable base ID** (from the URL: `https://airtable.com/appXXXXXXXXXXXX/...`). If they don't know it, use ~~target to list their bases and help them pick one.

## CRITICAL RULES

### Never Hallucinate Data
- **NEVER invent, guess, or fabricate field values.** Every value written to Airtable MUST come from an actual ~~source tool response.
- **If you didn't fetch a record, you don't have its data.** Do not fill in from memory or pattern-matching.
- **Fetch EVERY record before writing it.** If you fetched 2 out of 10, migrate those 2 and go back for the rest.
- **Prefer an empty field over a wrong field.**

### Be Fast — Minimize Tool Calls
- **Fetch ALL records in one call** at max page size. Don't fetch 2 at a time.
- **Batch writes at 10** (the max). Never 1 or 2 at a time.
- **Don't re-search** between tables. Don't re-read instructions mid-run.
- A single-table migration should take ~5-6 tool call turns total.

## Execution Flow

### Phase 1: Audit
1. Use ~~source `search` to find all databases in the workspace
2. For each database, use ~~source `fetch` to get the full schema
3. Map cross-table relations (which databases link to which)
4. Present the audit to the user. Ask which databases to migrate.

### Phase 2: Create Airtable Tables
For each chosen database:
1. Map Notion property types to Airtable field types (see the `type-mapping` skill)
2. Check for name collisions with ~~target `list_tables_for_base`
3. Create the table with ~~target, including a `_notion_id` tracking field
4. Skip relation, rollup, and formula fields — they come later

### Phase 3: Migrate Records
For each database, in dependency order:
1. Fetch ALL records from ~~source (paginate fully — check `has_more`)
2. Extract property values, converting types per the mapping
3. Batch-create in ~~target (10 records per call, `typecast: true`)
4. Track the ID mapping: notion_page_id → airtable_record_id
5. If a batch fails, split in half and retry to isolate the bad record

**⚠️ Databases with file attachments go FIRST** — Notion file URLs expire in ~1 hour.

### Phase 4: Wire Relations
1. For each table with relation properties:
   - Translate Notion page IDs → Airtable record IDs using the mapping
   - Update records in ~~target with linked record IDs
2. Only set one side of bidirectional relations (Airtable auto-creates the reverse)

### Phase 5: Verify
1. Compare record counts per table (~~source vs ~~target)
2. Spot-check 3 random records
3. List remaining manual work: formulas to rewrite, rollups to configure, button actions to set up
4. Present the final report to the user

## Airtable Tool Flow
Always discover IDs first: `search_bases` → `list_tables_for_base` → `get_table_schema` → then read/write. Do this discovery ONCE, not per table.
