---
title: "Komunikaty"
---

Dane do systemu DEBT Manager importowane są za pomocą komunikatów. Komunikaty mogą być przekazywane do API w dwóch trybach:

-   **Samodzielne komunikat** - w dowolnym momencie można zasilić API komunikatem, który zmodyfikuje wybrane dane w systemie np. system zewnętrzny przekazuje do DM pojedyncze wpłaty (brak jednego pliku ze wszystkimi wpłatami) poprzez przekazanie do API pojedynczego komunikatu typu Payment
-   **Pliki importowe, dzielone na komunikaty** - tryb importowania plików polega na przyjęciu przez API pliku danego typu importu, a następnie walidacji poprawności danych w pliku i podzieleniu danych z pliku na komunikaty, którymi zasilane jest API

!!! info "Przykład importu danych z pliku"

    Zakładamy, że do systemu importujemy plik Excel o poniższej strukturze:

    <table class="import-example-table">
    <tr>
      <th>Nazwa</th><th>PESEL</th><th>Zew nr dłużnika</th><th>rodzaj dłużnika</th><th>telefon</th>
      <th>nr rach. bankowego</th><th>Obsługa Od</th><th>Obsługa Do</th>
      <th>data wyst.</th><th>data wymag.</th><th>data nal. odsetek</th><th>kwota wymagana</th>
    </tr>
    <tr>
      <td>Jan Kowalski</td><td>83041100112</td><td>774</td><td>os.fiz.</td><td>622333111</td>
      <td>01 1240 4416 (...)</td><td>2024-01-01</td><td>2024-03-30</td>
      <td>2018-12-01</td><td>2019-03-01</td><td>73051</td><td>250,12</td>
    </tr>
    </table>

    Import polega na wykonaniu dwóch kroków:

    1) Walidacja danych i wykazanie błędów pliku importowego np.

    -   Niepoprawne rozszerzenie pliku importowego
    -   Import pliku o takiej samej nazwie jak już poprzednio zaimportowany plik
    -   Import osoby, która ma podane imię i nazwisko ale brak PESEL-u
    -   Import wpłaty bez podania daty wpłaty itd.

    2) Podział danych z pliku na komunikaty, które następnie wysyłane są do API

    <table class="import-example-table">
    <tr>
      <th>Nazwa</th><th>PESEL</th><th>Zew nr dłużnika</th><th>rodzaj dłużnika</th><th>telefon</th>
      <th>nr rach. bankowego</th><th>Obsługa Od</th><th>Obsługa Do</th>
      <th>data wyst.</th><th>data wymag.</th><th>data nal. odsetek</th><th>kwota wymagana</th>
    </tr>
    <tr>
      <td class="band-customer">Jan Kowalski</td><td class="band-customer">83041100112</td><td class="band-customer">774</td><td class="band-customer">os.fiz.</td><td class="band-customer">622333111</td>
      <td class="band-case">01 1240 4416 (...)</td><td class="band-case">2024-01-01</td><td class="band-case">2024-03-30</td>
      <td class="band-contract">2018-12-01</td><td class="band-contract">2019-03-01</td><td class="band-contract">2019-03-02</td><td class="band-contract">250,12</td>
    </tr>
    <tr>
      <td class="band-customer band-label" colspan="5"><strong>Customer</strong></td>
      <td class="band-case band-label" colspan="3"><strong>Case</strong></td>
      <td class="band-contract band-label" colspan="4"><strong>Contract</strong></td>
    </tr>
    </table>

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

    <table class="import-example-table">
    <tr>
      <th>Nazwa</th><th>PESEL</th><th>Zew nr dłużnika</th><th>rodzaj dłużnika</th><th>telefon</th>
      <th>nr rach. bankowego</th><th>Obsługa Od</th><th>Obsługa Do</th>
      <th>data wyst.</th><th>data wymag.</th><th>data nal. odsetek</th><th>kwota wymagana</th>
    </tr>
    <tr>
      <td class="band-customer">Jan Kowalski</td><td class="band-customer">83041100112</td><td class="band-customer">774</td><td class="band-customer">os.fiz.</td><td class="band-customer">622333111</td>
      <td class="band-case">01 1240 4416 (...)</td><td class="band-case">2024-01-01</td><td class="band-case">2024-03-30</td>
      <td class="band-contract">2018-12-01</td><td class="band-contract">2019-03-01</td><td class="band-contract">2019-03-02</td><td class="band-contract">250,12</td>
    </tr>
    <tr>
      <td class="band-customer band-label" colspan="5"><strong>Customer</strong></td>
      <td class="band-case band-label" colspan="3"><strong>Case</strong></td>
      <td class="band-contract band-label" colspan="4"><strong>Contract</strong></td>
    </tr>
    </table>

    Dla każdego typu komunikatu musimy wybrać MatchKey i MatchKeyType:

    -   **Customer:** MatchKey = "Zew nr dłużnika", MatchKeyType = 2 (External customer number)
    -   **Case:** MatchKey = "numer rachunku bankowego", MatchKeyType = 1 (Bank Account)
    -   **Contract:** MatchKey = "numer rachunku bankowego", MatchKeyType = 1 (Bank Account)

    Gdy numer może się powtarzać pomiędzy portfelami, warto rozszerzyć MatchKey o id kontrahenta lub portfela, np. `774_12_333` (12 = ko\_id, 333 = uko\_id).

## Flagi aktualizacyjne obiektów (ObjectsUpdateBehaviour)

<div class="md-typeset__scrollwrap" markdown>
<table class="flag-matrix">
<thead>
<tr>
  <th>Obiekt</th>
  <th>Add (1)</th>
  <th>AddOrUpdate (2)</th>
  <th>Update (3)</th>
  <th>Replace (4)</th>
  <th>Ommit (5)</th>
  <th>AddOrOmmit (6)</th>
  <th>Append (7)</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Action</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Action → AttachedFiles</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Action → AttributesList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Adjustment</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Case</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Case → AttributeList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td><span class="flag append" title="Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: ;).">APPEND</span></td>
</tr>
<tr>
  <td>Case → CustomerContractList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Contract</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Contract → AttributeList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td><span class="flag append" title="Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: ;).">APPEND</span></td>
</tr>
<tr>
  <td>Contract → DocumentsList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Contract → Docs → BookingsList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key. (*) Tylko dla Partnerów biznesowych.">UPSERT *</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony. (*) Tylko dla Partnerów biznesowych.">UPDATE *</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące. (*) Tylko dla Partnerów biznesowych.">REPLACE *</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer → AddressList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag deact" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego.">DEACT_ALL+ADD</span></td>
  <td><span class="flag deact-err" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni. Dodanie nowego.">DEACT_ALL!+ADD</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer → PropertyList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer → EmailList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag deact" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego.">DEACT_ALL+ADD</span></td>
  <td><span class="flag deact-err" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni. Dodanie nowego.">DEACT_ALL!+ADD</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer → IdentityDocumentList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag deact" title="Dezaktywacja istniejącego obiektu (ustawienie daty ważności). Dodanie nowego.">DEACT+ADD</span></td>
  <td><span class="flag deact-err" title="Dezaktywacja istniejącego obiektu. Błąd jeśli nie istnieje. Dodanie nowego.">DEACT!+ADD</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Customer → PhoneNumberList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag deact" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego.">DEACT_ALL+ADD</span></td>
  <td><span class="flag deact-err" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni. Dodanie nowego.">DEACT_ALL!+ADD</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Debtor → AttributeList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td><span class="flag append" title="Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: ;).">APPEND</span></td>
</tr>
<tr>
  <td>Document → AttributeList</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td><span class="flag replace" title="Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">REPLACE</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td><span class="flag append" title="Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: ;).">APPEND</span></td>
</tr>
<tr>
  <td>Payment</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>Signature</td>
  <td><span class="flag add" title="Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje.">ADD</span></td>
  <td><span class="flag deact" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego.">DEACT_ALL+ADD</span></td>
  <td><span class="flag deact-err" title="Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni. Dodanie nowego.">DEACT_ALL!+ADD</span></td>
  <td><span class="flag sync" title="Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key).">SYNC</span></td>
  <td><span class="flag skip" title="Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia.">SKIP</span></td>
  <td><span class="flag add-skip" title="Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje.">ADD/SKIP</span></td>
  <td>—</td>
</tr>
<tr>
  <td>UpdateField</td>
  <td>—</td>
  <td>—</td>
  <td><span class="flag update" title="Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony.">UPDATE</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
</tr>
<tr>
  <td>ContractBalance</td>
  <td>—</td>
  <td><span class="flag upsert" title="Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key.">UPSERT</span></td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
  <td>—</td>
</tr>
</tbody>
</table>
</div>

(*) — Dozwolone tylko dla Partnerów biznesowych, dla których partner księguje operacje finansowe, a DEBT Manager jest aktualizowany tylko aktualnym saldem.

!!! tip "Tooltip"
    Najedź kursorem na kolorową etykietę, aby zobaczyć pełny opis zachowania.

**Legenda:**

| Etykieta | Opis |
| --- | --- |
| <span class="flag add">ADD</span> | Dodanie nowego obiektu jeśli nie istnieje. Błąd jeśli obiekt (po match key) istnieje. |
| <span class="flag upsert">UPSERT</span> | Dodanie nowego obiektu lub aktualizacja jeśli znaleziony po match key. |
| <span class="flag update">UPDATE</span> | Aktualizacja obiektu jeśli istnieje. Błąd jeśli nie znaleziony. |
| <span class="flag skip">SKIP</span> | Pominięcie aktualizacji — potrzebne przy aktualizacji obiektów na głębszym poziomie zagnieżdżenia. |
| <span class="flag add-skip">ADD/SKIP</span> | Dodanie jeśli nie istnieje. Pominięcie jeśli istnieje. |
| <span class="flag deact">DEACT+ADD</span> | Dezaktywacja istniejącego obiektu (ustawienie "daty ważności"). Dodanie nowego. |
| <span class="flag deact-err">DEACT!+ADD</span> | Dezaktywacja istniejącego obiektu. Błąd jeśli nie istnieje. Dodanie nowego. |
| <span class="flag deact">DEACT_ALL+ADD</span> | Dezaktywacja wszystkich aktywnych obiektów danego typu. Dodanie nowego. |
| <span class="flag deact-err">DEACT_ALL!+ADD</span> | Dezaktywacja wszystkich aktywnych obiektów danego typu. Błąd jeśli nie istnieje poprzedni (po match key). Dodanie nowego. |
| <span class="flag sync">SYNC</span> | Dezaktywuje obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key). |
| <span class="flag replace">REPLACE</span> | Usuwa obiekty powiązane z nadrzędnym, które nie istnieją w komunikacie. Dodaje nowe. Aktualizuje istniejące (po match key). |
| <span class="flag append">APPEND</span> | Dodaje jeśli nie istnieje. Jeśli istnieje — dopisuje wartość do istniejącej (separator: `;`). |
