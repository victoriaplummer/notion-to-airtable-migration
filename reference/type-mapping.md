# Notion → Airtable Property Type Mapping

## Direct Mappings (Phase 2)

| Notion Type | Airtable Field Type | Config Notes |
|---|---|---|
| `title` | `singleLineText` | Maps to the primary field. Primary fields accept: string types (`singleLineText`, `multilineText`, `richText`, `email`, `url`, `phoneNumber`, `barcode`), plus `date`, `dateTime`, and `formula`. `singleLineText` is the most common choice. |
| `rich_text` | `multilineText` | Formatting lost. Extract `plain_text` from each segment. |
| `number` | `number` | Map format: `percent` → percent field, `dollar`/`euro`/etc → currency |
| `select` | `singleSelect` | Include all options with names. Map colors: default→blueLight2, gray→grayLight2, green→greenLight2, red→redLight2, etc. |
| `multi_select` | `multipleSelects` | Same option mapping as select. |
| `status` | `singleSelect` | Flatten all groups (To-do, In progress, Complete) into a flat list of options. |
| `date` | `date` | Use `start` value as ISO string. If `end` exists, add a second field `{name} End`. |
| `checkbox` | `checkbox` | Direct. |
| `url` | `url` | Direct. |
| `email` | `email` | Direct. |
| `phone_number` | `phoneNumber` | Direct. |
| `people` | `singleLineText` | Store user names as plain text. Notion user IDs don't map to Airtable collaborators — just extract the display name. |
| `button` | `button` | Airtable has a button field type — create it with the same label. **The action won't transfer.** Notion buttons trigger automations; you'll need to manually configure the Airtable button's action (open URL or run automation) after migration. |
| `files` | `multipleAttachments` | Pass as `[{url, filename}]`. **⚠️ Notion-hosted URLs expire in ~1 hour!** |
| `unique_id` | `singleLineText` | Store as "PREFIX-NUMBER" text to preserve the prefix. |
| `verification` | `checkbox` | Map verified → true, unverified → false. |

## Deferred Fields (Phase 4+)

| Notion Type | Airtable Field Type | When to Create |
|---|---|---|
| `relation` | linked records | **After all tables + records exist** (Phase 4). Must know both table IDs. |
| `rollup` | rollup | **After relations exist.** Configure which linked field to aggregate. |
| `formula` | formula | **Manual rewrite.** Notion syntax ≠ Airtable syntax. |

## Auto-Populated (Skip)

| Notion Type | Airtable Equivalent | Notes |
|---|---|---|
| `created_time` | `createdTime` | Airtable auto-creates this. Don't set it. |
| `created_by` | `createdBy` | Airtable auto-creates this. |
| `last_edited_time` | `lastModifiedTime` | Airtable auto-creates this. |
| `last_edited_by` | `lastModifiedBy` | Airtable auto-creates this. |

## Notion Number Format → Airtable Field Type

| Notion Format | Airtable Type | Symbol |
|---|---|---|
| `number` | number | |
| `number_with_commas` | number | |
| `percent` | percent | |
| `dollar` | currency | $ |
| `euro` | currency | € |
| `pound` | currency | £ |
| `yen` / `yuan` | currency | ¥ |
| `rupee` | currency | ₹ |
| `won` | currency | ₩ |
| `canadian_dollar` | currency | CA$ |
| `real` | currency | R$ |
| All other currencies | currency | Look up symbol |

## Airtable Value Formats (for create_records_for_table)

| Field Type | Value Format | Example |
|---|---|---|
| singleLineText | `"string"` | `"Task name"` |
| multilineText | `"string"` | `"Line 1\nLine 2"` |
| number / currency / percent | `number` | `42`, `3.14` |
| checkbox | `boolean` | `true` |
| date | `"YYYY-MM-DD"` | `"2024-03-15"` |
| singleSelect | `"option name"` | `"In Progress"` |
| multipleSelects | `["opt1", "opt2"]` | `["Urgent", "Bug"]` |
| url / email / phoneNumber | `"string"` | `"https://..."` |
| multipleAttachments | `[{url, filename}]` | `[{"url": "https://...", "filename": "doc.pdf"}]` |
| linked records | `["recXXX"]` | `["recABC123", "recDEF456"]` |

## Edge Cases

### File URLs Expire
Notion-hosted files (`type: "file"`) use signed S3 URLs that expire in ~1 hour. External files (`type: "external"`) are permanent. Migrate databases with file properties FIRST.

### >25 Relation References
Notion API only returns the first 25 linked pages per relation property. If `has_more: true`, you need to paginate through the page property endpoint to get all references.

### Date Ranges
Notion dates can have `start` AND `end`. Airtable date fields store a single date. Use the start date in the primary field and create a second `{field_name} End` field for the end date if it exists.

### Rich Text Formatting
Notion rich text has bold, italic, strikethrough, code, color, links. Options:
- **Plain text**: Extract `plain_text` from each segment (default)
- **Markdown**: Convert annotations → `**bold**`, `*italic*`, `~~strike~~`, `` `code` ``, `[text](url)`

### Batch Failures
Airtable create_records fails atomically for a batch of up to 10 records. If one record has a bad value, all 10 fail. Isolate by splitting: try 5+5, then 3+2, then 1+1 until you find the bad record.