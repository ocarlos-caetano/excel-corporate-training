# M1.2 — IT Asset Control

**Module:** 01 — Conditional Logic & Lookup
**Functions practiced:** `XLOOKUP`, text concatenation with `&`

## Task (as received)

> Carlos, good morning. IT sent over the asset database (notebooks,
> monitors, general equipment) for me to check a few things before
> tomorrow's audit. I need you to bring me the following, using a lookup
> (no manual filtering):
>
> 1. Who is the **Responsible person** and what is the **Status** of asset
>    ID **"AT012"**.
> 2. The **Value** of asset **"AT005"**.
> 3. Bonus: in a single formula, bring back **Department + Responsible
>    person + Value** for asset **"AT020"** (hint: XLOOKUP can return more
>    than one column at once).
>
> Spreadsheet attached.

## Business context

Simulates an IT asset audit request — a common task in corporate
environments where you need to quickly cross-reference asset IDs against a
master list without manually scrolling or filtering.

## Approach

- **XLOOKUP** structure: *what to find → where to find it → what to
  return*.
- Combined multiple return columns into a single formula using `&` to
  concatenate text (including literal text like `" - "` between values).
- Example:
  `=XLOOKUP("AT020", Ativos[ID_Ativo], Ativos[Departamento] & " - " & Ativos[Responsavel] & " - " & Ativos[Valor])`

## Result

| Question | Result |
|---|---|
| Responsible + Status — AT012 | Rebecca Dias - Em uso |
| Value — AT005 | 2350 |
| Department + Responsible + Value — AT020 | Suprimentos - Jeni Alves - 4200 |

## Lessons learned

- XLOOKUP's argument order (find → range → return) finally clicked after
  practicing it hands-on, rather than memorizing it abstractly.
- Debugging note: reused a formula by copying it and changing only part of
  it, but left the original lookup ID unchanged in one case — a reminder to
  always double-check **every** argument when adapting a formula, not just
  the one you meant to change.
