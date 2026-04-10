---
title: "GetDictionaries"
---

# GetDictionaries

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/GetDictionaries</div>
</div>

Pobiera wartości wszystkich słowników używanych przez API. Zwraca listę tabel słownikowych wraz z ich wpisami.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

Brak parametrów.

```bash title="Przykład wywołania"
curl "https://[adres_api]/GetDictionaries" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Tablica słowników. Każdy element zawiera:

<ul class="param-list">
  <li>
    <span class="param-name">Name</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa tabeli słownikowej w postaci <code>nazwa_schematu.nazwa_tabeli</code></span>
  </li>
  <li>
    <span class="param-name">Rows</span>
    <span class="param-type">DictionaryRow[]</span>
    <span class="param-desc">Lista wierszy występujących w słowniku</span>
  </li>
</ul>

**Struktura `DictionaryRow`:**

<ul class="param-list">
  <li>
    <span class="param-name">Id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID wpisu z tabeli słownikowej</span>
  </li>
  <li>
    <span class="param-name">Name</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa wpisu z tabeli słownikowej</span>
  </li>
</ul>

```json title="Przykład odpowiedzi"
[
  {
    "Name": "dbo.umowa_kontrahent",
    "Rows": [
      { "Id": 94, "Name": "IT Import Test" },
      { "Id": 1, "Name": "PKL" },
      { "Id": 2, "Name": "VELO" }
    ]
  },
  {
    "Name": "dbo.sygnatura_typ",
    "Rows": [
      { "Id": 1, "Name": "Sądowa" },
      { "Id": 2, "Name": "Komornicza" },
      { "Id": 3, "Name": "Policyjna" },
      { "Id": 4, "Name": "Prokuratorska" }
    ]
  }
]
```

</div>
