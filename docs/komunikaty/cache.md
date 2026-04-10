---
title: "Cache"
---

**Opis:** Komunikat Odświeża wybrane domeny cache dla danego match key. Uwaga - komunikat jest komunikatem technicznym i jego pojawienie się w bazie może wynikać z przetwarzania innych komunikatów, jak i ręcznego zakolejkowania w EnqueueMessage.

**Lokalizacja w bazie integracyjnej:** dm\_messages.cache\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:1 - numer rachunku bankowego (Bank Account - rachunek_bankowy.rb_nr)2 - zewnętrzny numer dłużnika (External customer number dluznik.dl_ext_id)3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id)4 - numer sprawy (Case number - sprawa.sp_numer)5 - zewnętrzny numer dokumentu (External document number - dokument.do_ext_id)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)20 - Wszystkie sprawy windykacyjnyjne powiązane z zewnętrznym id wierzytelnosci (wierzytelnosc.wi_ext_id)21 - dowolna (jedna) ze spraw windykacyjnych powiązana z zewnętrznym id wierzytelnosci (wierzytelnosc.wi_ext_id) | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "cache",
    "message": "
    {
        \"Objects\": [
            {
                \"MatchKey\": \"3_15237574252923798622544812_200785453\",
                \"MatchKeyType\": 6,
                \"ObjectId\": \"00000000-0000-0000-0000-000000000001\",
                \"ToDoAt\": \"2024-04-08T09:04:10.9982539+02:00\"
            },
            {
                \"MatchKey\": \"3_96164366624736830645821115_7743999\",
                \"MatchKeyType\": 6,
                \"ObjectId\": \"00000000-0000-0000-0000-000000000002\",
                \"ToDoAt\": \"2024-04-08T09:04:10.9982539+02:00\"
            }
        ],
        \"ObjectsUpdateBehaviour\": 6
    }"
}

```
