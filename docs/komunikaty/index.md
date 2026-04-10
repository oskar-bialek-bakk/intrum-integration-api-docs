---
title: "Komunikaty"
---

<div class="api-section" markdown>
<div class="api-section-title">Tryby importu danych</div>

Dane do systemu DEBT Manager importowane są za pomocą komunikatów. Komunikaty mogą być przekazywane do API w dwóch trybach:

-   **Samodzielny komunikat** — w dowolnym momencie można zasilić API komunikatem, który zmodyfikuje wybrane dane w systemie, np. system zewnętrzny przekazuje do DM pojedyncze wpłaty poprzez przekazanie do API pojedynczego komunikatu typu Payment.
-   **Pliki importowe, dzielone na komunikaty** — tryb importowania plików polega na przyjęciu przez API pliku danego typu importu, a następnie walidacji poprawności danych w pliku i podzieleniu danych z pliku na komunikaty, którymi zasilane jest API.

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

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Budowa komunikatów</div>

Każdy komunikat składa się z dwóch typów pól:

-   **Pola specyficzne** dla komunikatu, które przenoszą właściwe dane importowane do systemu, np. komunikat Payment zawiera pole z kwotą wpłaty, a komunikat Customer zawiera pole z numerami PESEL, NIP, REGON itd.
-   **Pola współdzielone**, takie same dla wszystkich komunikatów, które sterują mechanizmem API:

<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Pole pozwalające zidentyfikować status przetwarzania danego komunikatu w ramach API. Dwa typowe przypadki: (1) obiekt posiada ID w systemie zewnętrznym — używamy tego ID; (2) obiekt nie posiada ID — generujemy GUID.</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Kiedy API powinno przetworzyć komunikat. Brak podania = data bieżąca. Pozwala opóźnić przetwarzanie.</span>
  </li>
  <li>
    <span class="param-name required">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania — pod który obiekt w DM należy podpiąć dane z komunikatu. Działa w powiązaniu z <code>MatchKeyType</code>.</span>
  </li>
  <li>
    <span class="param-name required">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania — określa po jakim polu API wyszukuje wartości z <code>MatchKey</code>. Słownik w tabeli <code>[dm_config].[match_key_type]</code>. Dopuszczalne wartości:</span>
    <ul class="mk-values">
      <li>
        <span class="mk-id">1</span>
        <span class="mk-label">Rachunek bankowy do spłaty (Bank Account)</span>
        <span class="mk-field">→ rachunek_bankowy.rb_nr</span>
      </li>
      <li>
        <span class="mk-id">2</span>
        <span class="mk-label">Zewnętrzny numer klienta (External customer number)</span>
        <span class="mk-field">→ dluznik.dl_ext_id</span>
      </li>
      <li>
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy (External contract number)</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">4</span>
        <span class="mk-label">Numer sprawy (Case number)</span>
        <span class="mk-field">→ sprawa.sp_numer</span>
      </li>
      <li>
        <span class="mk-id">5</span>
        <span class="mk-label">Zewnętrzny numer dokumentu (External document number)</span>
        <span class="mk-field">→ dokument.do_ext_id</span>
      </li>
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy (External case number)</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

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

</div>

---

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
