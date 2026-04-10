---
title: "RefreshDictionaries"
---

# RefreshDictionaries

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/RefreshDictionaries</div>
</div>

Odświeża cache wszystkich słowników używanych do walidacji importów (te same słowniki, które zwraca [GetDictionaries](get-dictionaries.md)). Wywołanie nie wymaga parametrów.

!!! tip "Kiedy wywoływać"
    Wywołaj `RefreshDictionaries` po każdej zmianie danych słownikowych w bazie (np. dodanie nowego typu importu, zmiana statusów). API cache'uje słowniki w pamięci — bez odświeżenia walidacje będą korzystać z nieaktualnych wartości.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

Brak parametrów.

```bash title="Przykład wywołania"
curl "https://[adres_api]/RefreshDictionaries" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Brak treści odpowiedzi (HTTP 200 w przypadku sukcesu).

</div>
