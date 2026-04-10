---
title: "Balance"
---

**Opis:** Komunikat pozwalający na zmianę salda na wybranym koncie dokumentu poprzez jego nadpisanie nową wartością (wykorzystywane tylko przy importach, w których saldo przychodzi z systemu zewnętrznego i DM nie rozksięgowuje wpłat)

**Lokalizacja w bazie integracyjnej:** dm\_messages.balance\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id)5 - zewnętrzny numer dokumentu (External document number - dokument.do_ext_id)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json

```
