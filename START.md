# How To Use This

## What This Is

A project folder that turns Claude CoWork into a Notion → Airtable migration agent. Drop this folder into CoWork, connect both MCP servers, edit one config file, and go.

## Setup (3 steps)

### 1. Connect MCP Servers

In Claude Desktop, go to **Settings → Integrations** and connect both:

- **Notion** — click connect, sign in, grant access to your workspace
- **Airtable** — click connect, sign in, grant access to your bases

Both use OAuth. No tokens. No config files.

### 2. Edit config.json

Open `config.json` and replace `REPLACE_WITH_YOUR_BASE_ID` with your Airtable base ID.

Find it in the URL when you open your base: `https://airtable.com/appXXXXXXXXXXXX/...`

### 3. Open in CoWork

1. Open Claude Desktop → **CoWork** tab
2. Click "Work in a Folder" → select this folder
3. Tell Claude:

```
Read CLAUDE.md for your instructions, then read config.json.
Start with Phase 1: audit my Notion workspace.
Show me what you find before proceeding.
```

That's it. Claude reads the instructions, connects to both platforms via MCP, and starts migrating.

## What's In This Folder

```
notion-to-airtable-migration/
├── CLAUDE.md              ← Agent instructions (Claude reads this)
├── config.json            ← Your settings (base ID, options)
├── START.md               ← You are here
├── reference/
│   └── type-mapping.md    ← Full property type mapping reference
├── state/                 ← Migration progress (auto-created)
│   ├── notion-schemas.json
│   ├── table-map.json
│   ├── id-map.json
│   └── failed-records.json
└── output/                ← Reports (auto-created)
    ├── audit-report.md
    ├── migration-progress.md
    └── verification-report.md
```

## Resuming After Interruption

If Claude stops mid-migration (context limit, you close the laptop, etc.):

1. Open CoWork with this folder again
2. Tell Claude:

```
Read CLAUDE.md for your instructions. Check the state/ folder
to see what's already been done, then resume from where you left off.
```

Claude reads `state/id-map.json` and `state/table-map.json` to know what's already migrated.

## After Migration

Check `output/verification-report.md` for the results. Then manually:

1. **Add relation fields** in Airtable (Claude will tell you exactly which ones)
2. **Rewrite formulas** using the conversion table in CLAUDE.md
3. **Configure rollups** after relations exist
4. **Recreate views** (filters, sorts, grouping)