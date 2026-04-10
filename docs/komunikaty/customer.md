---
title: "Customer"
---

**Opis:** Komunikat dodaje dłużnika z atrybutami dłużnika

**Lokalizacja w bazie integracyjnej:** dm\_messages.debtor\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | dluznik.dl_ext_id (dla MatchKeyTypeID = 2) |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:2 - zewnętrzny numer dłużnika (External customer number dluznik.dl_ext_id)6 - External case number - sprawa.sp_ext_id3 - External contract number wierzytelnosc.wi_ext_id | Brak |

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "Customer",
    "message": "
    {
      \"Objects\": [
        {
          \"DebtorPortfolioId\": 1,
          \"ExternalId\": \"EXT12345\",
          \"CustomerTypeId\": 1,
          \"FirstName\": \"John\",
          \"LastName\": \"Doe\",
          \"CompanyFullName\": \"John Doe Enterprises Ltd.\",
          \"CompanyShortName\": \"JDE Ltd.\",
          \"SexId\": 1,
          \"PESEL\": \"12345678901\",
          \"REGON\": \"987654321\",
          \"NIP\": \"123-45-67-890\",
          \"KRS\": \"0000123456\",
          \"FamilyName\": \"Doe\",
          \"Description\": \"A valued customer\",
          \"BirthDate\": \"1985-05-20T00:00:00\",
          \"DeathDate\": \"2070-08-15T00:00:00\",
          \"SecondName\": \"Michael\",
          \"MotherName\": \"Jane Doe\",
          \"FatherName\": \"Richard Doe\",
          \"BirthPlace\": \"New York\",
          \"CompanyStatusId\": 2,
          \"LiquidationDate\": \"2090-01-01T00:00:00\",
          \"BankruptcyDate\": \"2085-06-10T00:00:00\",
          \"CountryId\": 1,
          \"Death\": true,
          \"Bankruptcy\": true,
          \"SourceId\": 5,
          \"AttributesList\": {
            \"Objects\": [
              {
                \"Value\": \"test\",
                \"TypeId\": 2,
                \"ExternalId\": null
			  }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
          \"EmailList\": {
            \"Objects\": [
              {
                \"Email\": \"test\",
                \"SourceId\": 1,
                \"EmailTypeId\": 1,
                \"ExternalId\": \"1\",
                \"AdditionalInformation\": \"test\"
              }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
          \"PhoneNumberList\": {
            \"Objects\": [
              {
                \"PhoneNumber\": \"test\",
                \"SourceId\": 1,
                \"Comment\": \"test\",
                \"PhoneNumberTypeId\": 1,
                \"ExternalId\": \"1\"
              }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
          \"AddressList\": {
            \"Objects\": [
              {
                \"AddressTypeId\": 12,
                \"SourceId\": 1,
                \"Country\": \"Polska\",
                \"Street\": \"test\",
                \"HouseNumber\": \"1\",
                \"ApartmentNumber\": \"1\",
                \"PostalCode\": \"01-111\",
                \"City\": \"Warszawa\",
                \"PostalOffice\": \"test\",
                \"AdditionalInformation\": \"test\",
                \"RegionId\": 1,
                \"ExternalId\": \"1\"
              }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
          \"IdentityDocumentList\": {
            \"Objects\": [
              {
                \"DocumentTypeId\": 1,
                \"Series\": \"test\",
                \"Number\": \"test\",
                \"IssueDate\": \"2025-05-08T14:56:07.1824282Z\",
                \"ExpiryDate\": \"2025-05-08T14:56:07.1824282Z\",
                \"IssuingAuthority\": \"test\",
                \"SourceId\": 1
              }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
          \"CustomerPropertyList\": {
            \"Objects\": [
              {
                \"TypeId\": 3,
                \"SubtypeId\": 11,
                \"ValidFrom\": \"1985-05-20T00:00:00\",
                \"ValidTo\": \"2070-08-15T00:00:00\"
              },
              {
                \"TypeId\": 3,
                \"SubtypeId\": 12,
                \"ValidFrom\": \"1985-05-20T00:00:00\",
                \"ValidTo\": \"2070-08-15T00:00:00\"
              }
            ],
            \"ObjectsUpdateBehaviour\": 6
          },
		  \"MatchKeyType\": 2,
		  \"ObjectId\": \"00000000-0000-0000-0000-000000000000\",
		  \"MatchKey\": \"Customer_MK_1\"
		}
      ],
      \"ObjectsUpdateBehaviour\": 6
    }"
}

```
