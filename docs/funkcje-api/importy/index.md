---
title: "Importy — przegląd"
---

# Importy — przegląd

<div class="api-section" markdown>
<div class="api-section-title">Cykl życia importu</div>

W trybie API-2-API import przechodzi przez 4 etapy. Klient API steruje pierwszym etapem (Adding) i opcjonalnie trzecim (Validation, jeśli waliduje dane u siebie); API odpowiada za pozostałe.

<div class="pipeline">
  <div class="pipeline-step">
    <span class="step-num">1</span>
    <span class="step-title">Adding</span>
    <span class="step-desc">Klient API wysyła komunikaty (EnqueueImportMessage).</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">3</span>
    <span class="step-title">Validation</span>
    <span class="step-desc">Walidacja danych — wykonywana przez API (reguły z SetImportValidations) lub przez Klienta API u siebie.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">4</span>
    <span class="step-title">Consumption</span>
    <span class="step-desc">DM konsumuje komunikaty z kolejek <code>dm_messages.*</code> w SQL Server.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">5</span>
    <span class="step-title">Finishing</span>
    <span class="step-desc">Zamknięcie importu, raport, e-mail (GetMailRecipients).</span>
  </div>
</div>

!!! note "Stage 2 Transformation"
    Wartość `Stage=2 (Transformation)` zachowana w schemacie API ze względów historycznych. W trybie API-2-API nie jest używana — komunikaty są już dostarczane przez Klienta API w docelowym formacie.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pełny przepływ</div>

Pełny tutorial pierwszego importu — patrz [Przypadki użycia → Pierwszy import](../../przypadki-uzycia/pierwszy-import.md).

Lista endpointów z metodami HTTP — patrz [Funkcje API](../index.md).

</div>
