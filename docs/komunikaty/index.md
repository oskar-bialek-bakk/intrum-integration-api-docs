---
title: "Komunikaty"
---

Dane do systemu DEBT Manager importowane są za pomocą komunikatów. Komunikaty mogą być przekazywane do API w dwóch trybach:

-   **Samodzielne komunikat** - w dowolnym momencie można zasilić API komunikatem, który zmodyfikuje wybrane dane w systemie np. system zewnętrzny przekazuje do DM pojedyncze wpłaty (brak jednego pliku ze wszystkimi wpłatami) poprzez przekazanie do API pojedynczego komunikatu typu Payment
-   **Pliki importowe, dzielone na komunikaty** - tryb importowania plików polega na przyjęciu przez API pliku danego typu importu, a następnie walidacji poprawności danych w pliku i podzieleniu danych z pliku na komunikaty, którymi zasilane jest API



!!! info "Przykład importu danych z pliku"
    Przykład importu danych z pliku

    Zakładamy,  że do systemu importujemy plik Excel o poniższej strukturze:

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon (telefony po przecinku) | numer rachunku bankowego | Obsługa Od | Obsługa Do | data wystawienia | data wymagalności | data naliczania odsetek | kwota wymagana |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 9974 5006 4166 7054 | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 73051 | 250,12 |

    Import polega na wykonaniu dwóch kroków:

    1) Walidacja danych i wykazanie błędów pliku importowego np.

    -   Niepoprawne rozszerzenie pliku importowgo
    -   Import pliku o takiej samej nazwie jak juz poprzednio zaimporotwany plik
    -   Import osoby, która ma podane imię i nazwisko ale brak peselu
    -   Import wpłaty bez podania daty wpłaty itd.

    2) Podział danych z pliku na komunikaty, które następnie wysyłane są do API

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon (telefony po przecinku) | numer rachunku bankowego | Obsługa Od | Obsługa Do | data wystawienia | data wymagalności | data naliczania odsetek | kwota wymagana |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 9974 5006 4166 7054 | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 2019-03-02 | 250,12 |
| Customer | Case | Contract |  |  |  |  |  |  |  |  |  |

**Budowa komunikatów**

**Każdy komunikat, składa się z dwóch typów pól:**

-   Pola specyficzne dla komunikatu, które przenoszą właściwe dane importowane do systemu np. komunikat Payment zawiera pole z kwotą wpłaty, a komunikat Customer zawiera pole z numerami PESEL, NIP, REGON itd.
-   Pola współdzielone, takie same dla wszystkich komunikatów, które sterują mechanizmem API. Poniżej lista wspólnych pól współdzielone przez mechanizm API:

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS |
| --- | --- | --- | --- |
| ObjectId | string | Tak | Pole pozwalające zidentyfikować status przetwarzania danego komunikatu w ramach API. Wyróżniamy dwa typowe przypadki:przekazywany w ramach komunikatu obiekt posiada ID w systemie zewnętrznym z którego zasilane są dane do systemu DM, więc w polu ObjectId używamy tego ID. Np. w systemie zewnętrznym klient posiada ID 533przekazywany w ramach komunikatu obiekt nie posiada ID w systemie zewnętrznym, w takim wypadku sami generuje dla tego obiektu GUIDPo przekazaniu komunikatu do API pole ObjectId pozwala nam później zidentyfikować jaki jest aktualny stan przetwarzania danych, które zostały przekazane w ramach komunikatu. |
| ToDoAt | Datetime | Nie | Pole mówiące o tym kiedy API powinno przetworzyć dany komunikat. W przypadku braku podania pola system przyjmuje, że jest to data bieżąca. Pole pozwala opóźnić przetwarzanie wybranych danych. |
| MatchKey | string | Tak | Klucz wyszukiwania pod który obiekt system DM należy podpiąć przekazane w komunikacie dane. Pole działa w powiązaniu z MatchKeyType |
| MatchKeyType | int | Tak | Typ klucza wyszukiwania, określa po jakim polu w systemie API integracyjne wyszukuje wartości z pola MatchKey. Bazowy słownik przechowywany jest w tabeli [dm_config].[match_key_type] i zawiera następujące wartości:1 - rachunek bankowy do spłaty (Bank Account - rachunek_bankowy.rb_nr)2 - zewnętrzny numer klienta (External customer number dluznik.dl_ext_id)3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id)4 - numer sprawy (Case number - sprawa.sp_numer)5 - zewnętrzny numer dokumentu (External document number - dokument.do_ext_id)6 - zewnętrzny numer sprawy (External case number sprawa.sp_ext_id)Słownik może być rozszerzany w ramach życia systemu. Zmiany w słowniku wymagają zmiany w metodach wyszukiwania (procedury SQL "_find" dla każdego typu obiektu np. dm_messages.customer_find). |

!!! info "Przykład użycia pól MatchKey i MatchKeyType"
    Przykład użycia pól MatchKey i MatchKeyType

    Zakładamy, że do systemu importujemy plik o poniższej strukturze podzielony na następujące typy komunikatów:

    | Nazwa | PESEL | Zew nr dłużnika | rodzaj dłużnika | telefon (telefony po przecinku) | numer rachunku bankowego | Obsługa Od | Obsługa Do | data wystawienia | data wymagalności | data naliczania odsetek | kwota wymagana |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Jan Kowalski | 83041100112 | 774 | os.fiz. | 622333111 | 01 1240 4416 9974 5006 4166 7054 | 2024-01-01 | 2024-03-30 | 2018-12-01 | 2019-03-01 | 2019-03-02 | 250,12 |
| Customer | Case | Contract |  |  |  |  |  |  |  |  |  |

    Dla każdego typu komunikatu musimy wybrać z pól przekazanych w pliku MatchKey i MatchKeyType, tak aby API mogło wyszukać czy dane znajdujące się w wybranym komunikacie istnieją już w systemie (jest to szczególnie istotne w przypadku importów aktualizujących). Przykładowo dla powyższego typu pliku moglibyśmy wskazać następujące pola:

    -   Komunikat Customer:
        -   MatchKey: pole "Zew nr dłużnika" (bo to pole pozwala nam zidentyfikować unikalnego dłużnika w ramach pliku)
        -   MatchKeyType: 2 - zewnętrzny numer klienta (External customer number)
    -   Komunikat Case:
        -   MatchKey: pole "numer rachunku bankowego" (bo to pole pozwala nam zidentyfikować unikalną sprawę w ramach pliku)
        -   MatchKeyType: 1  - rachunek bankowy do spłaty (Bank Account)
    -   Komunikat Contract:
        -   MatchKey: pole "numer rachunku bankowego" ( bo wiemy, że w przykładowym pliku importowym zawsze pod jeden numer rachunku bankowego będzie podpięta jedna wierzytelność, więc pole "numer rachunku bankowego" wyznaczy nam unikalną wierzytelność)
        -   MatchKeyType: 1  - rachunek bankowy do spłaty (Bank Account)

    W przypadku gdy istnieje ryzyko, ze dany numer może się powtarzać pomiędzy portfelami np. zewnętrzny numer dłużnika 774 może być użyty zarówno w portfelu kontrahenta X i kontrahenta Y, warto pomyśleć o rozszerzeniu MatchKey o numer klienta lub numer portfela np. 774\_12\_333 gdzie (12 to id kontrahenta w systemie DM (ko\_id), a 333 to id portfela w systemie DM (uko\_id).



Interpretacje i możliwość używania flag aktualizacyjnch obiektów.

|  | Interpretacja Flag |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Obiekt | Add (1) | AddOrUpdate (2) | Update (3) | Replace (4) | Ommit (5) | AddOrOmmit (6) | Append (7) |
| Action | L1 | Niedozwolona | Niedozwolona | Niedozwolona | Niedozwolona | L5 | Niedozwolona |
| Action → AttachedFiles | L1 | Niedozwolona | Niedozwolona | Niedozwolona | L4 | L5 | Niedozwolona |
| Action → AttributesList | L1 | L2 | L3 | L11 | L4 | L5 | Niedozwolona |
| Adjustment | L1 | Niedozwolona | Niedozwolona | Niedozwolona | L4 | L5 | Niedozwolona |
| Case | L1 | Niedozwolona | Niedozwolona | Niedozwolona | L4 | L5 | Niedozwolona |
| Case → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Case → CustomerContractList | L1 | Niedozwolona | Niedozwolona | Niedozwolona | L4 | L5 | Niedozwolona |
| Contract | L1 | L2 | L3 | Niedozwolona | L4 | L5 | Niedozwolona |
| Contract → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Contract → DocumentsList | L1 | L2 | L3 | L11 | L4 | L5 | Niedozwolona |
| Contract → DocumentsList → BookingsList | L1 | L2 (*) | L3 (*) | L11 (*) | L4 | L5 | Niedozwolona |
| Customer | L1 | L2 | L3 | Niedozwolona | L4 | L5 | Niedozwolona |
| Customer → AddressList | L1 | L8 | L9 | L10 | L4 | L5 | Niedozwolona |
| Customer → CustomerPropertyList Customer → AddressList → AddressPropertyList Customer → EmailList → MailPropertyList Customer → PhoneNumberList → PhonePropertyList | L1 | L2 | L3 | L10 | L4 | L5 | Niedozwolona |
| Customer → EmailList | L1 | L8 | L9 | L10 | L4 | L5 | Niedozwolona |
| Customer → IdentityDocumentList | L1 | L6 | L7 | L10 | L4 | L5 | Niedozwolona |
| Customer → PhoneNumberList | L1 | L8 | L9 | L10 | L4 | L5 | Niedozwolona |
| Debtor → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Document → AttributeList | L1 | L2 | L3 | L11 | L4 | L5 | L12 |
| Payment | L1 | Niedozwolona | Niedozwolona | Niedozwolona | L4 | L5 | Niedozwolona |
| Signature | L1 | L8 | L9 | L10 | L4 | L5 | Niedozwolona |
| UpdateField | Niedozwolona | Niedozwolona | L3 | Niedozwolona | Niedozwolona | Niedozwolona | Niedozwolona |
| ContractBalance | Niedozwolona | L2 | Niedozwolona | Niedozwolona | Niedozwolona | Niedozwolona | Niedozwolona |

Legenda:

1.  Dodanie nowego obiektu (operacja bazodanowa insert) jesli nie istnieje. Zwrócenie błedu jeśli obiekt (po match key) istnieje.
2.  Dodanie nowego obiektu lub aktualizacja jeśli w bazie danych został znaleziony obiekt po match key (operacja bazodanowa merge).
3.  Aktualizacja obiektu jeśli istnieje w bazie danych (operacja bazodanowa update), zwrócenie błedu jeśli nie został znaleziony obiekt.
4.  Pominięcie aktualizacji obiektu - potrzebne w przypadku aktualizacji obiektów na głębszym poziomie zagnieżdżenia.
5.  Dodanie nowego obiektu (operacja bazodanowa insert) jesli nie istnieje. Pominięcie jeśli obiekt istnieje.
6.  Dezaktywacja istniejącego obiektu jeśli istnieje (poprzez ustawienie "daty ważności"). Dodanie nowego obiektu (operacja bazodanowa insert)
7.  Dezaktywacja istniejącego obiektu jeśli istnieje (poprzez ustawienie "daty ważności"). Zwrócenie błedu jeśli obiekt nie istnieje. Dodanie nowego obiektu (operacja bazodanowa insert)
8.  Dezaktywacja istniejącego wszystkich obiektów aktywnych obiektów danego typu jeśli istnieją (poprzez ustawienie "daty ważności"). Dodanie nowego obiektu (operacja bazodanowa insert)
9.  Dezaktywacja istniejącego wszystkich obiektów aktywnych obiektów danego typu jeśli istnieją (poprzez ustawienie "daty ważności"). Zwrócenie błędu jeśli nie istnieje poprzedni obiekt (sprawdzane po Match key). Dodanie nowego obiektu (operacja bazodanowa insert)
10.  Dezaktywuje wszystkie istniejące obiekty powiązane z obiektem nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe obiekty. Aktualizuje obiekty istniejące (sprawdzane po match key)
11.  Usuwa wszystkie istniejące obiekty powiązane z obiektem nadrzędnym, które nie istnieją w komunikacie. Dodaje obiekty z listy. Aktualizuje obiekty istniejące (sprawdzane po match key).
12.  Dodaje obiekt jeśli nie istnieje. Jeśli obiekt nie istnieje to dopisuje wartość do istniejącej - wartości rozdzielone są średnikiem (;).

(\*) - Dozwolone tylko dla Partnerów biznesowych, dla których to partner biznesowy księguje operacje finansowe a Debt Manager jest aktualizowany tylko aktualnym saldem.
