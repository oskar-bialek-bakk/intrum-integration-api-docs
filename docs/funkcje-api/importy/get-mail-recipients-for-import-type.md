---
title: "GetMailRecipientsForImportType"
---

# GetMailRecipientsForImportType

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/GetMailRecipientsForImportType</div>
</div>

Pobiera listę adresatów wiadomości e-mail dla wskazanego typu importu.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">importTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu importu, dla którego pobieramy listę adresatów</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl "https://[adres_api]/GetMailRecipientsForImportType?importTypeId=12" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Tablica obiektów:

<ul class="param-list">
  <li>
    <span class="param-name">Recipients</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adresy e-mail adresatów (oddzielone średnikami)</span>
  </li>
  <li>
    <span class="param-name">Agreement</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa bazy, w której znajduje się typ importu</span>
  </li>
  <li>
    <span class="param-name">Creditor</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kontrahenta</span>
  </li>
  <li>
    <span class="param-name">Operation</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa importu</span>
  </li>
  <li>
    <span class="param-name">ImportTypeId</span>
    <span class="param-type">int?</span>
    <span class="param-desc">ID słownika z tabeli <code>import_typ</code></span>
  </li>
</ul>

!!! note "Nieużywane pola"
    Pola `Title`, `ExportTypeId` i `Link` są obecne w odpowiedzi, ale zawsze mają wartość `null`.

```json title="Przykład odpowiedzi"
[
  {
    "Title": null,
    "Recipients": "test@bakk.com;test@gmail.com",
    "Agreement": "PKL",
    "Creditor": "PKO Leasing S.A.",
    "Operation": "PKL WYCOFANE",
    "ExportTypeId": null,
    "ImportTypeId": 12,
    "Link": null
  }
]
```

</div>
