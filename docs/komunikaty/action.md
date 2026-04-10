---
title: "Action"
---

**Opis:** Komunikat dodaje akcje z rezultatem i atrybutami

**Lokalizacja w bazie integracyjnej:** dm\_messages.action\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACJA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)20 - Wszystkie sprawy windykacyjnyjne powiązane z zewnętrznym id wierzytelnosci (wierzytelnosc.wi_ext_id) | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "Action",
	"message": "{
	\"Objects\": [{
        \"ActionTypeId\": 1012,
        \"Comment\": \"\",
        \"IsClosed\": true,
        \"MatchKey\": \"Case_MK_1\",
        \"MatchKeyType\": 6,
        \"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
        \"ResultAddedDate\": \"2023-04-03T00:00:00\",
        \"ResultPlannedDate\": \"2023-04-03T00:00:00\",
        \"ResultTypeId\": null,
        \"ToDoAt\": \"2024-04-08T09:04:10.9982539+02:00\",
        \"AttachedFiles\": {
		\"Objects\": [
            {
            \"Name\": \"Sampkle151.xlsx\",
            \"Description\": \"Updated terms of service.\",
            \"SendDate\": \"2023-04-03T00:00:00\",
            \"SharedPath\": \"C:\\\\Users\\\\Downloads\\\\Sample.xlsx\",
            \"Base64Content\": \"\"
            }],
		\"ObjectsUpdateBehaviour\": 6
		}
    }],
	\"ObjectsUpdateBehaviour\": 6
	}"
}

```
