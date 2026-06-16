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

Drugi insert rejestruje typ importu w schemacie `dm_config` w bazie DEBT Manager (`dm_config.import_type`). Dla standardowego importu API-2-API obsługiwanego po stronie DM kolumny `validation_class_name`, `transformation_class_name` i `finalization_class_name` ustaw na pusty string `''` (kolumny są `NOT NULL`) — żaden z tych kroków nie wykonuje wtedy dedykowanej klasy etapu. Walidacja odbywa się przez reguły rejestrowane w [SetImportValidations](../funkcje-api/importy/set-import-validations.md), egzekwowane per komunikat na etapie Consumption.

```sql
INSERT INTO [dm_config].[import_type]
    ([id], [name], [validation_class_name], [transformation_class_name],
     [finalization_class_name], [dm_impt_id], [dm_ko_id], [dm_uko_id])
SELECT
     @integration_import_type_id
    ,@import_type_name
    ,''   -- validation_class_name: walidacja przez reguły (SetImportValidations) na etapie Consumption, bez klasy etapu
    ,''   -- transformation_class_name: brak transformacji (komunikaty już w docelowym formacie)
    ,''   -- finalization_class_name: brak finalizatora
    ,@dm_import_type_id
    ,@dm_orginal_creditor_id
    ,@dm_portfolio_id
```

!!! tip "Kroki walidacja / transformacja / finalizacja"
    Pusty string `''` w tych kolumnach (są `NOT NULL`) oznacza, że dany krok nie uruchamia żadnej klasy (przechodzi dalej). Walidacja danych dla importu API-2-API odbywa się przez reguły rejestrowane w [SetImportValidations](../funkcje-api/importy/set-import-validations.md) i egzekwowane per komunikat na etapie Consumption (brama walidacyjna, konfiguracja `IntegrationsAPI:Validation:UseSharedCatalog = true`). Klasa walidatora/transformatora/finalizatora etapu jest potrzebna tylko dla importów z dedykowaną logiką po stronie DM. Więcej o krokach importu w rozdziale [Procedura importów](../zalozenia/procedura-importow.md).

</div>
