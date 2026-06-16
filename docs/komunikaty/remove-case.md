---
title: "RemoveCase"
---

# RemoveCase

<div class="msg-header">
  <span class="msg-type">RemoveCase</span>
  <span class="msg-badge queue">kolejka: RemoveCase</span>
  <span class="msg-badge db">dm_messages.removeCase_details</span>
  <p class="msg-desc">Komunikat pozwalający na usunięcie wybranej sprawy z systemu.</p>
</div>

!!! warning "Komunikat w przygotowaniu (do potwierdzenia)"
    Na branchu `features/Intrum/S1` nie znaleziono implementacji tego komunikatu w API integracyjnym (brak klasy obiektu i kolejki w warstwie Messages). Opis poniżej jest propozycją i wymaga potwierdzenia przez dewelopera API: czy komunikat jest planowany, czy obsługiwany inną drogą. Do tego czasu nie należy zakładać, że jest dostępny na środowisku dev.

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
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">5</span>
        <span class="mk-label">Zewnętrzny numer dokumentu</span>
        <span class="mk-field">→ dokument.do_ext_id</span>
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

<div class="api-section">
<div class="api-section-title">Pola obiektu RemoveCase</div>

<details class="collapsible-fields">
<summary>RemoveCase — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name">CaseId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID sprawy do usunięcia (wewnętrzny identyfikator DM).</span>
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
      "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
      "queueName": "RemoveCase",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "CaseId": 1,
          "MatchKey": "SZC00/W/WTSPR/9877817/33/24_10013_94",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ]
    }
    ```

=== "Minimalny przykład"

    ```json title="Koperta API"
    {
      "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
      "queueName": "RemoveCase",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "SP/2024/001",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ]
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md)
- Powiązane komunikaty: [Case](case.md) (dodawanie sprawy)

</div>
