# Notion → Airtable Migration

A project folder that turns [Claude CoWork](https://claude.ai) into a Notion → Airtable migration agent using MCP tools.

## Quick Start

1. **Connect** — Claude Desktop → Settings → Integrations → add Notion + Airtable (OAuth)
2. **Configure** — Edit `config.json` with your Airtable base ID
3. **Run** — CoWork tab → select this folder → tell Claude: *"Read CLAUDE.md, then start Phase 1"*

See [START.md](START.md) for detailed setup instructions.

## What's Inside

| File | Purpose |
|---|---|
| `CLAUDE.md` | Agent instructions — Claude reads this as its system prompt |
| `config.json` | Your settings — Airtable base ID, migration options |
| `START.md` | Human-readable setup guide |
| `reference/type-mapping.md` | Complete Notion → Airtable property type mapping |
| `state/` | Migration progress saved here (JSON files for resume) |
| `output/` | Reports written here (audit, progress, verification) |

## How It Works

Both Notion and Airtable have official remote MCP servers with pre-built Claude connectors:

- **Notion**: `https://mcp.notion.com/mcp` (OAuth)
- **Airtable**: `https://mcp.airtable.com/mcp` (OAuth)

Claude reads `CLAUDE.md` for migration instructions, connects to both platforms via MCP, and executes a 5-phase migration:

1. **Audit** — Discover all Notion databases and schemas
2. **Create Tables** — Build matching Airtable tables with correct field types
3. **Migrate Records** — Extract, transform, and batch-load all records
4. **Wire Relations** — Connect cross-table linked records using ID mapping
5. **Verify** — Compare counts, spot-check values, list remaining manual work

## Resuming After Interruption

Migration state is saved to the `state/` folder after every batch. If interrupted:

```
Read CLAUDE.md for your instructions. Check the state/ folder
to see what's already been done, then resume from where you left off.
```

## License

MIT