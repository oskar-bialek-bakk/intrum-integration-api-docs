---
title: "EnqueueImportMessage"
---

# EnqueueImportMessage

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage</div>
</div>

Dodaje komunikat (wiadomość) do kolejki przetwarzania w bazie DEBT Manager (`dm_messages.[nazwa]`). Komunikat jest zawsze powiązany z konkretnym importem przez `importId` zwrócony z [CreateImport](create-import.md). Klient API woła ten endpoint po zgłoszeniu rozpoczęcia fazy Adding ([SetImportStep](set-import-step.md)).

Pole `queueName` przyjmuje nazwę komunikatu (np. `Case`, `Customer`, `Payment`) — pełna lista wraz z formatami: [Wykaz komunikatów](../../komunikaty/index.md#wykaz-komunikatow).

!!! warning "Wymagany aktywny import w fazie Adding"
    `importId` jest **obowiązkowy** i musi wskazywać istniejący import. Pusty GUID (`00000000-0000-0000-0000-000000000000`) jest odrzucany (błąd `ImportId is required.`).

    Dodatkowo import musi być w stanie **Adding / InProgress** (`stage_id=1`, `status_id=1`), czyli po wywołaniu [SetImportStep](set-import-step.md) z `Stage=1`/`StageStatus=1`, a przed jego zamknięciem (`Adding/Done`). Wysłanie komunikatu do importu w innym stanie zwraca błąd (`Import ... cannot accept messages`).

!!! note "queueName vs. kolejki techniczne RabbitMQ"
    Wartość `queueName` to nazwa komunikatu / kolejki SQL Server w schemacie `dm_messages` (np. `Case` → `dm_messages.case_details`). Nie należy jej mylić z **kolejkami technicznymi RabbitMQ** (np. `DebtImportQueue_ImportValidation`), które są używane wyłącznie do orkiestracji walidacji i finalizacji importu i nie są dostępne przez to API. Szczegóły: [Kolejki techniczne](../../kolejki/index.md).

---

<div class="api-section" markdown>
<div class="api-section-title">Request body</div>

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Id importu, w kontekście którego wysyłany jest komunikat. Wartość zwrócona przez <a href="../create-import/">CreateImport</a>.</span>
  </li>
  <li>
    <span class="param-name required">queueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa komunikatu, który zostanie dodany do kolejki przetwarzania (np. <code>Case</code>, <code>Customer</code>, <code>Payment</code>). Pełny wykaz — patrz <a href="../../../komunikaty/#wykaz-komunikatow">Wykaz komunikatów</a>.</span>
  </li>
  <li>
    <span class="param-name required">message</span>
    <span class="param-type">string (JSON)</span>
    <span class="param-desc">Treść komunikatu w formacie JSON — lista obiektów zgodna ze strukturą wybranego komunikatu. Szczegóły — patrz <a href="../../../komunikaty/">Komunikaty</a>.</span>
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
    <span class="param-desc">Lista szczegółów, np. <code>"Błąd w ObjectID a0f22474-...: Brak uzupełnionego pola Waluta"</code></span>
  </li>
  <li>
    <span class="param-name">MessageIds</span>
    <span class="param-type">string[]</span>
    <span class="param-desc">Lista identyfikatorów komunikatów utworzonych w kolejce w wyniku tego wywołania. Wypełniana przy powodzeniu (przy błędzie zwykle pusta).</span>
  </li>
</ul>

=== "Success"

    ```json
    {
      "Status": 0,
      "StatusName": "Success",
      "StatusMessage": null,
      "StatusDetails": [],
      "MessageIds": [
        "9f4bd359-ae27-4edb-a20c-3cd34a865129",
        "f33bfd2c-b1d3-4fc1-a5fa-87f01b925208"
      ]
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
      ],
      "MessageIds": []
    }
    ```

</div>
