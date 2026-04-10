---
title: "Case"
---

!!! warning "Wymagane pola"
    Pola `caseNumber` i `debtorId` są wymagane w każdym komunikacie Case.

**Opis:** Komunikat dodaje sprawę z atrybutami sprawy oraz powiązania klientów i wierzytelności do sprawy

**Lokalizacja w bazie integracyjnej:** dm\_messages.case\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | sprawa.sp_ext_id |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json

```
