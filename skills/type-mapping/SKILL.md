---
name: type-mapping
description: Notion to Airtable property type mapping reference. Used automatically during migration to convert field types correctly.
---

# Notion → Airtable Type Mapping

Use this mapping when creating Airtable tables from Notion database schemas.

## Direct Mappings

| Notion Type | Airtable Field Type | Notes |
|---|---|---|
| `title` | `singleLineText` | Primary field. Accepts any string type, plus `date`, `dateTime`, and `formula`. |
| `rich_text` | `multilineText` | Formatting lost. Extract `plain_text` from each segment. |
| `number` | `number` | Map `percent` → percent field, `dollar`/`euro`/etc → currency with symbol. |
| `select` | `singleSelect` | Include all option names. Map colors: default→blueLight2, gray→grayLight2, green→greenLight2, red→redLight2. |
| `multi_select` | `multipleSelects` | Same as select. |
| `status` | `singleSelect` | Flatten all status groups into a flat option list. |
| `date` | `date` | Use `start` value. If `end` exists, create a second `{name} End` field. |
| `checkbox` | `checkbox` | Direct. |
| `url` | `url` | Direct. |
| `email` | `email` | Direct. |
| `phone_number` | `phoneNumber` | Direct. |
| `people` | `singleLineText` | Store display names as plain text. Notion user IDs don't map to Airtable collaborators. |
| `button` | `button` | Create with same label. Action won't transfer — must configure manually. |
| `files` | `multipleAttachments` | Pass as `[{url, filename}]`. **Notion-hosted URLs expire in ~1 hour!** |
| `unique_id` | `singleLineText` | Preserve as "PREFIX-NUMBER" text. |
| `verification` | `checkbox` | Map verified → true. |

## Deferred (create later)

| Notion Type | Airtable Type | When |
|---|---|---|
| `relation` | linked records | After all tables + records exist (Phase 4) |
| `rollup` | rollup | After relations. Configure manually. |
| `formula` | formula | Rewrite syntax manually. See `formula-conversion` skill. |

## Skip (auto-populated)

`created_time`, `created_by`, `last_edited_time`, `last_edited_by` — Airtable fills these automatically.

## Always Add

Every table gets a `_notion_id` field (`singleLineText`) for migration tracking and relation mapping.

## Value Formats for Airtable

| Field Type | Format | Example |
|---|---|---|
| singleLineText | `"string"` | `"Task name"` |
| number | `number` | `42` |
| checkbox | `boolean` | `true` |
| date | `"YYYY-MM-DD"` | `"2024-03-15"` |
| singleSelect | `"option name"` | `"In Progress"` |
| multipleSelects | `["a", "b"]` | `["Urgent", "Bug"]` |
| multipleAttachments | `[{url, filename}]` | `[{"url": "...", "filename": "doc.pdf"}]` |
| linked records | `["recXXX"]` | `["recABC123"]` |
