---
title: "Payment"
---

**Opis:** Komunikat dodaje płatności

**Lokalizacja w bazie integracyjnej:** dm\_messages.payment\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:1 - numer rachunku bankowego (Bank Account - rachunek_bankowy.rb_nr)4 - numer sprawy (Case number - sprawa.sp_numer)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)21 - dowolna (jedna) ze spraw windykacyjnych powiązana z zewnętrznym id wierzytelnosci (wierzytelnosc.wi_ext_id)22 - dowolna (jedna) ze spraw windykacyjnych powiązana z zewnętrznym id dokumentu (dokumentu .do_ext_id) | Brak |

```json

```
