# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool the user connects in that category. This plugin uses two connectors:

| Category | Placeholder | Included servers | Other options |
|----------|-------------|-----------------|---------------|
| Source database | `~~source` | Notion | — |
| Target database | `~~target` | Airtable | — |

Both connectors use OAuth — you sign in and grant access when prompted. No API tokens needed.
