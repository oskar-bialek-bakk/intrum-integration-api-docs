---
title: "ImportFromFileUpload"
---

# ImportFromFileUpload

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://[adres_api]/ImportFromFileUpload</div>
</div>

Wykonuje import wybranego pliku. Plik przesyłany jest jako `multipart/form-data`.

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
</ul>

**Body** (`multipart/form-data`):

<ul class="param-list">
  <li>
    <span class="param-name required">file</span>
    <span class="param-type">file</span>
    <span class="param-desc">Plik do zaimportowania</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl -X POST "https://[adres_api]/ImportFromFileUpload?importTypeId=16" \
  -H "Authorization: Bearer {TOKEN}" \
  -F "file=@SampleImport.xlsx"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Struktura odpowiedzi jest identyczna jak w funkcji [GetImportStatus](get-import-status.md).

</div>
