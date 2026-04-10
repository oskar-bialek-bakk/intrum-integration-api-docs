---
title: "Case"
---

# Case

<div class="msg-header">
  <span class="msg-type">Case</span>
  <span class="msg-badge queue">kolejka: Case</span>
  <span class="msg-badge db">dm_messages.case_details</span>
  <p class="msg-desc">Komunikat dodaje sprawę z atrybutami sprawy oraz powiązania klientów i wierzytelności do sprawy.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">Case</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">StageAction</span> <span class="tree-type">etap / akcja na sprawie</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AttributesList</span> <span class="tree-type">lista atrybutów sprawy</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">CustomerContractList</span> <span class="tree-type">powiązania klient–wierzytelność</span>
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
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Case</div>

<details class="collapsible-fields">
<summary>Case — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania (GUID lub ID z systemu zewnętrznego).</span>
  </li>
  <li>
    <span class="param-name required">ExternalCaseNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny numer sprawy w systemie klienta.</span>
  </li>
  <li>
    <span class="param-name">RepaymentAccount</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer rachunku bankowego do spłat.</span>
  </li>
  <li>
    <span class="param-name">CreateRepaymentAccount</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy utworzyć rachunek do spłat, jeśli nie istnieje.</span>
  </li>
  <li>
    <span class="param-name">Comment</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komentarz do sprawy.</span>
  </li>
  <li>
    <span class="param-name">ServiceStartDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data rozpoczęcia obsługi.</span>
  </li>
  <li>
    <span class="param-name">ServiceEndDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zakończenia obsługi.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>StageAction — etap / akcja</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ActionTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu akcji (słownik).</span>
  </li>
  <li>
    <span class="param-name">ResultTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu rezultatu (słownik).</span>
  </li>
  <li>
    <span class="param-name">Comment</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komentarz do akcji.</span>
  </li>
  <li>
    <span class="param-name">ResultAddedDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data dodania rezultatu.</span>
  </li>
  <li>
    <span class="param-name">ResultPlannedDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Planowana data rezultatu.</span>
  </li>
  <li>
    <span class="param-name">IsClosed</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy akcja jest zamknięta.</span>
  </li>
  <li>
    <span class="param-name">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania akcji (opcjonalny).</span>
  </li>
  <li>
    <span class="param-name">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania akcji.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AttributesList — atrybuty sprawy</summary>
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
</ul>
</details>

<details class="collapsible-fields">
<summary>CustomerContractList — powiązania klient–wierzytelność</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">CustomerRoleId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID roli klienta w sprawie (słownik).</span>
  </li>
  <li>
    <span class="param-name required">CustomerMatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania klienta (dłużnika).</span>
  </li>
  <li>
    <span class="param-name required">CustomerMatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania klienta.</span>
  </li>
  <li>
    <span class="param-name required">ContractMatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania wierzytelności.</span>
  </li>
  <li>
    <span class="param-name required">ContractMatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania wierzytelności.</span>
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
      "queueName": "Case",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ExternalCaseNumber": "SZC00/W/WTSPR/9877817/33/24_10013_94",
          "RepaymentAccount": "17124069577501000008529904",
          "CreateRepaymentAccount": false,
          "Comment": null,
          "ServiceStartDate": "2023-09-18T00:00:00",
          "ServiceEndDate": null,
          "StageAction": {
            "ActionTypeId": 20,
            "ResultTypeId": null,
            "Comment": "",
            "ResultAddedDate": "2024-04-10T00:00:00+02:00",
            "ResultPlannedDate": "2024-04-10T00:00:00+02:00",
            "IsClosed": true,
            "MatchKey": null,
            "MatchKeyType": 0
          },
          "AttributesList": {
            "Objects": [
              { "Value": "test", "TypeId": 126 }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "CustomerContractList": {
            "Objects": [
              {
                "CustomerRoleId": 1,
                "CustomerMatchKey": "Customer_MK_1",
                "CustomerMatchKeyType": 2,
                "ContractMatchKey": "Contract_MK_1",
                "ContractMatchKeyType": 3
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Tylko wymagane pola — dodanie sprawy z jednym powiązaniem klient–wierzytelność.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Case",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ExternalCaseNumber": "SP/2024/001",
          "CustomerContractList": {
            "Objects": [
              {
                "CustomerRoleId": 1,
                "CustomerMatchKey": "Customer_MK_1",
                "CustomerMatchKeyType": 2,
                "ContractMatchKey": "Contract_MK_1",
                "ContractMatchKeyType": 3
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Customer](customer.md) (dłużnik), [Contract](contract.md) (wierzytelność), [Action](action.md) (akcje), [AttributeCase](attribute-case.md) (atrybuty sprawy)

</div>
