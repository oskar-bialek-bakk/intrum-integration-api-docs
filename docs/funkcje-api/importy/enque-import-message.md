---
title: "EnqueImportMessage"
---

# EnqueImportMessage

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://[adres_api]/EnqueImportMessage</div>
</div>

Dodaje wiadomość (komunikat) na wybraną kolejkę przetwarzania. Komunikat może być powiązany z konkretnym importem lub wysłany niezależnie (z pustym GUID).

Szczegóły dostępnych kolejek i formatów komunikatów — patrz [Komunikaty](../../komunikaty/index.md).

---

<div class="api-section" markdown>
<div class="api-section-title">Request body</div>

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Id importu, z którym powiązany jest komunikat. Jeśli komunikat nie jest powiązany z żadnym importem, przekaż pusty GUID: <code>00000000-0000-0000-0000-000000000000</code></span>
  </li>
  <li>
    <span class="param-name required">queueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki, na którą zostanie dodana wiadomość (np. <code>Case</code>, <code>Customer</code>, <code>Payment</code>). Lista kolejek — patrz <a href="../../kolejki/index.md">Kolejki</a></span>
  </li>
  <li>
    <span class="param-name required">message</span>
    <span class="param-type">string (JSON)</span>
    <span class="param-desc">Treść komunikatu w formacie JSON — lista obiektów zgodna ze strukturą wybranej kolejki. Szczegóły — patrz <a href="../../komunikaty/index.md">Komunikaty</a></span>
  </li>
</ul>

```json title="Przykład request body"
{
  "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "queueName": "Case",
  "message": "[{\"ObjectID\": \"bd459997-d8b3-4ae5-b00f-fe1a5042bd5a\", ...}]"
}
```

!!! note "Pole `message`"
    Wartość pola `message` to **string zawierający JSON** (nie obiekt JSON bezpośrednio).
    Lista obiektów musi być serializowana do stringa przed wysłaniem.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

<ul class="param-list">
  <li>
    <span class="param-name">Status</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status operacji:</span>
<ul class="status-values">
<li><code>0</code> — Success</li>
<li><code>1</code> — Warning</li>
<li><code>2</code> — Error</li>
</ul>
  </li>
  <li>
    <span class="param-name">StatusName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa statusu: <code>Success</code>, <code>Warning</code>, <code>Error</code></span>
  </li>
  <li>
    <span class="param-name">StatusMessage</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tekstowy opis statusu, np. <code>"W przekazanych komunikatach występują błędy walidacji"</code></span>
  </li>
  <li>
    <span class="param-name">StatusDetails</span>
    <span class="param-type">string[]</span>
    <span class="param-desc">Lista szczegółów — np. <code>"Błąd w ObjectID a0f22474-...: Brak uzupełnionego pola Waluta"</code></span>
  </li>
</ul>

=== "Success"

    ```json
    {
      "Status": 0,
      "StatusName": "Success",
      "StatusMessage": null,
      "StatusDetails": []
    }
    ```

=== "Error"

    ```json
    {
      "Status": 2,
      "StatusName": "Error",
      "StatusMessage": "W przekazanych komunikatach występują błędy walidacji",
      "StatusDetails": [
        "Błąd w ObjectID a0f22474-4cf1-4716-85a6-8ab84067b261: Brak uzupełnionego pola Waluta"
      ]
    }
    ```

</div>
