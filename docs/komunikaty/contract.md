---
title: "Contract"
---

**Opis:** Komunikat dodaje wierzytelność z inicjalnym stanem finansowym

**Lokalizacja w bazie integracyjnej:** dm\_messages.contract\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACJA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | wierzytelnosc.wi_ext_id |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id) | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "Contract",
    "message": "
    {
		\"Objects\": [
			{
				\"AttributesList\": {
					\"Objects\": [
						{
							\"TypeId\": 2,
							\"Value\": \"10\"
						}
					],
					\"ObjectsUpdateBehaviour\": 6
				},
				\"CurrencyId\": 1,
				\"DocumentsList\": {
					\"Objects\": [
						{
							\"AttributesList\": {
								\"Objects\": [
									{
										\"TypeId\": 2,
										\"Value\": \"1\"
									}
								],
								\"ObjectsUpdateBehaviour\": 6
							},
							\"BookingsList\": {
								\"Objects\": [
									{
                                        \"AccountTypeId\": 2,
                                        \"Amount\": 100,
                                        \"DueDate\": \"2023-04-06T00:00:00\",
                                        \"SubAccountTypeId\": 0
									}
								],
								\"ObjectsUpdateBehaviour\": 6
							},
                            \"Comment\": \"Dodatkowy komentarz\",
                            \"CurrencyId\": 1,
                            \"ExchangeRate\": 1,
                            \"Description\": \"Windykacja twarda - wydanie gazomierzy\",
                            \"DocumentTypeId\": 30,
                            \"InterestAccountId\": 1,
                            \"InterestCalculationDate\": \"2023-04-06T00:00:00\",
                            \"InterestTypeId\": 1,
                            \"IssueDate\": \"2023-03-16T00:00:00\",
                            \"LimitationDate\": \"2023-03-16T00:00:00\",
                            \"MatchKey\": \"123456\",
                            \"Number\": \"457457\",
                            \"OpeningBalance\": 250,
                            \"Title\": \"Wydanie gazomierza\"
						}
					],
					\"ObjectsUpdateBehaviour\": 6
				},
				\"EndDate\": null,
				\"MatchKey\": \"Contract_MK_1\",
				\"MatchKeyType\": 3,
				\"Number\": \"521578\",
				\"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
				\"OrginalCreditorId\": 1,
				\"PortfolioId\": 1,
				\"ProductTypeId\": 1,
				\"RegisterDate\": \"1900-01-01T00:00:00\",
				\"Title\": \"Wydanie gazomierza\",
				\"TypeId\": 1,
				\"VersionId\": null,
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
