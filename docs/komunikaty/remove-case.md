---
title: "RemoveCase"
---

**Opis:** Komunikat pozwalający na usunięcie wybranej sprawy z systemu.

**Lokalizacja w bazie integracyjnej:** dm\_messages.removeCase\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id)5 - zewnętrzny numer dokumentu (External document number - dokument.do_ext_id)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json

```
