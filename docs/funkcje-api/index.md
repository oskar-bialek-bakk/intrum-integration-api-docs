---
title: "Funkcje API"
---

# Funkcje API

<div class="api-section" markdown>
<div class="api-section-title">Przegląd</div>

Rozdział zawiera opis endpointów API integracyjnego DEBT Manager wykorzystywanych w trybie API-2-API. Endpointy są przedstawione w naturalnej kolejności użycia w cyklu życia importu — od utworzenia importu po monitorowanie finalizacji.

</div>

---

<div class="api-section">
<div class="api-section-title">Importy</div>

Grupa endpointów odpowiedzialna za cały cykl życia importu — od utworzenia rekordu importu, przez sterowanie etapami, wysłanie komunikatów, po monitorowanie statusu.

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/create-import/">CreateImport</a></span>
  </div>
  <p class="role-desc">Tworzy nowy rekord importu i zwraca <code>importId</code>.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/set-import-step/">SetImportStep</a></span>
  </div>
  <p class="role-desc">Sterowanie cyklem życia importu — zgłoszenie startu/zakończenia etapu Adding lub Validation.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-dictionaries/">GetDictionaries</a></span>
  </div>
  <p class="role-desc">Pobranie słowników walidacyjnych.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/refresh-dictionaries/">RefreshDictionaries</a></span>
  </div>
  <p class="role-desc">Odświeżenie cache słowników.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/set-import-validations/">SetImportValidations</a></span>
  </div>
  <p class="role-desc">Konfiguracja reguł walidacyjnych dla importu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/enqueue-import-message/">EnqueueImportMessage</a></span>
  </div>
  <p class="role-desc">Wysłanie pojedynczego komunikatu (lub batcha) w kontekście importu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-import-status/">GetImportStatus</a></span>
  </div>
  <p class="role-desc">Pobranie statusu importu — bieżący krok, historia, opcjonalnie statusy komunikatów.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-object-status-by-id/">GetObjectStatusByID</a></span>
  </div>
  <p class="role-desc">Status pojedynczego obiektu po identyfikatorze.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-mail-recipients/">GetMailRecipients</a></span>
  </div>
  <p class="role-desc">Lista odbiorców e-mail finalizacji importu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-mail-recipients-for-import-type/">GetMailRecipientsForImportType</a></span>
  </div>
  <p class="role-desc">Lista odbiorców e-mail dla typu importu.</p>
</div>

</div>
