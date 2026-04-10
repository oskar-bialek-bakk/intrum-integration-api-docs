---
title: "Komunikaty"
---

Dane do systemu DEBT Manager importowane są za pomocą komunikatów. Komunikaty mogą być przekazywane do API w dwóch trybach:

-   **Samodzielne komunikat** - w dowolnym momencie można zasilić API komunikatem, który zmodyfikuje wybrane dane w systemie np. system zewnętrzny przekazuje do DM pojedyncze wpłaty (brak jednego pliku ze wszystkimi wpłatami) poprzez przekazanie do API pojedynczego komunikatu typu Payment
-   **Pliki importowe, dzielone na komunikaty** - tryb importowania plików polega na przyjęciu przez API pliku danego typu importu, a następnie walidacji poprawności danych w pliku i podzieleniu danych z pliku na komunikaty, którymi zasilane jest API

!!! info "Przykład importu danych z pliku"

    Zakładamy, że do systemu importujemy plik Excel o poniższej strukturze:

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon | nr rach. bankowego | Obsługa Od | Obsługa Do | data wyst. | data wymag. | data nal. odsetek | kwota wymagana |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 (...) | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 73051 | 250,12 |

    Import polega na wykonaniu dwóch kroków:

    1) Walidacja danych i wykazanie błędów pliku importowego np.

    -   Niepoprawne rozszerzenie pliku importowgo
    -   Import pliku o takiej samej nazwie jak juz poprzednio zaimporotwany plik
    -   Import osoby, która ma podane imię i nazwisko ale brak peselu
    -   Import wpłaty bez podania daty wpłaty itd.

    2) Podział danych z pliku na komunikaty, które następnie wysyłane są do API

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon | nr rach. bankowego | Obsługa Od | Obsługa Do | data wyst. | data wymag. | data nal. odsetek | kwota wymagana |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 (...) | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 2019-03-02 | 250,12 |
    | Customer | Case | Contract |  |  |  |  |  |  |  |  |  |

## Budowa komunikatów

Każdy komunikat składa się z dwóch typów pól:

-   Pola specyficzne dla komunikatu, które przenoszą właściwe dane importowane do systemu np. komunikat Payment zawiera pole z kwotą wpłaty, a komunikat Customer zawiera pole z numerami PESEL, NIP, REGON itd.
-   Pola współdzielone, takie same dla wszystkich komunikatów, które sterują mechanizmem API. Poniżej lista wspólnych pól:

| Pole | Typ | Wymagane | Opis |
| --- | --- | --- | --- |
| ObjectId | string | Tak | Pole pozwalające zidentyfikować status przetwarzania danego komunikatu w ramach API. Dwa typowe przypadki: (1) obiekt posiada ID w systemie zewnętrznym — używamy tego ID; (2) obiekt nie posiada ID — generujemy GUID. Po przekazaniu komunikatu pole ObjectId pozwala zidentyfikować aktualny stan przetwarzania danych. |
| ToDoAt | Datetime | Nie | Kiedy API powinno przetworzyć komunikat. Brak podania = data bieżąca. Pozwala opóźnić przetwarzanie. |
| MatchKey | string | Tak | Klucz wyszukiwania pod który obiekt w DM należy podpiąć dane z komunikatu. Działa w powiązaniu z MatchKeyType. |
| MatchKeyType | int | Tak | Typ klucza wyszukiwania — określa po jakim polu API wyszukuje wartości z MatchKey. Słownik w tabeli `[dm_config].[match_key_type]` — patrz tabela poniżej. |

**Wartości MatchKeyType:**

| ID | Opis | Pole w DM |
| --- | --- | --- |
| 1 | Rachunek bankowy do spłaty (Bank Account) | rachunek\_bankowy.rb\_nr |
| 2 | Zewnętrzny numer klienta (External customer number) | dluznik.dl\_ext\_id |
| 3 | Zewnętrzny numer umowy (External contract number) | wierzytelnosc.wi\_ext\_id |
| 4 | Numer sprawy (Case number) | sprawa.sp\_numer |
| 5 | Zewnętrzny numer dokumentu (External document number) | dokument.do\_ext\_id |
| 6 | Zewnętrzny numer sprawy (External case number) | sprawa.sp\_ext\_id |

Słownik może być rozszerzany. Zmiany wymagają modyfikacji procedur SQL `_find` dla każdego typu obiektu (np. `dm_messages.customer_find`).

!!! info "Przykład użycia pól MatchKey i MatchKeyType"

    Zakładamy, że importujemy plik podzielony na następujące typy komunikatów:

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon | nr rach. bankowego | Obsługa Od | Obsługa Do | data wyst. | data wymag. | data nal. odsetek | kwota wymagana |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 (...) | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 2019-03-02 | 250,12 |
    | Customer | Case | Contract |  |  |  |  |  |  |  |  |  |

    Dla każdego typu komunikatu musimy wybrać MatchKey i MatchKeyType:

    -   **Customer:** MatchKey = "Zew nr dłużnika", MatchKeyType = 2 (External customer number)
    -   **Case:** MatchKey = "numer rachunku bankowego", MatchKeyType = 1 (Bank Account)
    -   **Contract:** MatchKey = "numer rachunku bankowego", MatchKeyType = 1 (Bank Account)

    Gdy numer może się powtarzać pomiędzy portfelami, warto rozszerzyć MatchKey o id kontrahenta lub portfela, np. `774_12_333` (12 = ko\_id, 333 = uko\_id).

## Flagi aktualizacyjne obiektów (ObjectsUpdateBehaviour)

| Obiekt | Add (1) | AddOrUpdate (2) | Update (3) | Replace (4) | Ommit (5) | AddOrOmmit (6) | Append (7) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Action | L1 | — | — | — | — | L5 | — |
| Action → AttachedFiles | L1 | — | — | — | L4 | L5 | — |
| Action → AttributesList | L1 | L2 | L3 | L11 | L4 | L5 | — |
| Adjustment | L1 | — | — | — | L4 | L5 | — |
| Case | L1 | — | — | — | L4 | L5 | — |
| Case → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Case → CustomerContractList | L1 | — | — | — | L4 | L5 | — |
| Contract | L1 | L2 | L3 | — | L4 | L5 | — |
| Contract → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Contract → DocumentsList | L1 | L2 | L3 | L11 | L4 | L5 | — |
| Contract → Docs → BookingsList | L1 | L2 (*) | L3 (*) | L11 (*) | L4 | L5 | — |
| Customer | L1 | L2 | L3 | — | L4 | L5 | — |
| Customer → AddressList | L1 | L8 | L9 | L10 | L4 | L5 | — |
| Customer → PropertyList | L1 | L2 | L3 | L10 | L4 | L5 | — |
| Customer → EmailList | L1 | L8 | L9 | L10 | L4 | L5 | — |
| Customer → IdentityDocumentList | L1 | L6 | L7 | L10 | L4 | L5 | — |
| Customer → PhoneNumberList | L1 | L8 | L9 | L10 | L4 | L5 | — |
| Debtor → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Document → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Payment | L1 | — | — | — | L4 | L5 | — |
| Signature | L1 | L8 | L9 | L10 | L4 | L5 | — |
| UpdateField | — | — | L3 | — | — | — | — |
| ContractBalance | — | L2 | — | — | — | — | — |

(*) — Dozwolone tylko dla Partnerów biznesowych, dla których partner księguje operacje finansowe, a DEBT Manager jest aktualizowany tylko aktualnym saldem.

**Legenda:**

| Kod | Opis |
| --- | --- |
| L1 | Dodanie nowego obiektu (INSERT) jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje. |
| L2 | Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key (MERGE). |
| L3 | Aktualizacja obiektu jeśli istnieje (UPDATE). Błąd jeśli nie znaleziony. |
| L4 | Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia. |
| L5 | Dodanie (INSERT) jeśli nie istnieje. Pominięcie jeśli istnieje. |
| L6 | Dezaktywacja istniejącego obiektu (ustawienie "daty ważności"). Dodanie nowego (INSERT). |
| L7 | Dezaktywacja istniejącego obiektu. Błąd jeśli nie istnieje. Dodanie nowego (INSERT). |
| L8 | Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego (INSERT). |
| L9 | Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni (po match key). Dodanie nowego (INSERT). |
| L10 | Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key). |
| L11 | Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key). |
| L12 | Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: `;`). |
