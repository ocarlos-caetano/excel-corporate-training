# M1.5 — Maintenance Order Criticality

**Module:** 01 — Conditional Logic & Lookup
**Functions practiced:** `IFS`

## Task (as received)

> Carlos, good morning. Maintenance sent over the list of open work orders
> and I need you to classify the criticality of each one before the 10am
> prioritization meeting. The scale is:
>
> - **Days_Overdue ≤ 0** → "Em Dia" (On Time)
> - **Days_Overdue between 1 and 3** → "Atenção" (Attention)
> - **Days_Overdue between 4 and 7** → "Crítico" (Critical)
> - **Days_Overdue greater than 7** → "Emergencial" (Emergency)
>
> I need the **Criticidade** column filled in. Since these are 4 sequential
> tiers based on a single value — not a combination of AND/OR conditions —
> this is a good candidate for **IFS** instead of nesting several IFs.

## Business context

Simulates a maintenance prioritization routine: classifying overdue work
orders into a criticality scale so the team knows what to tackle first.
Tiered classification by a single numeric value is one of the most common
real-world uses of `IFS`.

## Approach

- `IFS` evaluates each condition **in order** and returns the result of the
  first one that's true — so, like nested IF, the sequence of conditions
  still matters, but without the nested-parentheses structure.

  ```
  =IFS([@Dias_Atraso]<=0, "Em Dia",
       [@Dias_Atraso]<=3, "Atenção",
       [@Dias_Atraso]<=7, "Crítico",
       [@Dias_Atraso]>7, "Emergencial")
  ```

  Note each condition only needs an upper bound (`<=0`, `<=3`, `<=7`)
  because `IFS` stops at the first TRUE match — there's no need to also
  write a lower bound like `>0` for the second tier, since anything that
  reached that point already failed the first condition.

## Lessons learned

- `IFS` is best suited for flat, sequential range classification — a
  single variable being tested against ordered thresholds.
- Nested `IF` (used in M1.4) is still the right tool when the logic
  combines multiple different variables with `AND` / `OR`. Choosing
  between the two is about the *shape* of the business rule, not just
  personal preference.
