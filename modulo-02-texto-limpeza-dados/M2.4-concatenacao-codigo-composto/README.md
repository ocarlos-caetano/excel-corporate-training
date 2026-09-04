# M2.4 — Composite Code Generation for Warehouse Labels

**Module:** 02 — Text Handling & Data Cleaning
**Functions practiced:** `TEXTJOIN` (and a comparison against `CONCAT`)

## Task (as received)

> Carlos, good morning. The Warehouse is migrating to a new labeling system
> and needs 2 fields built from data we already have spread across separate
> columns:
>
> 1. **Codigo_Localizacao** — join Sector + Aisle + Shelf into a single
>    code, separated by hyphens (e.g. `PRD-A1-03`).
> 2. **Descricao_Completa** — join Item Name, Manufacturer, and Notes into
>    one sentence, separated by commas. The problem is that **not every
>    item has a Manufacturer or Notes filled in** — and the description
>    can't end up with a stray comma when a field is empty (e.g.
>    `"Bolt, , "` — that can't happen).
>
> You'll notice you need to think through which tool actually handles both
> cases well.

## Business context

Simulates generating standardized labels/descriptions from scattered
source columns — a common step before printing physical labels or
exporting a clean catalog, where source data often has optional fields
that aren't always filled in.

## Approach

Both fields were solved with **`TEXTJOIN`**:

```
=TEXTJOIN("-", TRUE, [@Setor_Cod], [@Corredor], [@Prateleira])
=TEXTJOIN(", ", TRUE, [@Nome_Item], [@Fabricante], [@Observacao])
```

I initially expected the first field (no blank values involved) to be a
better fit for `CONCAT`, but after comparing the two:

- `CONCAT` needs the delimiter **typed manually between every piece**:
  `=CONCAT([@Setor_Cod], "-", [@Corredor], "-", [@Prateleira])` — the more
  fields you join, the longer and more repetitive the formula gets, since
  the separator has to be re-typed each time.
- `TEXTJOIN` takes the delimiter **once**, as its first argument, and
  applies it automatically between every value that follows — the formula
  stays shorter and easier to read even as more fields get added.

Since `TEXTJOIN` also handles the blank-skipping case that `CONCAT` can't,
and produces a shorter formula even in the simple case, it was used for
both fields.

## Lessons learned

- `TEXTJOIN`'s `ignore_empty` argument (set to `TRUE`) automatically skips
  blank cells, preventing the double-comma / trailing-comma problem that
  would otherwise show up whenever `Fabricante` or `Observacao` is empty.
- `CONCAT` requires the separator to be re-typed between every single
  field being joined, which makes the formula noticeably longer and more
  repetitive as the number of fields grows — `TEXTJOIN` only needs the
  delimiter once, regardless of how many fields are joined.
- In practice, `TEXTJOIN` covers everything `CONCAT` does and more, so it's
  reasonable to default to `TEXTJOIN` unless there's a specific reason
  (e.g. signaling "this formula has no blank-handling logic") to use the
  simpler `CONCAT` instead.
