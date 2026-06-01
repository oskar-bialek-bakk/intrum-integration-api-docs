---
title: "GetImportFile"
---

# GetImportFile

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportFile</div>
</div>

Zwraca plik importu na podstawie ID importu. Odpowiedź zawiera binarną zawartość pliku.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu, dla którego chcemy pobrać plik</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportFile?importId=43A79F4D-CBFE-4799-AE2A-48B68CD5EA97" \
  -H "Authorization: Bearer {TOKEN}" \
  -o import_file.xlsx
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Plik importu (binary). Content-Type zależy od formatu pliku.

</div>
