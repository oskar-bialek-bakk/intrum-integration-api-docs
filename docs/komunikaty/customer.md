---
title: "Customer"
---

**Opis:** Komunikat dodaje dłużnika z atrybutami dłużnika

**Lokalizacja w bazie integracyjnej:** dm\_messages.debtor\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | dluznik.dl_ext_id (dla MatchKeyTypeID = 2) |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:2 - zewnętrzny numer dłużnika (External customer number dluznik.dl_ext_id)6 - External case number - sprawa.sp_ext_id3 - External contract number wierzytelnosc.wi_ext_id | Brak |

```json

```
