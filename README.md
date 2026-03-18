# Notion → Airtable Migration Plugin

Migrate your Notion workspace to Airtable through conversation. Audit databases, create matching tables, transfer records, wire relations, and verify — all from Cowork.

## Installation

### Cowork
Browse plugins in the Customize menu → Install "Notion to Airtable".

Or upload this plugin file directly.

### Claude Code
```bash
claude plugin install victoriaplummer/notion-to-airtable-migration
```

## Commands

| Command | What It Does |
|---|---|
| `/migrate` | Run the full migration: audit → create tables → transfer records → wire relations → verify |
| `/audit` | Just audit your Notion workspace — see what's there before committing to a migration |
| `/verify` | Verify a completed migration — count checks, spot checks, remaining manual work |

## Skills

Domain knowledge Claude uses automatically during migration:

| Skill | What It Does |
|---|---|
| `type-mapping` | Converts Notion property types to Airtable field types |
| `edge-cases` | Handles file URL expiry, batch failures, date ranges, and other gotchas |
| `formula-conversion` | Suggests Airtable formula equivalents for Notion formulas |

## How It Works

1. **You say:** `/migrate`
2. **Claude asks:** Which Airtable base to migrate into
3. **Claude audits:** Discovers all your Notion databases and schemas
4. **You choose:** Which databases to migrate
5. **Claude migrates:** Creates tables, transfers records, wires relations
6. **Claude verifies:** Counts, spot-checks, and reports what needs manual work

Both Notion and Airtable connect via OAuth — no API tokens needed.

## Connectors

This plugin uses two MCP connectors:

| Service | URL | Auth |
|---|---|---|
| Notion | `https://mcp.notion.com/mcp` | OAuth |
| Airtable | `https://mcp.airtable.com/mcp` | OAuth |

You'll be prompted to sign in when first used. See CONNECTORS.md for details.

## What Transfers vs. What Doesn't

| Transfers automatically | Needs manual work after |
|---|---|
| Text, numbers, dates, checkboxes | Formula fields (different syntax) |
| Select/multi-select with options | Rollup fields (configure after relations) |
| Relations (linked records) | Button actions (field exists, action doesn't) |
| File attachments | Views (filters, sorts, grouping) |
| Status options (groups flatten) | Automations |

## License

MIT
