# M1.3 — Invoice Consolidation

**Module:** 01 — Conditional Logic & Lookup
**Functions practiced:** `IFERROR`, combined with division and `XLOOKUP`

## Task (as received)

> Carlos, good afternoon. I need you to put together an invoice
> reconciliation summary before today's closing. The problem is the source
> data has a few values that came in wrong from the system — division by
> unregistered suppliers, for example — and it's throwing errors in the
> spreadsheet.
>
> I need you to:
> 1. Calculate the **Unit Value** (Total Value ÷ Quantity) for each invoice
>    — but without letting an ugly error (like `#DIV/0!`) show up when
>    Quantity is zero or blank. In those cases, show **"Revisar
>    Quantidade"** (Review Quantity) instead.
> 2. Do the same treatment for any supplier lookup that isn't found.
>
> You'll notice the data has an intentional "problem" — that's exactly how
> real system data arrives, full of gaps.

## Business context

Simulates a very common real-world scenario: consolidating invoice data
exported from an ERP system that contains bad or incomplete records (zeroed
quantities, unregistered supplier codes), which is exactly where raw
formulas break and `IFERROR` becomes essential.

## Approach

- **Unit value**, wrapping the division that could fail (`#DIV/0!`) when
  Quantity is zero:
  `=IFERROR(Notas_Fiscais[[#This Row],[Valor_Total]]/Notas_Fiscais[[#This Row],[Quantidade]], "Revisar Quantidade")`
- **Supplier name**, wrapping an `XLOOKUP` against a supplier master table
  that intentionally has missing entries:
  `=IFERROR(XLOOKUP(Notas_Fiscais[[#This Row],[ID_Fornecedor]], Fornecedores[ID_Fornecedor], Fornecedores[Nome_Fornecedor]), "Fornecedor não cadastrado")`

## Result

| Case | Outcome |
|---|---|
| Zeroed quantity (3 invoices) | Displayed "Revisar Quantidade" instead of `#DIV/0!` |
| Unregistered supplier (2 invoices) | Displayed "Fornecedor não cadastrado" instead of `#N/A` |
| All other rows | Calculated normally |

## Lessons learned

- `IFERROR` wraps the entire operation that might fail — not just part of
  it — so the fallback text only triggers when the actual calculation
  breaks.
- The same pattern (*operation that can fail → wrap in IFERROR*) applies
  regardless of what's inside: a division, a lookup, or any other formula
  prone to producing an error value. Recognizing that pattern mattered more
  than memorizing the exact syntax.
