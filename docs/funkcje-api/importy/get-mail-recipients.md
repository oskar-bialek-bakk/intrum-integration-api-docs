---
title: "GetMailRecipients"
---

# GetMailRecipients

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/GetMailRecipients</div>
</div>

Pobiera listę adresatów wiadomości e-mail, do których wysyłane są informacje o zakończeniu importów/eksportów. Bez parametrów zwraca pełną listę adresatów.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Opcjonalne — ID importu, dla którego pobieramy listę adresatów</span>
  </li>
  <li>
    <span class="param-name">exportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Opcjonalne — ID eksportu, dla którego pobieramy listę adresatów</span>
  </li>
</ul>

!!! note "Brak parametrów"
    Jeśli nie przekażemy żadnego z parametrów, funkcja zwróci pełną listę adresatów.

```bash title="Przykład wywołania"
curl "https://[adres_api]/GetMailRecipients?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
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
    <span class="param-desc">Nazwa importu/eksportu</span>
  </li>
  <li>
    <span class="param-name">ExportTypeId</span>
    <span class="param-type">int?</span>
    <span class="param-desc">ID słownika z tabeli <code>export_typ</code> (null dla importów)</span>
  </li>
  <li>
    <span class="param-name">ImportTypeId</span>
    <span class="param-type">int?</span>
    <span class="param-desc">ID słownika z tabeli <code>import_typ</code> (null dla eksportów)</span>
  </li>
  <li>
    <span class="param-name">Link</span>
    <span class="param-type">string</span>
    <span class="param-desc">Link do szczegółów importu/eksportu w aplikacji webowej</span>
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
    "ExportTypeId": null,
    "ImportTypeId": 12,
    "Link": "/user/blader/agreement-case/data-imports?caseId=703&creditorId=2&data=agreement&id=1"
  }
]
```

</div>
