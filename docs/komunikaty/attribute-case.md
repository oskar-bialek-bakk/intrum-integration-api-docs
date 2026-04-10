---
title: "AttributeCase"
---

**Opis:** Komunikat dodaje/aktualizuje atrybuty sprawy. Zakładamy, że dla jednej sprawy może być tylko jeden atrybut danego typu.

**Lokalizacja w bazie integracyjnej:** dm\_attributeCase\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| Value | string | Tak | Wartość atrybutu | atrybut_wartosc.atw_wartosc |
| TypeId | int | Tak | ID ze słownika atrybut_typ | atrybut_wartosc.atw_att_id |
| ObjectId | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| ToDoAt | Datetime | Nie | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json

```
