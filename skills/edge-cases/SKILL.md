---
name: edge-cases
description: Known edge cases and gotchas for Notion to Airtable migration. Referenced automatically when these situations arise during migration.
---

# Migration Edge Cases

## File URLs Expire (CRITICAL)
Notion-hosted files (`type: "file"`) use signed S3 URLs that expire in ~1 hour. External files (`type: "external"`) are permanent. **Migrate databases with file properties FIRST** while URLs are fresh. If URLs expire, re-fetch the records from ~~source to get new URLs.

## Batch Failures (CRITICAL)
Airtable `create_records_for_table` fails atomically — if 1 record in a batch of 10 has a bad value, all 10 fail. **Isolate by splitting:** try 5+5, then 3+2, then 1+1 until you find the bad record. Never discard a whole batch.

## >25 Relation References
Notion API only returns the first 25 linked pages per relation property. If `has_more: true`, paginate to get ALL references before writing relations.

## Date Ranges
Notion dates can have `start` AND `end`. Airtable date fields store a single date. Use start in the primary date field, create a second `{name} End` field for the end date.

## Rich Text Formatting
Notion rich text includes bold, italic, strikethrough, code, color, links. Airtable `multilineText` is plain text. Extract `plain_text` from each segment, or convert to markdown (`**bold**`, `*italic*`).

## Status Groups
Notion status has groups (To-do, In progress, Complete). Airtable `singleSelect` is flat — groups are lost. Options transfer fine, just no grouping.

## People Fields
Notion `people` references workspace users by ID. Airtable collaborator fields reference different users. Store as plain text (display names).

## Button Fields
Both Notion and Airtable have button fields. The field transfers, but the **action** (automation trigger, URL) does NOT. Must configure the button's action manually in Airtable after migration.

## Nested/Sub-Pages
Notion supports pages within pages. Airtable is flat. Either flatten into one table with a "Parent" self-link, or ignore nesting and just migrate database records.

## Page Body Content
Notion pages have rich body content (paragraphs, headings, images). Airtable records don't. Either skip it or store as markdown in a long text field.
