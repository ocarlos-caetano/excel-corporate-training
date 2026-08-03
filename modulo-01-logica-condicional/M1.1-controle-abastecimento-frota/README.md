# M1.1 — Fleet Fueling Control

**Module:** 01 — Conditional Logic & Lookup
**Functions practiced:** `SUMIFS`, `COUNTIFS`, structured references (named Table)

## Task (as received)

> Carlos, good morning. I need the weekly fuel closing report before noon.
> Attached is the spreadsheet with this week's fleet fueling records.
>
> I need 3 numbers:
> 1. Total spent (Liters × Price) on vehicles from the **"Manutenção"**
>    (Maintenance) sector that are already marked as **"Pago"** (Paid).
> 2. How many fueling records from the **"Produção"** (Production) sector
>    are still **"Pendente"** (Pending).
> 3. How many fueling records had more than **40 liters**, regardless of
>    sector.
>
> Send it over as soon as you have it, I need it for the 2pm meeting.

## Business context

Simulates a real Supply Chain / Logistics task: consolidating weekly fleet
fuel spending and flagging pending payments by department.

## Approach

- **Valor_Total** calculated row-by-row with structured references:
  `=Abastecimentos[[#This Row],[Litros]]*Abastecimentos[[#This Row],[Preco_Litro]]`
- **SUMIFS** to sum values with two simultaneous criteria (Sector + Status).
- **COUNTIFS** to count occurrences with two criteria (Sector + Status).
- For item 3, a single-criterion count was needed — this led to reviewing
  when `COUNTIF` (singular) is sufficient versus when `COUNTIFS` (2+
  criteria) is actually required.

## Result

| Metric | Value |
|---|---|
| Total Manutenção (Paid) | R$ 1.881,76 |
| Pending Produção | 5 |
| Fuelings > 40L | 9 |

## Lessons learned

- Practical difference between `COUNTIF` and `COUNTIFS`: the number of
  criteria determines which one to use, not simply "always use the S
  version."
- Reinforced the value of named tables + structured references to keep
  formulas readable and auditable — a practice used in real corporate
  environments.
