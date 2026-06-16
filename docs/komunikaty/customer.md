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
    <span class="param-name">Id</span>
    <span class="param-type">GUID</span>
    <span class="param-desc">Identyfikator obiektu w ramach pojedynczego komunikatu. Unikalny per obiekt.</span>
  </li>
  <li>
    <span class="param-name">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania (np. przez <a href="../../funkcje-api/importy/get-object-status-by-id/">GetObjectStatusByID</a>) — GUID albo ID z systemu zewnętrznego, patrz <a href="../#budowa-komunikatów">Budowa komunikatów</a>. Dla obiektu głównego <code>Customer</code> wymagany; dla obiektów zagnieżdżonych zazwyczaj <code>null</code> (rolę identyfikatora pełni <code>Id</code>).</span>
  </li>
  <li>
    <span class="param-name">VersionId</span>
    <span class="param-type">long</span>
    <span class="param-desc">Identyfikator wersji obiektu. Wypełniany w przypadku aktualizacji konkretnej wersji obiektu w DM; <code>null</code> przy operacji dodania.</span>
  </li>
  <li>
    <span class="param-name">OrderInMessage</span>
    <span class="param-type">int</span>
    <span class="param-desc">Kolejność obiektu w komunikacie. Dla obiektów zagnieżdżonych typowo <code>-2147483648</code> (brak kolejności). Dla obiektów głównych — kolejność wynikająca z pliku/źródła danych.</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
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
    <span class="param-name">UpdateBehaviour</span>
    <span class="param-type">int</span>
    <span class="param-desc">Flaga sterująca aktualizacją konkretnego obiektu (per-obiekt). Wartości jak w polu listowym <code>ObjectsUpdateBehaviour</code>, patrz <a href="../#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour">macierz flag</a>.</span>
  </li>
</ul>

Pola `Id`, `ToDoAt`, `UpdateBehaviour`, `VersionId`, `OrderInMessage`, `QueueId`, `QueueName`, `Tag`, `ObjectName` mają bezpieczne wartości domyślne i nie są wymagane w minimalnym komunikacie.

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
    <span class="param-desc">ID użytkownika DM, który rejestrowany jest jako autor wprowadzonych zmian (audyt). Typowo <code>5</code> = konto integracyjne. Musi być istniejącym operatorem (<code>GE_USER.US_ID</code>) w bazie DM, bo wartość trafia na kolumny FK (np. <code>rezultat.re_us_id_*</code>). Nieistniejący operator jest odrzucany na etapie konsumpcji z błędem <code>Operator (US_ID=X) does not exist in GE_USER</code>.</span>
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
    <span class="param-name required">DebtorPortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela dłużnika.</span>
  </li>
  <li>
    <span class="param-name required">ExternalId</span>
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

Poniższe przykłady zawierają **tylko pola faktycznie potrzebne**, aby komunikat przeszedł. Pola techniczne (`Id`, `VersionId`, `OrderInMessage`, `QueueId`, `QueueName`, `Tag`, `ObjectName`, `ToDoAt`, per-obiektowe `UpdateBehaviour`) są pomijane: API uzupełnia je domyślnie.

=== "Minimalny (osoba fizyczna)"

    Najmniejszy komunikat dodający osobę fizyczną. Pole `message` to **string z JSON** (poniżej rozpakowana zawartość).

    ```json title="Koperta API (request body)"
    {
      "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
      "queueName": "Customer",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message (po deserializacji)"
    {
      "Objects": [
        {
          "ObjectId": "78599fcf-6f66-40e4-b70e-a1345674ebf4",
          "MatchKey": "990000001",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "990000001",
          "CustomerTypeId": 1,
          "FirstName": "Jan",
          "LastName": "Kowalski",
          "PESEL": "85061504513"
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Z danymi kontaktowymi (zagnieżdżone listy)"

    Osoba fizyczna z atrybutem, e-mailem, telefonem i adresem. Wartości słownikowe (`CustomerTypeId`, `SexId`, `SourceId`, `TypeId`, `EmailTypeId`, `PhoneNumberTypeId`, `AddressTypeId`) muszą istnieć w słownikach (patrz [GetDictionaries](../funkcje-api/importy/get-dictionaries.md)).

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ObjectId": "f9e981a7-fd05-438a-b35a-add747bb3118",
          "MatchKey": "990000002",
          "MatchKeyType": 2,
          "DebtorPortfolioId": 1,
          "ExternalId": "990000002",
          "CustomerTypeId": 1,
          "FirstName": "Anna",
          "LastName": "Nowak",
          "PESEL": "90032204529",
          "SexId": 1,
          "SourceId": 2,
          "AttributesList": {
            "Objects": [
              { "TypeId": 8272, "Value": "I C 123/24" }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "EmailList": {
            "Objects": [
              { "Email": "anna.nowak@example.com", "EmailTypeId": 1, "SourceId": 2 }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "PhoneNumberList": {
            "Objects": [
              { "PhoneNumber": "600100200", "PhoneNumberTypeId": 2, "SourceId": 2 }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "AddressList": {
            "Objects": [
              { "AddressTypeId": 1, "Street": "Marszałkowska 1", "PostalCode": "00-001", "City": "Warszawa", "SourceId": 2 }
            ],
            "ObjectsUpdateBehaviour": 6
          }
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
