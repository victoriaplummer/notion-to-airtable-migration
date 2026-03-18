---
name: verify
description: Verify a completed Notion to Airtable migration. Compares record counts, spot-checks values, and lists remaining manual work.
---

# Verify Migration

Check that the migration completed correctly.

## What to Do

1. For each migrated table, compare record counts:
   - Query ~~source for the total record count
   - Query ~~target for the total record count
   - Report match/mismatch

2. Spot-check 3 random records per table:
   - Fetch a record from ~~source
   - Find the matching record in ~~target using `_notion_id`
   - Compare key field values

3. List remaining manual work:
   - **Formulas** — show the Notion formula and suggest the Airtable equivalent (see `formula-conversion` skill)
   - **Rollups** — specify which relation field and aggregation to configure
   - **Button actions** — note which buttons need their actions set up
   - **Relations** — report any missing references that couldn't be linked

4. Present the final report:
   - Total records migrated
   - Total relations linked
   - Any data that was lost or couldn't transfer
   - Pass/fail per table

## Rules
- **Never hallucinate.** Only report data from actual tool responses.
