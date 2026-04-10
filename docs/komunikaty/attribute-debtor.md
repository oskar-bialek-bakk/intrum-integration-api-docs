---
title: "AttributeDebtor"
---

**Opis:** Komunikat dodaje/aktualizuje atrybuty klienta. Zakładamy, że dla klienta może być tylko jeden atrybut danego typu.

**Lokalizacja w bazie integracyjnej:** dm\_attributeDebtor\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACJA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:2 - zewnętrzny numer dłużnika (External customer number dluznik.dl_ext_id) | Brak |

```json
{
	"importId": "00000000-0000-0000-0000-000000000000",
	"queueName": "AttributeDebtor",
	"message": "
	{
		\"Objects\": [
		{
			\"MatchKey\": \"Customer_MK_1\",
			\"MatchKeyType\": 2,
			\"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
			\"ToDoAt\": \"2024-04-08T08:59:50.9982539+02:00\",
			\"TypeId\": 2,
			\"Value\": \"90\"
		}
		],
		\"ObjectsUpdateBehaviour\": 6
	}"
}

```
