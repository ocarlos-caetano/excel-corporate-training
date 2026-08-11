# Module 1 Assessment — Supply Chain Express Audit

**Module:** 01 — Conditional Logic & Lookup (Final Assessment)
**Functions combined:** `XLOOKUP`, `IFERROR`, nested `IF` (AND/OR), `IFS`, `SUMIFS`, `COUNTIFS`

## Task (as received)

> Carlos, Internal Audit just called: a surprise internal review is landing
> tomorrow morning. I need the complete package today by 6pm, no room for
> error — this goes straight to the board.
>
> Attached is this month's purchase order data, full of gaps (just like a
> real system export: unregistered suppliers, zeroed quantities). I need
> you to:
>
> **Clean and enrich the data:**
> 1. **Nome_Fornecedor** and **Homologado** — look up in the Suppliers
>    sheet. Any supplier not found should show "Fornecedor não cadastrado".
> 2. **Valor_Unitario** (Order Value ÷ Quantity) — handle division by zero
>    by showing "Revisar Quantidade".
> 3. **Nivel_Aprovacao** — same approval policy as always: up to R$ 5,000
>    AND homologated supplier = Automatic; up to R$ 20,000 OR Urgent =
>    Management; anything else = Board.
> 4. **Criticidade** — same overdue-days scale as always: ≤0 On Time, ≤3
>    Attention, ≤7 Critical, >7 Emergency.
>
> **Then build a 4-number summary below the table:**
> 5. Total **Valor_Pedido** for the **"Manutenção"** sector with Status
>    **"Pendente"**.
> 6. How many orders are **"Emergencial"** criticality **AND** flagged
>    **Urgente = "Sim"**.
> 7. Total **Valor_Pedido** for orders placed **between 07/01/2026 and
>    07/15/2026** (inclusive).
> 8. How many orders fell into **Board approval** **AND** have an
>    unregistered supplier.
>
> No hints on which function to use where — that's for you to decide.

## Business context

This assessment combines every function from Module 1 into a single,
unscripted business demand: a supply chain audit package under time
pressure, with intentionally messy source data (missing supplier records,
zeroed quantities) that has to be handled gracefully rather than left to
throw raw Excel errors.

## Approach

- **XLOOKUP + IFERROR** to resolve supplier name/status against a master
  table with intentional gaps.
- **IFERROR** wrapping a division that could produce `#DIV/0!` on zeroed
  quantities.
- **Nested IF with AND/OR** to route each order through a 3-tier approval
  policy.
- **IFS** to classify overdue days into a 4-tier criticality scale.
- **SUMIFS** for two multi-criteria totals, one of them using a date range
  (the same date column tested twice, with `>=` and `<=`).
- **COUNTIFS** for two multi-criteria counts.

## Result

| Question | Answer |
|---|---|
| Total Manutenção + Pendente | R$ 103.000 |
| Emergencial + Urgente (count) | 3 |
| Total orders 01–15/07/2026 | R$ 136.400 |
| Board approval + unregistered supplier (count) | 0 |

All 20 rows across the 4 enriched columns validated correctly, including
every edge case (unregistered suppliers, zeroed quantities, boundary values
in the approval and criticality tiers).

## Lessons learned

- **SUMIFS is not a general substitute for COUNTIFS.** SUMIFS answers "what
  is the total of a numeric column, given some criteria" — it still needs a
  real column to sum. COUNTIFS answers "how many rows match the criteria,"
  with no numeric column required. Summing an unrelated column (in this
  case, `Quantidade`) to answer a "how many orders" question produced a
  wrong result that happened to look plausible at first glance — a good
  reminder to check whether the business question is really asking for a
  total or a count before picking the function.
- Consistent, standardized category text (e.g. always "Em Dia", never a
  mix of "Em Dia" and "Em dia") matters beyond just calculation —
  inconsistent text breaks grouping in future PivotTables, even when the
  underlying formula logic is correct.
- Combining all 5 functions from the module in one unscripted, business-realistic
  scenario — instead of practicing each in isolation — is what actually
  confirms the logic is internalized, not just memorized syntax.
