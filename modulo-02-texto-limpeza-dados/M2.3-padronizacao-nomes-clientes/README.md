# M2.3 — Customer Registry Standardization

**Module:** 02 — Text Handling & Data Cleaning
**Functions practiced:** `PROPER`, `LOWER`, `UPPER`

## Task (as received)

> Carlos, good morning. Our customer registry turned into a mess — each
> attendant typed things their own way (all caps, all lowercase, mixed
> case). Before migrating to the new system, 3 columns need standardizing,
> each with a different rule:
>
> 1. **Nome_Cliente_Padronizado** — person/company name always with the
>    first letter of each word capitalized (proper name format).
> 2. **Email_Padronizado** — email **always lowercase** (the universal
>    standard for email, avoids duplicate registrations caused by case
>    differences).
> 3. **Categoria_Sistema** — the category code needs to go **fully
>    uppercase**, since that's what the new system requires internally.

## Business context

Simulates a very common data migration cleanup: inconsistent manual data
entry across different people/systems needs to be normalized before it can
be trusted for deduplication, search, or system import — and different
fields often have different "correct" casing conventions.

## Approach

- **PROPER** capitalizes the first letter of each word, lowercasing the
  rest:
  `=PROPER([@Nome_Cliente])`
- **LOWER** converts everything to lowercase:
  `=LOWER([@Email])`
- **UPPER** converts everything to uppercase:
  `=UPPER([@Categoria])`

## Lessons learned

- There isn't a single "correct" casing rule for all text — the right
  function depends on the convention for that specific type of data (names
  use Proper Case, emails are conventionally lowercase, system/category
  codes are often uppercase).
- Standardizing casing at the data-cleaning stage prevents downstream
  problems: two "different" emails that are actually the same address
  (`Ana@Gmail.com` vs `ana@gmail.com`) would otherwise be treated as
  distinct records in a lookup, count, or deduplication process.
