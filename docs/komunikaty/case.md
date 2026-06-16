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
    <span class="param-desc">Klucz wyszukiwania obiektu w DM. Patrz <a href="../#budowa-komunikatów">Budowa komunikatów</a>.</span>
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
    <span class="param-desc">ID typu akcji wyznaczającej początkowy etap sprawy. Wartości odpowiadają etapom (słownik <code>sprawa_etap_typ</code>), np. <code>8</code> (Polubowny). Wartość musi istnieć też w słowniku <code>akcja_typ</code>.</span>
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

Sprawa wiąże klienta i wierzytelność przez `CustomerContractList`. Wyślij komunikaty w kolejności **Customer → Contract → Case** (Case linkuje pozostałe po MatchKey). `CustomerRoleId` ze słownika `sprawa_rola_typ` (`1` = Dłużnik główny).

=== "Minimalny (powiązany z Customer 990000001 + Contract 880000001)"

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ObjectId": "a570d857-fb10-4d46-aaca-8f4707d4657f",
          "MatchKey": "770000001",
          "MatchKeyType": 6,
          "ExternalCaseNumber": "770000001",
          "StageAction": { "ActionTypeId": 8 },
          "CustomerContractList": {
            "Objects": [
              { "CustomerRoleId": 1, "CustomerMatchKey": "990000001", "CustomerMatchKeyType": 2, "ContractMatchKey": "880000001", "ContractMatchKeyType": 3 }
            ],
            "ObjectsUpdateBehaviour": 6
          }
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Z atrybutem (powiązany z Customer 990000002 + Contract 880000002)"

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ObjectId": "e6e0e3ea-070c-43f1-8b80-4e9a66e4c551",
          "MatchKey": "770000002",
          "MatchKeyType": 6,
          "ExternalCaseNumber": "770000002",
          "Comment": "Sprawa testowa Anna Nowak",
          "StageAction": { "ActionTypeId": 8 },
          "AttributesList": {
            "Objects": [
              { "TypeId": 8272, "Value": "I C 456/24" }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "CustomerContractList": {
            "Objects": [
              { "CustomerRoleId": 1, "CustomerMatchKey": "990000002", "CustomerMatchKeyType": 2, "ContractMatchKey": "880000002", "ContractMatchKeyType": 3 }
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
- Powiązane komunikaty: [Customer](customer.md) (dłużnik), [Contract](contract.md) (wierzytelność), [Action](action.md) (akcje), [AttributeCase](attribute-case.md) (atrybuty sprawy)

</div>
