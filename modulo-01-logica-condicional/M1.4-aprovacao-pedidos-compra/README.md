# M1.4 — Purchase Order Approval

**Module:** 01 — Conditional Logic & Lookup
**Functions practiced:** Nested `IF` with `AND` / `OR`

## Task (as received)

> Carlos, good morning. I need you to set up this week's purchase order
> approval routing before I send it to the board. Our internal approval
> policy works like this:
>
> - If the **Order Value** is up to R$ 5,000 **AND** the supplier is
>   **Homologated** (pre-approved), it gets automatic approval.
> - If it doesn't meet the rule above, but the value is up to R$ 20,000
>   **OR** the order is flagged as **Urgent**, it goes up to Management
>   approval.
> - Any other case goes to Board approval.
>
> I need the **Nivel_Aprovacao** (Approval Level) column filled in using a
> nested IF with AND/OR for these 3 conditions. Data attached.

## Business context

Simulates an approval routing / authorization matrix — a very common
control mechanism in Supply Chain and Finance, where the value of a
transaction combined with other business flags (urgency, supplier status)
determines who needs to sign off.

## Approach

- Nested `IF`, evaluating the strictest rule first (automatic approval),
  then the intermediate rule (management), falling back to the broadest
  case (board):

  ```
  =IF(AND([@Valor_Pedido]<=5000, [@Fornecedor_Homologado]="Sim"),
      "Aprovação Automática",
      IF(OR([@Valor_Pedido]<=20000, [@Urgente]="Sim"),
          "Aprovação Gerência",
          "Aprovação Diretoria"))
  ```

## Lessons learned

- Order matters in nested conditionals: the most restrictive rule needs to
  be evaluated first, otherwise a broader condition further down could
  incorrectly catch a case that should have matched an earlier, stricter
  rule.
- `AND` requires every condition to be true; `OR` only requires one —
  mixing both inside the same nested structure reflects how real business
  policies are rarely a single flat rule.
