---
title: "ImportFromFileShare"
---

# ImportFromFileShare

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://[adres_api]/ImportFromFileShare</div>
</div>

Wykonuje import pliku z lokalizacji sieciowej (file share). Ścieżka do pliku przekazywana jest jako parametr query.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">importTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ importu, który chcemy wykonać</span>
  </li>
  <li>
    <span class="param-name required">fileLocalization</span>
    <span class="param-type">string</span>
    <span class="param-desc">Lokalizacja pliku na zasobie sieciowym (URL-encoded)</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl -X POST "https://[adres_api]/ImportFromFileShare?importTypeId=16&fileLocalization=ServerFiles%5CImport%5CSample.xlsx" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Struktura odpowiedzi jest identyczna jak w funkcji [GetImportStatus](get-import-status.md).

</div>
