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

Both fields were solved with **`TEXTJOIN`**, referencing a **range of
table columns** instead of listing each field individually:

```
=TEXTJOIN("-", TRUE, Estoque_Itens[@[Setor_Cod]:[Prateleira]])
=TEXTJOIN(", ", TRUE, Estoque_Itens[@[Nome_Item]:[Observacao]])
```

Since `Setor_Cod`, `Corredor`, and `Prateleira` sit next to each other in
the table (and likewise `Nome_Item`, `Fabricante`, `Observacao`), a single
structured-reference range (`[Coluna_Inicial]:[Coluna_Final]`) grabs every
value between them in one shot — shorter than listing each field
separately with commas.

I initially expected the first field (no blank values involved) to be a
better fit for `CONCAT`, but after comparing the two:

- `CONCAT` needs the delimiter **typed manually between every piece**, and
  can't take a column range the way `TEXTJOIN` did here — each field has
  to be listed individually: `=CONCAT([@Setor_Cod], "-", [@Corredor], "-",
  [@Prateleira])`. The more fields joined, the longer and more repetitive
  it gets.
- `TEXTJOIN` takes the delimiter **once**, as its first argument, applies
  it automatically between every value, and — as used here — can pull an
  entire contiguous range of columns in a single reference.

Since `TEXTJOIN` also handles the blank-skipping case that `CONCAT` can't,
and produced the shortest formula in both cases, it was used for both
fields.

## Lessons learned

- `TEXTJOIN`'s `ignore_empty` argument (set to `TRUE`) automatically skips
  blank cells, preventing the double-comma / trailing-comma problem that
  would otherwise show up whenever `Fabricante` or `Observacao` is empty.
- When the fields being joined are **adjacent columns in the same table**,
  referencing them as a range (`[Coluna_Inicial]:[Coluna_Final]`) is
  shorter and easier to maintain than listing each one — if a new column
  gets inserted between them later, the range picks it up automatically.
- `CONCAT` requires each field to be listed and the separator re-typed
  between every one, which makes the formula noticeably longer as the
  number of fields grows — `TEXTJOIN` only needs the delimiter once,
  regardless of how many fields (or how wide a range) is joined.
- In practice, `TEXTJOIN` covers everything `CONCAT` does and more, so it's
  reasonable to default to `TEXTJOIN` unless there's a specific reason
  (e.g. signaling "this formula has no blank-handling logic") to use the
  simpler `CONCAT` instead.
