---
title: "Signature"
---

**Opis:** Komunikat dodaje sygnatury

**Lokalizacja w bazie integracyjnej:** dm\_messages.signature\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "Signature",
    "message": "
    {
        \"Objects\": [
        {
            \"MatchKey\": \"Case_MK_1\",
            \"MatchKeyType\": 6,
            \"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
            \"SignatureDateFrom\": \"2024-04-04T12:20:50.7489216+02:00\",
            \"SignatureNumber\": \"Nc-e xxxxxxx/xx\",
            \"SignatureTypeId\": 2,
            \"ToDoAt\": \"2024-04-04T14:17:50.9982539+02:00\"
        }
        ],
        \"ObjectsUpdateBehaviour\": 6
    }"
}

```
