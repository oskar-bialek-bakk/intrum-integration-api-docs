---
title: "ContractBalance"
---

**Opis:**

Dla wypełnionej flagi **IncludeContractBalance** = **true**

Komunikat umożliwia aktualizację stanu finansowego wierzytelności poprzez nadpisanie dokumentów pojedyńczą wartością salda. Zachowanie komunikatu zależne od flagi „Czy zaniechaj”
Jeśli flaga wypełniona na tak to:
1\. Zostaje dodane(ew.nadpisane jeśli wcześniej już jakieś było) księgowanie na wskazanym dokumencie księgowym, na wskazanym koncie księgowym – kwota i waluta zgodna z przekazaniem
2\. Zaległości aktualne z pozostałych dokumentów tej wierzytelności są przeksięgowywane na typ Umorzenie/zaniechanie
W efekcie aktualna kwota do windykacji takiej wierzytelności będzie zgodna ze wskazaniem nowo otrzymanej kwoty

Jeśli flaga „Czy zaniechaj” nie jest wypełniona na tak wówczas:
1\. Zostaje dodany (ew.nadpisany) nowy dokument wskazanego typu wraz z księgowaniem nowej kwoty.
2\. Pozostałe dokumenty wraz z księgowaniami na wierzytelności pozostają niezmienne.
W efekcie po takiej operacji aktualna kwota do windykacji będzie sumą otrzymanej kwoty z kwotą do windykacji sprzed operacji.

Dla wypełnionej flagi **CloseOpenedCases** = **true**

Drugim możliwym użyciem konunikatu jest zamknięcie spraw windykacyjnych powiązanych. Zamknięcie jest uzależnone od wartości flagi IncludeContractBalance. Jest to flaga mówiąca o tym, czy należy zamknąć aktualnie otwarte sprawy windykacyjne powiązane przez Match Key.
1\. Ustawiona na true zamykałaby wszystkie otwarte sprawy windykacyjne powiązane ze zmienianą wierzytelnością.
2\. Ustawiona na false nie zmienia stanu spraw (nie otwiera zamkniętych spraw).



**Lokalizacja w bazie integracyjnej:** dm\_messages.contractbalance\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACJA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | wierzytelnosc.wi_ext_id |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id) | Brak |

Przykład 1 (zmiana salda)

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "ContractBalance",
    "message": "
    {
        \"Objects\": [
            {
                \"AccountTypeId\": 2,
                \"DocumentTypeId\": 103,
                \"CurrencyId\": 2,
                \"ExchangeRate\": 4.13,
                \"Amount\": 343.33,
                \"Discontinue\": true,
                \"MatchKey\": \"3_69485987\",
                \"MatchKeyType\": 3,
                \"Id\": \"0c08cd99-c413-425b-88fd-3cd87d899df4\",
                \"ObjectId\": \"10000000-0000-0000-0000-000000000000\",
                \"ObjectName\": \"ObjectId 10000000-0000-0000-0000-000000000000\",
                \"ToDoAt\": \"2025-08-18T10:18:49.9219838Z\",
				\"CloseOpenedCases\": false,
				\"IncludeContractBalance\": true
            }
        ],
        \"ObjectsUpdateBehaviour\": 2
    }"
}

```



Przykład 2 (zamknięcie spraw)

```json
{
    "importId": "00000000-0000-0000-0000-000000000000",
    "queueName": "ContractBalance",
    "message": "
    {
        \"Objects\": [
            {
                \"MatchKey\": \"3_69485987\",
                \"MatchKeyType\": 3,
                \"Id\": \"0c08cd99-c413-425b-88fd-3cd87d899df4\",
                \"ObjectId\": \"10000000-0000-0000-0000-000000000000\",
                \"ObjectName\": \"ObjectId 10000000-0000-0000-0000-000000000000\",
                \"ToDoAt\": \"2025-08-18T10:18:49.9219838Z\",
				\"CloseOpenedCases\": true,
				\"IncludeContractBalance\": false
            }
        ],
        \"ObjectsUpdateBehaviour\": 2
    }"
}

```
