---
title: "CreateImport"
---

# CreateImport

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/CreateImport</div>
</div>

Tworzy nowy rekord importu w schemacie `dm_imports` i zwraca jego identyfikator. **`importId` zwrócony z tego endpointu jest wymagany do wszystkich kolejnych wywołań** związanych z importem ([SetImportStep](set-import-step.md), [SetImportValidations](set-import-validations.md), [EnqueueImportMessage](enqueue-import-message.md), [GetImportStatus](get-import-status.md)).

Po utworzeniu importu Klient API zgłasza rozpoczęcie fazy Adding przez [SetImportStep](set-import-step.md) z `Stage=1`, `StageStatus=1`.

---

<div class="api-section" markdown>
<div class="api-section-title">Request body</div>

<ul class="param-list">
  <li>
    <span class="param-name required">ImportTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu importu z tabeli konfiguracyjnej w schemacie <code>dm_config</code>. Lista dostępnych typów importu — patrz <a href="../../../przypadki-uzycia/nowy-typ-importu/">Nowy typ importu</a>.</span>
  </li>
  <li>
    <span class="param-name required">CreditorId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kontrahenta (<code>kontrahent.ko_id</code> w DM), w ramach którego wykonywany jest import.</span>
  </li>
  <li>
    <span class="param-name required">PortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela (<code>umowa_kontrahent.uko_id</code> w DM), w ramach którego wykonywany jest import.</span>
  </li>
  <li>
    <span class="param-name">ExternalReference</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opcjonalny zewnętrzny identyfikator wsadu po stronie Klienta API (np. ID partii w systemie pośredniczącym). Wykorzystywany do śledzenia importu w logach.</span>
  </li>
</ul>

```json title="Przykład request body"
{
  "ImportTypeId": 100,
  "CreditorId": 1,
  "PortfolioId": 1,
  "ExternalReference": "opcjonalny opis importu"
}
```

!!! note
    Ten typ importu (`ImportTypeId: 100`, `CreditorId: 1`, `PortfolioId: 1`) został utworzony na środowisku testowym (`dmapi-intrum-dev`) specjalnie do testowania importów przez API integracyjne.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Walidacja</div>

API weryfikuje spójność żądania z konfiguracją typu importu. Wywołanie kończy się błędem, gdy:

- `CreditorId` nie zgadza się z kontrahentem skonfigurowanym dla danego `ImportTypeId` (kolumna `dm_ko_id` w `dm_config.import_type`),
- `PortfolioId` nie zgadza się z portfelem skonfigurowanym dla tego typu importu (kolumna `dm_uko_id`),
- typ importu nie jest skonfigurowany jako import API integracyjnego (klasa `IntegrationsApiImport`).

`ImportId` jest generowany po stronie serwera (klient go nie podaje) i musi być użyty we wszystkich kolejnych wywołaniach importu ([SetImportStep](set-import-step.md), [EnqueueImportMessage](enqueue-import-message.md), [GetImportStatus](get-import-status.md)).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

<ul class="param-list">
  <li>
    <span class="param-name">ImportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">Unikalny identyfikator utworzonego importu. Używany we wszystkich kolejnych wywołaniach API.</span>
  </li>
  <li>
    <span class="param-name">ImportTypeName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa typu importu — odczytana z konfiguracji w <code>dm_config</code>.</span>
  </li>
</ul>

```json title="Przykład response"
{
  "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "ImportTypeName": "DM Integration API Sample - External Payments Import"
}
```

</div>
