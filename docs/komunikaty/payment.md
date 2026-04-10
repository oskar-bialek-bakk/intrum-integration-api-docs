---
title: "Payment"
---

**Opis:** Komunikat dodaje płatności

**Lokalizacja w bazie integracyjnej:** dm\_messages.payment\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACJA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | Brak |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:1 - numer rachunku bankowego (Bank Account - rachunek_bankowy.rb_nr)4 - numer sprawy (Case number - sprawa.sp_numer)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)21 - dowolna (jedna) ze spraw windykacyjnych powiązana z zewnętrznym id wierzytelnosci (wierzytelnosc.wi_ext_id)22 - dowolna (jedna) ze spraw windykacyjnych powiązana z zewnętrznym id dokumentu (dokumentu .do_ext_id) | Brak |

```json
{
	"importId": "00000000-0000-0000-0000-000000000000",
	"queueName": "Payment",
    "message": "
    {
        \"Objects\": [
        {
			\"Amount\": 66.66,
			\"CurrencyId\": 1,
			\"ExchangeRate\": 1,
			\"PaymentDate\": \"2025-02-15T00:00:00\",
			\"SourceAccountNumber\": \"12345678901234567890123456\",
			\"DestinationAccountNumber\": \"65432109876543210987654321\",
			\"Payer\": \"Jan Kowalski\",
			\"Title\": \"Opłata za usługi\",
			\"Address\": \"Warszawska 1 Warszawa\",
			\"ExternalId\": \"ID_123\",
			\"OnlyInformational\": false,	
			\"MatchKeyType\": 6,
			\"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
			\"MatchKey\": \"Case_MK_1\",
			\"SnapshotDate\": 
			{
				\"Date\": \"2025-02-15T00:00:00\"
			},
            \"PaymentTypeId\": 15
        }
	  ],
      \"ObjectsUpdateBehaviour\": 6
    }"
}

```
