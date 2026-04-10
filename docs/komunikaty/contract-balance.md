---
title: "ContractBalance"
---

# ContractBalance

<div class="msg-header">
  <span class="msg-type">ContractBalance</span>
  <span class="msg-badge queue">kolejka: ContractBalance</span>
  <span class="msg-badge db">dm_messages.contractbalance_details</span>
  <p class="msg-desc">Komunikat umożliwia aktualizację stanu finansowego wierzytelności lub zamknięcie powiązanych spraw windykacyjnych.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Zachowanie komunikatu</div>

Działanie komunikatu jest kontrolowane przez dwie flagi:

**`IncludeContractBalance` = `true`** — zmiana salda wierzytelności:

1. Zostaje dodane (lub nadpisane) księgowanie na wskazanym dokumencie księgowym, na wskazanym koncie — kwota i waluta zgodna z przekazanymi wartościami.
2. Jeśli flaga `Discontinue` = `true` — zaległości z pozostałych dokumentów tej wierzytelności są przeksięgowywane na typ *Umorzenie/zaniechanie*. W efekcie aktualna kwota do windykacji będzie zgodna z nowo otrzymaną kwotą.
3. Jeśli flaga `Discontinue` nie jest ustawiona na `true` — zostaje dodany nowy dokument wskazanego typu z nową kwotą, a pozostałe dokumenty pozostają niezmienione. Kwota do windykacji będzie sumą nowej kwoty i dotychczasowej kwoty.

**`CloseOpenedCases` = `true`** — zamknięcie spraw windykacyjnych:

1. Ustawiona na `true` — zamyka wszystkie otwarte sprawy windykacyjne powiązane ze zmienianą wierzytelnością.
2. Ustawiona na `false` — nie zmienia stanu spraw.

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
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu ContractBalance</div>

<details class="collapsible-fields">
<summary>ContractBalance — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">IncludeContractBalance</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy wykonać zmianę salda wierzytelności.</span>
  </li>
  <li>
    <span class="param-name required">CloseOpenedCases</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy zamknąć otwarte sprawy windykacyjne powiązane z wierzytelnością.</span>
  </li>
  <li>
    <span class="param-name">AccountTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ konta księgowego (słownik). Wymagane gdy <code>IncludeContractBalance = true</code>.</span>
  </li>
  <li>
    <span class="param-name">DocumentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dokumentu księgowego (słownik). Wymagane gdy <code>IncludeContractBalance = true</code>.</span>
  </li>
  <li>
    <span class="param-name">Amount</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kwota. Wymagane gdy <code>IncludeContractBalance = true</code>.</span>
  </li>
  <li>
    <span class="param-name">CurrencyId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID waluty (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExchangeRate</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kurs wymiany waluty.</span>
  </li>
  <li>
    <span class="param-name">Discontinue</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy zaniechać — przeksięgować zaległości z pozostałych dokumentów na Umorzenie/zaniechanie.</span>
  </li>
  <li>
    <span class="param-name">Id</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator wewnętrzny.</span>
  </li>
  <li>
    <span class="param-name">ObjectName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa obiektu.</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zaplanowanego wykonania.</span>
  </li>
</ul>
</details>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przykład JSON</div>

=== "Zmiana salda"

    Zmiana salda wierzytelności z zaniechaniem pozostałych dokumentów.

    ```json
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "ContractBalance",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "AccountTypeId": 2,
          "DocumentTypeId": 103,
          "CurrencyId": 2,
          "ExchangeRate": 4.13,
          "Amount": 343.33,
          "Discontinue": true,
          "IncludeContractBalance": true,
          "CloseOpenedCases": false,
          "MatchKey": "3_69485987",
          "MatchKeyType": 3,
          "Id": "0c08cd99-c413-425b-88fd-3cd87d899df4",
          "ObjectId": "10000000-0000-0000-0000-000000000000",
          "ObjectName": "ObjectId 10000000-0000-0000-0000-000000000000",
          "ToDoAt": "2025-08-18T10:18:49.9219838Z"
        }
      ],
      "ObjectsUpdateBehaviour": 2
    }
    ```

=== "Zamknięcie spraw"

    Zamknięcie otwartych spraw windykacyjnych bez zmiany salda.

    ```json
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "ContractBalance",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "IncludeContractBalance": false,
          "CloseOpenedCases": true,
          "MatchKey": "3_69485987",
          "MatchKeyType": 3,
          "Id": "0c08cd99-c413-425b-88fd-3cd87d899df4",
          "ObjectId": "10000000-0000-0000-0000-000000000000",
          "ObjectName": "ObjectId 10000000-0000-0000-0000-000000000000",
          "ToDoAt": "2025-08-18T10:18:49.9219838Z"
        }
      ],
      "ObjectsUpdateBehaviour": 2
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Contract](contract.md) (wierzytelność), [Balance](balance.md) (nadpisanie salda na koncie dokumentu), [Case](case.md) (sprawa)

</div>
