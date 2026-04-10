---
title: "Case"
---

!!! warning "Wymagane pola"
    Pola `caseNumber` i `debtorId` są wymagane w każdym komunikacie Case.

**Opis:** Komunikat dodaje sprawę z atrybutami sprawy oraz powiązania klientów i wierzytelności do sprawy

**Lokalizacja w bazie integracyjnej:** dm\_messages.case\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | sprawa.sp_ext_id |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id) | Brak |

```json
{
	"importId": "00000000-0000-0000-0000-000000000000",
	"queueName": "Case",
    "message": "
    {
        \"Objects\": [
        {
			\"ExternalCaseNumber\": \"SZC00/W/WTSPR/9877817/33/24_10013_94\",
			\"RepaymentAccount\": \"17124069577501000008529904\",
			\"CreateRepaymentAccount\": false,
			\"Comment\": null,
			\"ServiceStartDate\": \"2023-09-18T00:00:00\",
			\"ServiceEndDate\": null,
			\"StageAction\": {
				\"ActionTypeId\": 20,
				\"ResultTypeId\": null,
				\"Comment\": \"\",
				\"ResultAddedDate\": \"2024-04-10T00:00:00+02:00\",
				\"ResultPlannedDate\": \"2024-04-10T00:00:00+02:00\",
				\"IsClosed\": true,
				\"MatchKey\": null,
				\"MatchKeyType\": 0
			},
			\"AttributesList\":     
			{
				\"Objects\": [
				{
					\"Value\": \"test\",
					\"TypeId\": 126
				}
				],
				\"ObjectsUpdateBehaviour\": 6
			},
			\"CustomerContractList\":     
			{
				\"Objects\": [
				{
					\"CustomerRoleId\": 1,
					\"CustomerMatchKey\": \"Customer_MK_1\",
					\"CustomerMatchKeyType\": 2,
					\"ContractMatchKey\": \"Contract_MK_1\",
					\"ContractMatchKeyType\": 3
				}
				],
				\"ObjectsUpdateBehaviour\": 6
			},
			\"MatchKey\": \"Case_MK_1\",
			\"MatchKeyType\": 6,
			\"ObjectId\": \"00000000-0000-0000-0000-000000000000\"
		}
        ],
        \"ObjectsUpdateBehaviour\": 6
    }"
}
```
