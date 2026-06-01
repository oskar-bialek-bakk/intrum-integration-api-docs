---
title: "GetImportFileFromFileShare"
---

# GetImportFileFromFileShare

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportFileFromFileShare</div>
</div>

Kopiuje plik wybranego importu do lokalizacji na zasobie sieciowym, a następnie zwraca ścieżkę do tego pliku.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">ImportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportFileFromFileShare?ImportId=1538E757-F973-47B3-BD8A-76E384C25BDA" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Ścieżka do pliku importu na zasobie sieciowym (string).

</div>
