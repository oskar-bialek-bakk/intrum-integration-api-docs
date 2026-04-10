---
title: "Action"
---

# Action

<div class="msg-header">
  <span class="msg-type">Action</span>
  <span class="msg-badge queue">kolejka: Action</span>
  <span class="msg-badge db">dm_messages.action_details</span>
  <p class="msg-desc">Komunikat dodaje akcje z rezultatem i atrybutami.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">Action</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">AttachedFiles</span> <span class="tree-type">załączone pliki</span>
</div>

Lista `AttachedFiles` ma własne pole `ObjectsUpdateBehaviour` — patrz [macierz flag aktualizacyjnych](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour).

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
        <span class="mk-id">20</span>
        <span class="mk-label">Wszystkie sprawy windykacyjne powiązane z zewnętrznym ID wierzytelności</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Action</div>

<details class="collapsible-fields">
<summary>Action — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
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
    <span class="param-name">IsClosed</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy akcja jest zamknięta.</span>
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
    <span class="param-name">ToDoAt</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zaplanowanego wykonania.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AttachedFiles — załączone pliki</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">Name</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa pliku.</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opis pliku.</span>
  </li>
  <li>
    <span class="param-name">SendDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wysłania.</span>
  </li>
  <li>
    <span class="param-name">SharedPath</span>
    <span class="param-type">string</span>
    <span class="param-desc">Ścieżka do pliku na dysku współdzielonym.</span>
  </li>
  <li>
    <span class="param-name">Base64Content</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zawartość pliku zakodowana w Base64.</span>
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
      "queueName": "Action",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ActionTypeId": 1012,
          "Comment": "",
          "IsClosed": true,
          "ResultAddedDate": "2023-04-03T00:00:00",
          "ResultPlannedDate": "2023-04-03T00:00:00",
          "ResultTypeId": null,
          "ToDoAt": "2024-04-08T09:04:10.9982539+02:00",
          "AttachedFiles": {
            "Objects": [
              {
                "Name": "Sample.xlsx",
                "Description": "Updated terms of service.",
                "SendDate": "2023-04-03T00:00:00",
                "SharedPath": "C:\\Users\\Downloads\\Sample.xlsx",
                "Base64Content": ""
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

    Dodanie zamkniętej akcji bez załączników.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Action",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ActionTypeId": 1012,
          "IsClosed": true,
          "ResultAddedDate": "2023-04-03T00:00:00",
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
- Powiązane komunikaty: [Case](case.md) (sprawa), [AttributeCase](attribute-case.md) (atrybuty sprawy)

</div>
