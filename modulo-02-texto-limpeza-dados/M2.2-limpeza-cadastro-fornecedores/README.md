# M2.2 — Supplier Registry Cleanup

**Module:** 02 — Text Handling & Data Cleaning
**Functions practiced:** `TRIM`

## Task (as received)

> Carlos, good morning. We imported the supplier registry from a legacy
> system, and Supply Chain is complaining that XLOOKUP isn't finding
> suppliers that clearly exist in the base — you can see them right there,
> but the formula throws an error.
>
> This is a classic symptom of invisible extra spaces coming from a badly
> exported system (leading space, trailing space, or a double space in the
> middle of the name). I need you to:
>
> 1. Create a **Nome_Fornecedor_Limpo** column, removing any extra spacing
>    (leading, trailing, or duplicated in the middle) from
>    **Nome_Fornecedor**.
> 2. In the test cells already on the sheet, try an XLOOKUP searching for
>    "Solda Forte Ltda" exactly as it appears against the **dirty** column
>    (`Nome_Fornecedor`) — see what happens. Then try the same search
>    against the **clean** column. This will show you firsthand why this
>    cleanup matters.

## Business context

One of the most common real-world data issues: values that look identical
on screen but fail every exact-match comparison (lookup, filter, grouping)
because of invisible extra whitespace baked in during export or manual
entry.

## Approach

- **TRIM** removes leading/trailing spaces and collapses any internal
  double (or more) spaces down to a single space:
  `=TRIM([@Nome_Fornecedor])`
- Testing the same `XLOOKUP` search term against the dirty column versus
  the clean column demonstrates the failure directly: the dirty column
  fails to match a manually typed search term even though the text "looks"
  the same, while the clean column matches correctly.

## Lessons learned

- Extra whitespace is invisible on screen but breaks exact-match logic —
  `"Solda Forte Ltda "` (trailing space) and `"Solda Forte Ltda"` are
  different strings as far as Excel is concerned, even though they render
  identically.
- `TRIM` should be a standard first step whenever working with text
  imported from an external system, before running any lookup or
  comparison logic against it — cleaning the data up front prevents
  confusing "it's right there, why isn't it matching" bugs downstream.
