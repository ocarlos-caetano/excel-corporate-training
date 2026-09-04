# M2.5 — Readable Statement Line for Manual Audit

**Module:** 02 — Text Handling & Data Cleaning
**Functions practiced:** `TEXT`, combined with `TEXTJOIN`/`&`

## Task (as received)

> Carlos, good morning. Controladoria is reviewing payments manually
> tomorrow and asked for each spreadsheet row to also appear as a **running
> sentence**, not just a raw number in a separate column — makes it easier
> for whoever is reviewing line by line.
>
> I need the **Descricao_Extrato** column, building a sentence like this
> for each payment:
>
> > "Pagamento de R$ 4.200,00 realizado em 05/07/2026 para Aços do Brasil
> > via Pix."
>
> The problem: if you just concatenate the value and date directly, Excel
> shows the "raw" number (like `4200` or the date's serial number, like
> `46212`), not formatted the way it needs to appear in the sentence.
> You'll need a function that **turns a number into already-formatted
> text** before joining it with the rest of the sentence.

## Business context

Simulates producing a human-readable summary line from structured data —
common whenever a report needs to be read manually (audit, approval,
customer-facing statement) rather than just scanned as a spreadsheet.

## Approach

- **TEXT** converts a number or date into a formatted text string, using a
  format code (the same codes used in cell number formatting):
  `=TEXT([@Valor_Pago], "R$ #.##0,00")`
  `=TEXT([@Data_Pagamento], "DD/MM/AAAA")`
- These formatted pieces are then joined with the rest of the sentence
  using `TEXTJOIN` or `&`:
  ```
  ="Pagamento de " & TEXT([@Valor_Pago], "R$ #.##0,00") & " realizado em " &
    TEXT([@Data_Pagamento], "DD/MM/AAAA") & " para " & [@Fornecedor] &
    " via " & [@Forma_Pagamento] & "."
  ```

## Lessons learned

- Concatenating a number or date directly (with `&` or `TEXTJOIN`) shows
  its raw underlying value — a date becomes its serial number, a currency
  value loses its formatting — because concatenation just converts the
  value to its simplest text form, ignoring how the cell is displayed.
- `TEXT` is the function that bridges that gap: it explicitly converts a
  number into the formatted text the sentence needs, using the same format
  codes available in Excel's cell formatting menu.
- This is a natural combination with `TEXTJOIN`/`&` from earlier in this
  module — `TEXT` prepares each formatted piece, and `TEXTJOIN`/`&`
  assembles them into the final sentence.
