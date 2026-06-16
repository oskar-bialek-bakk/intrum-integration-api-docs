---
title: "GetDictionaries"
---

# GetDictionaries

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetDictionaries</div>
</div>

Pobiera wartości wszystkich słowników używanych przez API. Zwraca listę tabel słownikowych wraz z ich wpisami.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

Brak parametrów.

```bash title="Przykład wywołania"
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetDictionaries" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Tablica słowników. Każdy element zawiera:

<ul class="param-list">
  <li>
    <span class="param-name">name</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa tabeli słownikowej w postaci <code>nazwa_schematu.nazwa_tabeli</code></span>
  </li>
  <li>
    <span class="param-name">rows</span>
    <span class="param-type">DictionaryRow[]</span>
    <span class="param-desc">Lista wierszy występujących w słowniku</span>
  </li>
</ul>

**Struktura `DictionaryRow`:**

<ul class="param-list">
  <li>
    <span class="param-name">id</span>
    <span class="param-type">long</span>
    <span class="param-desc">ID wpisu z tabeli słownikowej</span>
  </li>
  <li>
    <span class="param-name">value</span>
    <span class="param-type">object</span>
    <span class="param-desc">Wartość wpisu słownikowego (zwykle tekst, np. nazwa pozycji)</span>
  </li>
</ul>

!!! note "Nazewnictwo pól"
    Odpowiedź jest serializowana z polityką camelCase. Wiersz słownika ma pola `id` i `value` (nie `Name`).

```json title="Przykład odpowiedzi"
[
  {
    "name": "dbo.umowa_kontrahent",
    "rows": [
      { "id": 94, "value": "IT Import Test" },
      { "id": 1, "value": "PKL" },
      { "id": 2, "value": "VELO" }
    ]
  },
  {
    "name": "dbo.sygnatura_typ",
    "rows": [
      { "id": 1, "value": "Sądowa" },
      { "id": 2, "value": "Komornicza" },
      { "id": 3, "value": "Policyjna" },
      { "id": 4, "value": "Prokuratorska" }
    ]
  }
]
```

</div>
