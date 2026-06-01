---
title: "Nowy typ importu"
---

# Nowy typ importu

<div class="api-section" markdown>
<div class="api-section-title">Opis</div>

Przewodnik pokazuje jak skonfigurować nowy typ importu obsługiwanego zewnętrznie. Operacja wymaga wykonania insertów SQL w bazie **DEBT Manager** — w schematach `dm_config` (definicja typu importu) oraz `dm_validations` (reguły walidacji powiązane z typem importu).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 1 — Przygotowanie zmiennych</div>

Na początek zdefiniuj wartości parametrów nowego typu importu. Upewnij się, że ID kontrahenta i portfela istnieją w systemie — jeśli nie, dodaj je wcześniej do odpowiednich tabel.

```sql
DECLARE
     @dm_import_type_id          int          = 200
    ,@import_type_name           varchar(500) = 'My New case import'
    ,@integration_import_type_id int          = 200
    ,@dm_orginal_creditor_id     int          = 10013
    ,@dm_portfolio_id            int          = 94
```

<ul class="param-list">
  <li>
    <span class="param-name required">@dm_import_type_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">Unikalne ID typu importu w bazie DEBT Manager (<code>import_typ.impt_id</code>).</span>
  </li>
  <li>
    <span class="param-name required">@import_type_name</span>
    <span class="param-type">varchar(500)</span>
    <span class="param-desc">Nazwa wyświetlana typu importu.</span>
  </li>
  <li>
    <span class="param-name required">@integration_import_type_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu importu w schemacie <code>dm_config</code> w bazie DEBT Manager.</span>
  </li>
  <li>
    <span class="param-name required">@dm_orginal_creditor_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kontrahenta (<code>kontrahent.ko_id</code>) — jeśli nowy, dodaj go wcześniej.</span>
  </li>
  <li>
    <span class="param-name required">@dm_portfolio_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela (<code>umowa_kontrahent.uko_id</code>) — jeśli nowy, dodaj go wcześniej.</span>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Rejestracja typu importu w DEBT Manager (schemat `dbo`)</div>

Poniższy insert dodaje nowy typ importu do tabeli `dbo.import_typ` w bazie DEBT Manager. Klasa `IntegrationsApiImport` oznacza, że import będzie obsługiwany przez API integracyjne (a nie przez wbudowany mechanizm DM).

```sql
INSERT INTO [dbo].[import_typ]
    ([impt_id], [impt_nazwa], [impt_class], [impt_isAsynchronous], [impt_is_webapi], [impt_uuid])
SELECT
     @dm_import_type_id
    ,@import_type_name
    ,'DebtManager.BL.DataIntegration.Core.Import.IntegrationsApiImport'
    ,1    -- asynchroniczny
    ,1    -- obsługiwany przez Web API
    ,NEWID()
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 3 — Konfiguracja typu importu (schemat `dm_config` w bazie DEBT Manager)</div>

Drugi insert rejestruje typ importu w schemacie `dm_config` w bazie DEBT Manager (`dm_config.import_type`). Klasy `ExternalRabbitMq*` oznaczają, że walidacja, transformacja i finalizacja są obsługiwane zewnętrznie przez system klienta (komunikacja przez RabbitMQ). Reguły walidacji powiązane z typem importu konfiguruje się oddzielnie w schemacie `dm_validations` (poza zakresem tego przewodnika).

```sql
INSERT INTO [dm_config].[import_type]
    ([id], [name], [validation_class_name], [transformation_class_name],
     [finalization_class_name], [dm_impt_id], [dm_ko_id], [dm_uko_id])
SELECT
     @integration_import_type_id
    ,@import_type_name
    ,'DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqValidator'
    ,'DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqTransformator'
    ,'DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqFinalizer'
    ,@dm_import_type_id
    ,@dm_orginal_creditor_id
    ,@dm_portfolio_id
```

!!! tip "Zewnętrzna obsługa kroków importu"
    Klasy `ExternalRabbitMqValidator`, `ExternalRabbitMqTransformator` i `ExternalRabbitMqFinalizer` delegują odpowiednio walidację, transformację i finalizację do zewnętrznego systemu klienta. Więcej o krokach importu w rozdziale [Procedura importów](../zalozenia/procedura-importow.md).

</div>
