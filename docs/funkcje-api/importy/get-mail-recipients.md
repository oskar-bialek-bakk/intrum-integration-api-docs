---
title: "GetMailRecipients"
---

# GetMailRecipients

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetMailRecipients</div>
</div>

Pobiera listę adresatów wiadomości e-mail, do których wysyłane są informacje o zakończeniu importów. Bez parametrów zwraca pełną listę adresatów.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Opcjonalne: ID importu, dla którego pobieramy listę adresatów</span>
  </li>
  <li>
    <span class="param-name">exportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Opcjonalne: ID eksportu, dla którego pobieramy listę adresatów</span>
  </li>
</ul>

!!! note "Brak parametru"
    Jeśli nie przekażemy ani `importId`, ani `exportId`, funkcja zwróci pełną listę adresatów.

```bash title="Przykład wywołania"
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetMailRecipients?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Tablica obiektów:

<ul class="param-list">
  <li>
    <span class="param-name">Title</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tytuł wysyłanej wiadomości e-mail</span>
  </li>
  <li>
    <span class="param-name">Recipients</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adresy e-mail adresatów (oddzielone średnikami)</span>
  </li>
  <li>
    <span class="param-name">Agreement</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa bazy, w kontekście której wysyłana jest wiadomość</span>
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
    <span class="param-name">ExportTypeId</span>
    <span class="param-type">int?</span>
    <span class="param-desc">ID typu eksportu (wypełniane dla adresatów powiązanych z eksportem; dla importów <code>null</code>)</span>
  </li>
  <li>
    <span class="param-name">ImportTypeId</span>
    <span class="param-type">int?</span>
    <span class="param-desc">ID słownika z tabeli <code>import_typ</code></span>
  </li>
  <li>
    <span class="param-name">Link</span>
    <span class="param-type">string</span>
    <span class="param-desc">Link do szczegółów importu w aplikacji webowej</span>
  </li>
</ul>

```json title="Przykład odpowiedzi"
[
  {
    "Title": "t1",
    "Recipients": "test@bakk.com;test@gmail.com",
    "Agreement": "PKL",
    "Creditor": "PKO Leasing S.A.",
    "Operation": "PKL WYCOFANE",
    "ImportTypeId": 12,
    "Link": "/user/blader/agreement-case/data-imports?caseId=703&creditorId=2&data=agreement&id=1"
  }
]
```

</div>
