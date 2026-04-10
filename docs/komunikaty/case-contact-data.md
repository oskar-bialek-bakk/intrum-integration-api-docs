---
title: "CaseContactData"
---

**Opis:** Komunikat pozwalający powiązanie wybranych danych teleadresowych dłużnika z jedną z jego spraw.

**Lokalizacja w bazie integracyjnej:** dm\_messages.CaseContactData\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)3 - zewnętrzny numer wierzytelności (External contract number wierzytelnosc.wi_ext_id) | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "CaseContactData",
    "message": "
    {
		{
			\"MatchKey\": \"Case_MK_1\",
			\"MatchKeyType\": 6
			\"AddressList\": {
				\"Objects\": [
					{
						\"AdditionalInformation\": \"test\",
						\"AddressTypeId\": 12,
						\"ApartmentNumber\": \"1\",
						\"City\": \"Warszawa\",
						\"Country\": \"Polska\",
						\"ExternalId\": \"1\",
						\"HouseNumber\": \"1\",
						\"PostalCode\": \"01-111\",
						\"PostalOffice\": \"test\",
						\"RegionId\": 1,
						\"SourceId\": 1,
						\"Street\": \"test\"
					}
				],
				\"ObjectsUpdateBehaviour\": 6
			},
			\"EmailList\": {
				\"Objects\": [
					{
						\"AdditionalInformation\": \"test\",
						\"Email\": \"test\",
						\"EmailTypeId\": 1,
						\"ExternalId\": \"1\",
						\"SourceId\": 1
					}
				],
				\"ObjectsUpdateBehaviour\": 6
			},
			\"PhoneNumberList\": {
				\"Objects\": [
					{
						\"Comment\": \"test\",
						\"ExternalId\": \"1\",
						\"PhoneNumber\": \"test\",
						\"PhoneNumberTypeId\": 1,
						\"SourceId\": 1
					}
				],
				\"ObjectsUpdateBehaviour\": 6
			}
		}
    }"
}

```
