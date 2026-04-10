---
title: "CaseContactData"
---

# CaseContactData

<div class="msg-header">
  <span class="msg-type">CaseContactData</span>
  <span class="msg-badge queue">kolejka: CaseContactData</span>
  <span class="msg-badge db">dm_messages.CaseContactData_details</span>
  <p class="msg-desc">Komunikat pozwalający powiązać wybrane dane teleadresowe dłużnika z jedną z jego spraw.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">CaseContactData</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AddressList</span> <span class="tree-type">adresy korespondencyjne / zameldowania</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">EmailList</span> <span class="tree-type">adresy e-mail</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">PhoneNumberList</span> <span class="tree-type">numery telefonów</span>
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
    <span class="param-desc">Klucz wyszukiwania obiektu w DM. Patrz <a href="index.md#budowa-komunikatów">Budowa komunikatów</a>.</span>
  </li>
  <li>
    <span class="param-name required">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania. Dopuszczalne wartości:</span>
    <ul class="mk-values">
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
      <li>
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer wierzytelności</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pola obiektu CaseContactData</div>

!!! info "Uwaga"
    Ten komunikat nie korzysta z wrappera `Objects` — pola `MatchKey`, `MatchKeyType` oraz listy zagnieżdżone znajdują się bezpośrednio w obiekcie `message`.

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
</ul>
</details>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przykład JSON</div>

=== "Pełny komunikat"

    ```json
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "CaseContactData",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "MatchKey": "Case_MK_1",
      "MatchKeyType": 6,
      "AddressList": {
        "Objects": [
          {
            "AddressTypeId": 12,
            "SourceId": 1,
            "Country": "Polska",
            "Street": "test",
            "HouseNumber": "1",
            "ApartmentNumber": "1",
            "PostalCode": "01-111",
            "City": "Warszawa",
            "PostalOffice": "test",
            "AdditionalInformation": "test",
            "RegionId": 1,
            "ExternalId": "1"
          }
        ],
        "ObjectsUpdateBehaviour": 6
      },
      "EmailList": {
        "Objects": [
          {
            "Email": "test@example.com",
            "SourceId": 1,
            "EmailTypeId": 1,
            "ExternalId": "1",
            "AdditionalInformation": "test"
          }
        ],
        "ObjectsUpdateBehaviour": 6
      },
      "PhoneNumberList": {
        "Objects": [
          {
            "PhoneNumber": "600123456",
            "SourceId": 1,
            "Comment": "test",
            "PhoneNumberTypeId": 1,
            "ExternalId": "1"
          }
        ],
        "ObjectsUpdateBehaviour": 6
      }
    }
    ```

=== "Minimalny przykład"

    Powiązanie jednego adresu e-mail ze sprawą.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "CaseContactData",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "MatchKey": "Case_MK_1",
      "MatchKeyType": 6,
      "EmailList": {
        "Objects": [
          {
            "Email": "jan.kowalski@example.com",
            "EmailTypeId": 1,
            "ExternalId": "1"
          }
        ],
        "ObjectsUpdateBehaviour": 6
      }
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Customer](customer.md) (dłużnik — źródło danych kontaktowych), [Case](case.md) (sprawa)

</div>
