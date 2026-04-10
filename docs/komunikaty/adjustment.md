---
title: "Adjustment"
---

**Opis:** Komunikat dodaje korekty

**Lokalizacja w bazie integracyjnej:** dm\_messages.adjustment\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id)5 - zewnętrzny numer dokumentu (External document number - dokument.do_ext_id)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json
{
  "importId": "00000000-0000-0000-0000-000000000000",
  "queueName": "Adjustment",
    "message": "{
    \"Objects\": [
	{
 		\"OperationDate\": \"2023-10-07T14:32:28.1234567\",
  		\"BookingDate\": \"2023-10-07T14:32:28.1234567\",
		\"CurrencyId\": 1,
		\"ExchangeRate\": 1,
        \"MatchKey\": \"WRO00/W/WTSPR/9875841/749/23_10013_94\",
        \"MatchKeyType\": 6,
        \"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
        \"Description\": \"Test\",
        \"AccountId\": 2,
		\"Amount\": 20,
        \"ToDoAt\": \"2024-04-04T14:17:50.9982539+02:00\",
		\"SnapshotDate\": 
		{
			\"Date\": \"2025-02-15T00:00:00\"
		}
    },
	    {
        \"MatchKey\": \"WRO00/W/WTSPR/9875841/749/23_10013_94\",
        \"MatchKeyType\": 6,
        \"ObjectId\": \"00000000-0000-0000-0000-000000000001\",
		\"CurrencyId\": 1,
		\"ExchangeRate\": 1,
        \"Description\": \"Test\",
        \"AccountId\": 5,
		\"Amount\": 5,
        \"ToDoAt\": \"2024-04-04T14:17:50.9982539+02:00\",
		\"SnapshotDate\": 
		{
			\"Date\": \"2025-02-15T00:00:00\"
		}
    }
	],
    \"ObjectsUpdateBehaviour\": 6
    }"
}

```
