# M2.1 — Asset Code Migration

**Module:** 02 — Text Handling & Data Cleaning
**Functions practiced:** `LEFT`, `MID`, `RIGHT`

## Task (as received)

> Carlos, good morning. We're migrating the asset registry to a new system,
> and the legacy asset code comes packed into a single field — we need to
> split it into columns before importing.
>
> The code format is always fixed: `PPP-AAAA-NNNNN-UF` (17 characters),
> where:
> - **PPP** = 3-letter sector code (e.g. PRD, ENG, MNT)
> - **AAAA** = 4-digit acquisition year
> - **NNNNN** = 5-digit sequential number
> - **UF** = 2-letter state code
>
> I need you to split this into 3 new columns:
> 1. **Setor_Cod** — the first 3 letters.
> 2. **Ano_Aquisicao** — the 4-digit year (in the middle of the text).
> 3. **UF** — the last 2 letters.
>
> Spreadsheet attached.

## Business context

Simulates a very common data migration task: legacy systems often encode
multiple pieces of information into a single fixed-format string, and any
system migration or reporting rebuild requires splitting that string back
into structured columns.

## Approach

- **LEFT** to grab a fixed number of characters from the start of the
  string:
  `=LEFT([@Codigo_Patrimonio], 3)`
- **RIGHT** to grab a fixed number of characters from the end:
  `=RIGHT([@Codigo_Patrimonio], 2)`
- **MID** to grab characters from the middle, given a starting position and
  a length:
  `=MID([@Codigo_Patrimonio], 5, 4)`

Since the code has a fixed length and fixed structure, all three functions
rely on counting character positions manually (position 5 is where the
year starts, right after `PPP-`).

## Lessons learned

- `LEFT`/`RIGHT` only need "how many characters," while `MID` needs "where
  to start" plus "how many characters" — the extra argument is what usually
  trips people up the first time.
- This kind of fixed-position extraction only works reliably when the
  source string has a **guaranteed, consistent format**. If the code length
  or structure varied row to row, this approach would break — a different
  technique (like splitting on the `-` delimiter) would be needed instead.
