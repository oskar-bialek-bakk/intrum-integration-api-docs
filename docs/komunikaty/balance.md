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
{
	"importId": "00000000-0000-0000-0000-000000000000",
	"queueName": "Balance",
    "message": "
    {
        \"Objects\": [
        {
			\"OperationDate\": \"2023-10-07T14:32:28.1234567\",
			\"BookingDate\": \"2023-10-07T14:32:28.1234567\",
			\"DueDate\": \"2023-10-07T14:32:28.1234567\",
			\"InterestAccrualDate\": \"2023-10-07T14:32:28.1234567\",
			\"MatchKey\": \"Case_MK_1\",
			\"MatchKeyType\": 6,
			\"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
			\"AccountTypeId\": 2,
			\"Amount\": 44.44,
			\"ToDoAt\": \"2024-04-04T14:17:50.9982539+02:00\"
		},
		{
			\"OperationDate\": \"2023-10-07T14:32:28.1234567\",
			\"BookingDate\": \"2023-10-07T14:32:28.1234567\",
			\"DueDate\": \"2023-10-07T14:32:28.1234567\",
			\"InterestAccrualDate\": \"2023-10-07T14:32:28.1234567\",
			\"MatchKey\": \"Case_MK_1\",
			\"MatchKeyType\": 6,
			\"ObjectId\": \"00000000-0000-0000-0000-000000000001\",
			\"AccountTypeId\": 5,
			\"Amount\": 5.33,
			\"ToDoAt\": \"2024-04-04T14:17:50.9982539+02:00\"
		}
        ],
        \"ObjectsUpdateBehaviour\": 6
    }"
}

```
