---
title: "CaseContactData"
---

**Opis:** Komunikat pozwalający powiązanie wybranych danych teleadresowych dłużnika z jedną z jego spraw.

**Lokalizacja w bazie integracyjnej:** dm\_messages.CaseContactData\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)3 - zewnętrzny numer wierzytelności (External contract number wierzytelnosc.wi_ext_id) | Brak |

```json

```
