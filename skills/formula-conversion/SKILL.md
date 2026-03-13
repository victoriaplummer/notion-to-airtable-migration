---
name: formula-conversion
description: Convert Notion formulas to Airtable formula syntax. Used during migration verification to suggest Airtable formula equivalents.
---

# Notion → Airtable Formula Conversion

Notion and Airtable formulas use completely different syntax. Formulas CANNOT be auto-migrated — they must be manually rewritten. Use this reference to suggest equivalents.

## Property References

| Notion | Airtable |
|---|---|
| `prop("Field Name")` | `{Field Name}` |

## Common Functions

| Notion | Airtable | Notes |
|---|---|---|
| `if(condition, a, b)` | `IF(condition, a, b)` | |
| `length(str)` | `LEN(str)` | |
| `contains(str, sub)` | `FIND(sub, str) > 0` | Args are swapped! |
| `now()` | `NOW()` | |
| `today()` | `TODAY()` | |
| `dateBetween(a, b, "days")` | `DATETIME_DIFF(a, b, 'days')` | Single quotes for unit |
| `dateAdd(date, num, "days")` | `DATEADD(date, num, 'days')` | |
| `and(a, b)` | `AND(a, b)` | |
| `or(a, b)` | `OR(a, b)` | |
| `not(x)` | `NOT(x)` | |
| `abs(x)` | `ABS(x)` | |
| `round(x)` | `ROUND(x, 0)` | Airtable requires precision arg |
| `floor(x)` | `FLOOR(x)` | |
| `ceil(x)` | `CEILING(x)` | |
| `min(a, b)` | `MIN(a, b)` | |
| `max(a, b)` | `MAX(a, b)` | |
| `replace(str, old, new)` | `SUBSTITUTE(str, old, new)` | |
| `lower(str)` | `LOWER(str)` | |
| `upper(str)` | `UPPER(str)` | |
| `trim(str)` | `TRIM(str)` | |
| `toNumber(str)` | `VALUE(str)` | |
| `empty(x)` | `x = BLANK()` | Different pattern |
| `test(str, regex)` | `REGEX_MATCH(str, regex)` | |

## Date Parts

| Notion | Airtable |
|---|---|
| `year(date)` | `YEAR(date)` |
| `month(date)` | `MONTH(date)` |
| `day(date)` | `DAY(date)` |
| `hour(date)` | `HOUR(date)` |
| `minute(date)` | `MINUTE(date)` |

## No Airtable Equivalent

These Notion functions have no direct Airtable equivalent:
- `link()`, `style()`, `unstyle()` — rich text formatting
- `format()` — complex string formatting
- `fromTimestamp()` — manual conversion needed
- `log2()`, `ln()` — use `LOG()` with base conversion

## Common Patterns

**Days until due date:**
- Notion: `dateBetween(prop("Due Date"), now(), "days")`
- Airtable: `DATETIME_DIFF({Due Date}, NOW(), 'days')`

**Is overdue:**
- Notion: `if(prop("Due Date") < now() and not prop("Done"), true, false)`
- Airtable: `IF(AND({Due Date} < NOW(), NOT({Done})), TRUE(), FALSE())`

**Full name:**
- Notion: `prop("First Name") + " " + prop("Last Name")`
- Airtable: `{First Name} & " " & {Last Name}`

**Status emoji:**
- Notion: `if(prop("Status") == "Done", "✅", if(prop("Status") == "In progress", "🔄", "⏳"))`
- Airtable: `IF({Status} = "Done", "✅", IF({Status} = "In progress", "🔄", "⏳"))`
