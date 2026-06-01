---
title: "Customer"
---

# Customer

<div class="msg-header">
  <span class="msg-type">Customer</span>
  <span class="msg-badge queue">kolejka: Customer</span>
  <span class="msg-badge db">dm_messages.debtor_details</span>
  <p class="msg-desc">Komunikat dodaje dłużnika z atrybutami, danymi kontaktowymi (adresy, telefony, e-mail), dokumentami tożsamości i właściwościami.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">Customer</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AttributesList</span> <span class="tree-type">lista atrybutów dłużnika</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">EmailList</span> <span class="tree-type">adresy e-mail</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">PhoneNumberList</span> <span class="tree-type">numery telefonów</span><br>
  <span class="tree-connector">│&nbsp;&nbsp;&nbsp;└── </span><span class="tree-leaf">PhonePropertyList</span> <span class="tree-type">właściwości telefonu</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AddressList</span> <span class="tree-type">adresy korespondencyjne / zameldowania</span><br>
  <span class="tree-connector">│&nbsp;&nbsp;&nbsp;└── </span><span class="tree-leaf">AddressPropertyList</span> <span class="tree-type">właściwości adresu</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">IdentityDocumentList</span> <span class="tree-type">dokumenty tożsamości</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">CustomerPropertyList</span> <span class="tree-type">właściwości dłużnika</span>
</div>

Każda lista zagnieżdżona ma własne pole `ObjectsUpdateBehaviour` — patrz [macierz flag aktualizacyjnych](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pola wspólne (MatchKey)</div>

<ul class="param-list">
  <li>
    <span class="param-name required">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania obiektu w DM. Patrz <a href="../#budowa-komunikatów">Budowa komunikatów</a>.</span>
  </li>
  <li>
    <span class="param-name required">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania. Dopuszczalne wartości:</span>
    <ul class="mk-values">
      <li>
        <span class="mk-id">2</span>
        <span class="mk-label">Zewnętrzny numer dłużnika</span>
        <span class="mk-field">→ dluznik.dl_ext_id</span>
      </li>
      <li>
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pola wspólne dla każdego obiektu w komunikacie</div>

Każdy obiekt — zarówno `Customer` na poziomie głównym, jak i wszystkie obiekty zagnieżdżone (`AttributesList[*]`, `PhoneNumberList[*]`, `AddressList[*]` itd.) — zawiera dodatkowo poniższy zestaw pól technicznych. Pola te służą do śledzenia statusu przetwarzania, kontroli wersji oraz sterowania zachowaniem aktualizacji.

<ul class="param-list">
  <li>
    <span class="param-name required">Id</span>
    <span class="param-type">GUID</span>
    <span class="param-desc">Identyfikator obiektu w ramach pojedynczego komunikatu. Wymagany, unikalny per obiekt.</span>
  </li>
  <li>
    <span class="param-name">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania (np. przez <a href="../../funkcje-api/importy/get-object-status-by-id/">GetObjectStatusByID</a>) — GUID albo ID z systemu zewnętrznego, patrz <a href="../#budowa-komunikatów">Budowa komunikatów</a>. Dla obiektu głównego <code>Customer</code> wymagany; dla obiektów zagnieżdżonych zazwyczaj <code>null</code> (rolę identyfikatora pełni <code>Id</code>).</span>
  </li>
  <li>
    <span class="param-name">VersionId</span>
    <span class="param-type">GUID</span>
    <span class="param-desc">Identyfikator wersji obiektu. Wypełniany w przypadku aktualizacji konkretnej wersji obiektu w DM; <code>null</code> przy operacji dodania.</span>
  </li>
  <li>
    <span class="param-name">OrderInMessage</span>
    <span class="param-type">int</span>
    <span class="param-desc">Kolejność obiektu w komunikacie. Dla obiektów zagnieżdżonych typowo <code>-2147483648</code> (brak kolejności). Dla obiektów głównych — kolejność wynikająca z pliku/źródła danych.</span>
  </li>
  <li>
    <span class="param-name required">ToDoAt</span>
    <span class="param-type">datetime (ISO 8601, UTC)</span>
    <span class="param-desc">Moment, od którego API może rozpocząć przetwarzanie obiektu. Format: <code>2025-11-18T06:28:57.4287002Z</code>.</span>
  </li>
  <li>
    <span class="param-name">QueueId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kolejki, do której trafia obiekt główny (dla <code>Customer</code> typowo <code>2</code>). Dla obiektów zagnieżdżonych <code>0</code>.</span>
  </li>
  <li>
    <span class="param-name">QueueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki (np. <code>"Customer"</code>). Dla obiektów zagnieżdżonych <code>null</code>.</span>
  </li>
  <li>
    <span class="param-name">Tag</span>
    <span class="param-type">string</span>
    <span class="param-desc">Pole dowolnego użytku — etykieta diagnostyczna lub korelacyjna. Najczęściej <code>null</code>.</span>
  </li>
  <li>
    <span class="param-name">ObjectName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Czytelna nazwa obiektu używana w logach API. Dla obiektu głównego: <code>"ObjectId &lt;guid&gt;"</code>. Dla obiektów zagnieżdżonych: <code>"-2147483648 object in message"</code>.</span>
  </li>
  <li>
    <span class="param-name required">UpdateBehaviour</span>
    <span class="param-type">int</span>
    <span class="param-desc">Flaga sterująca aktualizacją konkretnego obiektu (per-obiekt). Wartości jak w polu listowym <code>ObjectsUpdateBehaviour</code> — patrz <a href="../#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour">macierz flag</a>. Zwykle <code>6</code> (dodaj-lub-zaktualizuj) lub <code>3</code> (tylko aktualizacja, jeśli istnieje).</span>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pole na poziomie koperty komunikatu</div>

Wewnątrz pola `message` (po deserializacji) na najwyższym poziomie obowiązują dodatkowo:

<ul class="param-list">
  <li>
    <span class="param-name required">Objects</span>
    <span class="param-type">array</span>
    <span class="param-desc">Lista obiektów <code>Customer</code> do przetworzenia w ramach jednego komunikatu.</span>
  </li>
  <li>
    <span class="param-name required">ObjectsUpdateBehaviour</span>
    <span class="param-type">int</span>
    <span class="param-desc">Domyślna flaga aktualizacji stosowana, gdy obiekt z listy nie ma własnej (<code>UpdateBehaviour</code>).</span>
  </li>
  <li>
    <span class="param-name">ObjectsAddedUserId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID użytkownika DM, który rejestrowany jest jako autor wprowadzonych zmian (audyt). Typowo <code>5</code> = konto integracyjne.</span>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Customer</div>

<details class="collapsible-fields">
<summary>Customer — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name">DebtorPortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela dłużnika.</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator dłużnika w systemie klienta.</span>
  </li>
  <li>
    <span class="param-name">CustomerTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dłużnika (np. <code>1</code> — osoba fizyczna, <code>2</code> — firma).</span>
  </li>
  <li>
    <span class="param-name">FirstName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię.</span>
  </li>
  <li>
    <span class="param-name">LastName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwisko.</span>
  </li>
  <li>
    <span class="param-name">SecondName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Drugie imię.</span>
  </li>
  <li>
    <span class="param-name">CompanyFullName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Pełna nazwa firmy (dla osób prawnych).</span>
  </li>
  <li>
    <span class="param-name">CompanyShortName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Skrócona nazwa firmy.</span>
  </li>
  <li>
    <span class="param-name">SexId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Płeć (<code>1</code> — mężczyzna, <code>2</code> — kobieta).</span>
  </li>
  <li>
    <span class="param-name">PESEL</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer PESEL.</span>
  </li>
  <li>
    <span class="param-name">REGON</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer REGON.</span>
  </li>
  <li>
    <span class="param-name">NIP</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer NIP.</span>
  </li>
  <li>
    <span class="param-name">KRS</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer KRS.</span>
  </li>
  <li>
    <span class="param-name">FamilyName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwisko rodowe.</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opis / komentarz do dłużnika.</span>
  </li>
  <li>
    <span class="param-name">BirthDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data urodzenia.</span>
  </li>
  <li>
    <span class="param-name">DeathDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zgonu.</span>
  </li>
  <li>
    <span class="param-name">Death</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Flaga zgonu.</span>
  </li>
  <li>
    <span class="param-name">MotherName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię matki.</span>
  </li>
  <li>
    <span class="param-name">FatherName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię ojca.</span>
  </li>
  <li>
    <span class="param-name">BirthPlace</span>
    <span class="param-type">string</span>
    <span class="param-desc">Miejsce urodzenia.</span>
  </li>
  <li>
    <span class="param-name">CompanyStatusId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status firmy (dla osób prawnych).</span>
  </li>
  <li>
    <span class="param-name">LiquidationDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data likwidacji firmy.</span>
  </li>
  <li>
    <span class="param-name">BankruptcyDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data ogłoszenia upadłości.</span>
  </li>
  <li>
    <span class="param-name">Bankruptcy</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Flaga upadłości.</span>
  </li>
  <li>
    <span class="param-name">CountryId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kraju (słownik).</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych (słownik).</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AttributesList — atrybuty dłużnika</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu atrybutu (słownik).</span>
  </li>
  <li>
    <span class="param-name required">Value</span>
    <span class="param-type">string</span>
    <span class="param-desc">Wartość atrybutu.</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator atrybutu.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>EmailList — adresy e-mail</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">Email</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adres e-mail.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">EmailTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ adresu e-mail (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
  <li>
    <span class="param-name">AdditionalInformation</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowe informacje.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>PhoneNumberList — numery telefonów</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">PhoneNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer telefonu.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">Comment</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komentarz.</span>
  </li>
  <li>
    <span class="param-name">PhoneNumberTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ numeru telefonu (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
  <li>
    <span class="param-name">PhonePropertyList</span>
    <span class="param-type">obiekt z listą</span>
    <span class="param-desc">Lista właściwości telefonu (z własnym <code>ObjectsUpdateBehaviour</code>) — patrz sekcja niżej.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>PhonePropertyList — właściwości telefonu</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name required">SubtypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID podtypu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name">ValidFrom</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data początku obowiązywania właściwości.</span>
  </li>
  <li>
    <span class="param-name">ValidTo</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data końca obowiązywania właściwości (typowo <code>2100-01-01T00:00:00</code> dla otwartego okresu).</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AddressList — adresy</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">AddressTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ adresu (słownik).</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">Country</span>
    <span class="param-type">string</span>
    <span class="param-desc">Kraj.</span>
  </li>
  <li>
    <span class="param-name">Street</span>
    <span class="param-type">string</span>
    <span class="param-desc">Ulica.</span>
  </li>
  <li>
    <span class="param-name">HouseNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer domu.</span>
  </li>
  <li>
    <span class="param-name">ApartmentNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer mieszkania.</span>
  </li>
  <li>
    <span class="param-name">PostalCode</span>
    <span class="param-type">string</span>
    <span class="param-desc">Kod pocztowy.</span>
  </li>
  <li>
    <span class="param-name">City</span>
    <span class="param-type">string</span>
    <span class="param-desc">Miasto.</span>
  </li>
  <li>
    <span class="param-name">PostalOffice</span>
    <span class="param-type">string</span>
    <span class="param-desc">Poczta.</span>
  </li>
  <li>
    <span class="param-name">AdditionalInformation</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowe informacje.</span>
  </li>
  <li>
    <span class="param-name">RegionId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID regionu (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
  <li>
    <span class="param-name">AddressPropertyList</span>
    <span class="param-type">obiekt z listą</span>
    <span class="param-desc">Lista właściwości adresu (z własnym <code>ObjectsUpdateBehaviour</code>) — patrz sekcja niżej.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AddressPropertyList — właściwości adresu</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name required">SubtypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID podtypu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name">ValidFrom</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data początku obowiązywania.</span>
  </li>
  <li>
    <span class="param-name">ValidTo</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data końca obowiązywania (typowo <code>2100-01-01T00:00:00</code>).</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>IdentityDocumentList — dokumenty tożsamości</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">DocumentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dokumentu tożsamości (słownik).</span>
  </li>
  <li>
    <span class="param-name">Series</span>
    <span class="param-type">string</span>
    <span class="param-desc">Seria dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Number</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer dokumentu.</span>
  </li>
  <li>
    <span class="param-name">IssueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wydania.</span>
  </li>
  <li>
    <span class="param-name">ExpiryDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data ważności.</span>
  </li>
  <li>
    <span class="param-name">IssuingAuthority</span>
    <span class="param-type">string</span>
    <span class="param-desc">Organ wydający.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>CustomerPropertyList — właściwości dłużnika</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name required">SubtypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID podtypu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name">ValidFrom</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data początku obowiązywania.</span>
  </li>
  <li>
    <span class="param-name">ValidTo</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data końca obowiązywania.</span>
  </li>
</ul>
</details>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przykłady JSON</div>

Wszystkie przykłady poniżej to **rzeczywiste komunikaty** z konfiguracji testowej Intrum (`dmapi-intrum-dev`). Dane osobowe są zanonimizowane (losowe nazwy firm, numery NIP nie spełniają walidacji), telefony i adresy syntetyczne. Daty `ToDoAt` pochodzą z konkretnego runu pipeline z 2025-11-18.

=== "Pełny komunikat (2 dłużników)"

    Komunikat zasilający kolejkę `Customer` dwoma obiektami dłużnika — każdy z atrybutem, telefonami i adresem. Flaga `ObjectsUpdateBehaviour: 6` na poziomie listy = "dodaj-lub-zaktualizuj".

    ```json title="Koperta API (request body)"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Customer",
      "message": "{...}"
    }
    ```

    Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message (po deserializacji)"
    {
      "Objects": [
        {
          "MatchKey": "test_9_6_640536561",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "640536561",
          "CustomerTypeId": 1,
          "FirstName": null,
          "LastName": null,
          "CompanyFullName": "HUBYH CXTFH DTDDLV YOHUTEZOYZNB HUWPHO",
          "CompanyShortName": null,
          "SexId": 1,
          "PESEL": null,
          "REGON": null,
          "NIP": "EK4689703218",
          "KRS": null,
          "FamilyName": null,
          "Description": null,
          "BirthDate": null,
          "DeathDate": null,
          "SecondName": null,
          "MotherName": null,
          "FatherName": null,
          "BirthPlace": null,
          "CompanyStatusId": null,
          "LiquidationDate": null,
          "BankruptcyDate": null,
          "CountryId": null,
          "Death": null,
          "Bankruptcy": null,
          "SourceId": null,
          "AttributesList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "Value": "0",
                "TypeId": 7689,
                "ExternalId": null,
                "Id": "fe1b382e-96f3-4019-8a12-a68f367f30eb",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "EmailList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "PhoneNumberList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "PhoneNumber": "706581109",
                "SourceId": 2,
                "Comment": null,
                "PhoneNumberTypeId": 2,
                "ExternalId": null,
                "PhonePropertyList": {
                  "Objects": [
                    {
                      "MatchKey": null,
                      "MatchKeyType": 0,
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "837281aa-08ab-45a8-bd6a-71b81b233cb4",
                      "OrderInMessage": -2147483648,
                      "ObjectId": null,
                      "VersionId": null,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "QueueId": 0,
                      "QueueName": null,
                      "Tag": null,
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "f33bfd2c-b1d3-4fc1-a5fa-87f01b925208",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              },
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "PhoneNumber": "706304621",
                "SourceId": 2,
                "Comment": null,
                "PhoneNumberTypeId": 2,
                "ExternalId": null,
                "PhonePropertyList": {
                  "Objects": [
                    {
                      "MatchKey": null,
                      "MatchKeyType": 0,
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "c19a28f8-e3b9-40b8-882c-676639352353",
                      "OrderInMessage": -2147483648,
                      "ObjectId": null,
                      "VersionId": null,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "QueueId": 0,
                      "QueueName": null,
                      "Tag": null,
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "9152126e-cee5-44f2-af95-a6c34583b4ba",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "AddressList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "AddressTypeId": 1,
                "SourceId": 2,
                "Country": null,
                "Street": "DK.FLGZTZFSGNRJL 51",
                "HouseNumber": null,
                "ApartmentNumber": null,
                "PostalCode": "96-050",
                "City": "FHCVPVBOCJNFH",
                "PostalOffice": null,
                "AdditionalInformation": null,
                "RegionId": null,
                "ExternalId": null,
                "AddressPropertyList": {
                  "Objects": [
                    {
                      "MatchKey": null,
                      "MatchKeyType": 0,
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "4ad7a38c-00e1-450c-ba7f-2921c61f8540",
                      "OrderInMessage": -2147483648,
                      "ObjectId": null,
                      "VersionId": null,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "QueueId": 0,
                      "QueueName": null,
                      "Tag": null,
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "ed2e54bb-7ae9-47f5-aa9d-7826c24df353",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "IdentityDocumentList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "CustomerPropertyList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "Id": "b2ec4620-1765-42dc-9733-4561d3f35be1",
          "OrderInMessage": 92,
          "ObjectId": "5fb23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "VersionId": null,
          "ToDoAt": "2025-11-18T06:28:57.4287002Z",
          "QueueId": 2,
          "QueueName": "Customer",
          "Tag": null,
          "ObjectName": "ObjectId 5fb23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "UpdateBehaviour": 6
        },
        {
          "MatchKey": "test_9_6_640124357",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "640124357",
          "CustomerTypeId": 1,
          "FirstName": null,
          "LastName": null,
          "CompanyFullName": "KHTEZK LOCBLZOC DRVUBF",
          "CompanyShortName": null,
          "SexId": 1,
          "PESEL": null,
          "REGON": null,
          "NIP": "EK1956231624",
          "KRS": null,
          "FamilyName": null,
          "Description": null,
          "BirthDate": null,
          "DeathDate": null,
          "SecondName": null,
          "MotherName": null,
          "FatherName": null,
          "BirthPlace": null,
          "CompanyStatusId": null,
          "LiquidationDate": null,
          "BankruptcyDate": null,
          "CountryId": null,
          "Death": null,
          "Bankruptcy": null,
          "SourceId": null,
          "AttributesList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "Value": "0",
                "TypeId": 7689,
                "ExternalId": null,
                "Id": "4a7450d0-47a1-42e1-986b-863b405ac63f",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "EmailList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "PhoneNumberList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "PhoneNumber": "269593276",
                "SourceId": 2,
                "Comment": null,
                "PhoneNumberTypeId": 2,
                "ExternalId": null,
                "PhonePropertyList": {
                  "Objects": [
                    {
                      "MatchKey": null,
                      "MatchKeyType": 0,
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "f11019d0-d9c3-4533-95b7-fa0f49924e2e",
                      "OrderInMessage": -2147483648,
                      "ObjectId": null,
                      "VersionId": null,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "QueueId": 0,
                      "QueueName": null,
                      "Tag": null,
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "9f4bd359-ae27-4edb-a20c-3cd34a865129",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "AddressList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "AddressTypeId": 1,
                "SourceId": 2,
                "Country": null,
                "Street": "DK.PZFXGJL V 65",
                "HouseNumber": null,
                "ApartmentNumber": null,
                "PostalCode": "73-600",
                "City": "ŻHLHZ",
                "PostalOffice": null,
                "AdditionalInformation": null,
                "RegionId": null,
                "ExternalId": null,
                "AddressPropertyList": {
                  "Objects": [
                    {
                      "MatchKey": null,
                      "MatchKeyType": 0,
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "1d5f2a4b-105a-4f90-9c88-4aec35ee3000",
                      "OrderInMessage": -2147483648,
                      "ObjectId": null,
                      "VersionId": null,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "QueueId": 0,
                      "QueueName": null,
                      "Tag": null,
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "694f2eeb-5731-4ff9-a98d-1953e92faa9c",
                "OrderInMessage": -2147483648,
                "ObjectId": null,
                "VersionId": null,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "Tag": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "IdentityDocumentList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "CustomerPropertyList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "Id": "0e93ab82-b94c-4ac5-ac5c-8dd5fcb982e4",
          "OrderInMessage": 214,
          "ObjectId": "4546140b-282c-4909-bbe3-fd1259c26cf6",
          "VersionId": null,
          "ToDoAt": "2025-11-18T06:28:57.4287002Z",
          "QueueId": 2,
          "QueueName": "Customer",
          "Tag": null,
          "ObjectName": "ObjectId 4546140b-282c-4909-bbe3-fd1259c26cf6",
          "UpdateBehaviour": 6
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Wariant: dodaj-lub-zaktualizuj (flaga 6)"

    Pojedynczy dłużnik z flagą `ObjectsUpdateBehaviour: 6` i `UpdateBehaviour: 6` per-obiekt. Semantyka: jeżeli dłużnik o danym `MatchKey` istnieje — aktualizuj; jeżeli nie — dodaj.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "updateTest_9_6_646009963",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "640536561",
          "CustomerTypeId": 1,
          "FirstName": "Jan",
          "LastName": "Kowalski",
          "CompanyFullName": "HUBYH CXTFH DTDDLV YOHUTEZOYZNB HUWPHO",
          "SexId": 1,
          "NIP": "EK4689703218",
          "AttributesList": {
            "Objects": [
              {
                "MatchKey": null,
                "MatchKeyType": 0,
                "Value": "0",
                "TypeId": 7689,
                "Id": "fe1b382e-96f3-4019-8a12-a68f367f30eb",
                "OrderInMessage": -2147483648,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "QueueId": 0,
                "QueueName": null,
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "EmailList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "PhoneNumberList": {
            "Objects": [
              {
                "PhoneNumber": "706581109",
                "SourceId": 2,
                "PhoneNumberTypeId": 2,
                "PhonePropertyList": {
                  "Objects": [
                    {
                      "TypeId": 2,
                      "SubtypeId": 2,
                      "ValidFrom": "2025-11-18T00:00:00",
                      "ValidTo": "2100-01-01T00:00:00",
                      "Id": "837281aa-08ab-45a8-bd6a-71b81b233cb4",
                      "OrderInMessage": -2147483648,
                      "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                      "ObjectName": "-2147483648 object in message",
                      "UpdateBehaviour": 6
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "Id": "f33bfd2c-b1d3-4fc1-a5fa-87f01b925208",
                "OrderInMessage": -2147483648,
                "ToDoAt": "2025-11-18T06:28:57.4287002Z",
                "ObjectName": "-2147483648 object in message",
                "UpdateBehaviour": 6
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "AddressList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "IdentityDocumentList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "CustomerPropertyList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "Id": "11ec4620-1765-42dc-9733-4561d3f35be1",
          "OrderInMessage": 92,
          "ObjectId": "11b23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "ToDoAt": "2025-11-18T06:28:57.4287002Z",
          "QueueId": 2,
          "QueueName": "Customer",
          "ObjectName": "ObjectId 11b23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "UpdateBehaviour": 6
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Wariant: tylko aktualizuj (flaga 3)"

    Ten sam dłużnik z flagami `ObjectsUpdateBehaviour: 3` i `UpdateBehaviour: 3`. Semantyka: jeżeli obiekt o danym `MatchKey` istnieje — aktualizuj; jeżeli nie — **nie dodawaj** (komunikat zostanie odrzucony na poziomie tego obiektu). Listy zagnieżdżone (`AttributesList`, `PhoneNumberList`…) zachowują własną flagę aktualizacji.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "updateTest_9_3_646009963",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "640536561",
          "CustomerTypeId": 1,
          "FirstName": "Jan",
          "LastName": "Nowak",
          "CompanyFullName": "HUBYH CXTFH DTDDLV YOHUTEZOYZNB HUWPHO",
          "SexId": 1,
          "NIP": "EK4689703218",
          "AttributesList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "EmailList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "PhoneNumberList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "AddressList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "IdentityDocumentList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "CustomerPropertyList": { "Objects": [], "ObjectsUpdateBehaviour": 6 },
          "Id": "22ec4620-1765-42dc-9733-4561d3f35be1",
          "OrderInMessage": 92,
          "ObjectId": "22b23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "ToDoAt": "2025-11-18T06:28:57.4287002Z",
          "QueueId": 2,
          "QueueName": "Customer",
          "ObjectName": "ObjectId 22b23fd0-a9f5-46d3-a57e-0b9cbfc6d9af",
          "UpdateBehaviour": 3
        }
      ],
      "ObjectsUpdateBehaviour": 3,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Minimalny przykład"

    Najmniejszy zestaw pól umożliwiający dodanie osoby fizycznej — bez list zagnieżdżonych.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Customer",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "DEB-000001",
          "MatchKeyType": 2,
          "ExternalId": "DEB-000001",
          "CustomerTypeId": 1,
          "FirstName": "Jan",
          "LastName": "Kowalski",
          "PESEL": "83041100112",
          "Id": "00000000-0000-0000-0000-000000000001",
          "ObjectId": "00000000-0000-0000-0000-000000000001",
          "ToDoAt": "2025-11-18T06:28:57Z",
          "QueueId": 2,
          "QueueName": "Customer",
          "ObjectName": "ObjectId 00000000-0000-0000-0000-000000000001",
          "UpdateBehaviour": 6
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Case](case.md) (sprawa dłużnika), [Contract](contract.md) (wierzytelność), [Signature](signature.md) (podpis)

</div>
