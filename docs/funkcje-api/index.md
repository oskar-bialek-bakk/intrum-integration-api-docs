---
title: "Funkcje API"
---

# Funkcje API

<div class="api-section" markdown>
<div class="api-section-title">Przegląd</div>

Niniejszy rozdział zawiera opis funkcji API wykorzystywanych do przeprowadzania importów/eksportów danych dla systemu DEBT Manager.

</div>

---

<div class="api-section">
<div class="api-section-title">Importy</div>

Grupa endpointów odpowiedzialna za cały cykl życia importu danych — od przesłania pliku, przez przetwarzanie komunikatów, po monitorowanie statusu i pobieranie wyników.

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/import-from-file-upload/">ImportFromFileUpload</a></span>
  </div>
  <p class="role-desc">Import danych z pliku przesłanego przez HTTP.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/import-from-file-share/">ImportFromFileShare</a></span>
  </div>
  <p class="role-desc">Import danych z pliku na udziale sieciowym.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/enque-import-message/">EnqueImportMessage</a></span>
  </div>
  <p class="role-desc">Zakolejkowanie pojedynczego komunikatu importowego.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-import-status/">GetImportStatus</a></span>
  </div>
  <p class="role-desc">Pobranie statusu importu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-object-status-by-id/">GetObjectStatusByID</a></span>
  </div>
  <p class="role-desc">Pobranie statusu obiektu po identyfikatorze.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-import-file/">GetImportFile</a></span>
  </div>
  <p class="role-desc">Pobranie pliku importowego (HTTP).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-import-file-from-file-share/">GetImportFileFromFileShare</a></span>
  </div>
  <p class="role-desc">Pobranie pliku importowego z udziału sieciowego.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/set-import-step/">SetImportStep</a></span>
  </div>
  <p class="role-desc">Ustawienie etapu przetwarzania importu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge post">POST</span>
    <span class="role-name"><a href="importy/set-import-validations/">SetImportValidations</a></span>
  </div>
  <p class="role-desc">Konfiguracja walidacji dla importu.</p>
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
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-mail-recipients/">GetMailRecipients</a></span>
  </div>
  <p class="role-desc">Pobranie listy odbiorców e-mail.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="method-badge get">GET</span>
    <span class="role-name"><a href="importy/get-mail-recipients-for-import-type/">GetMailRecipientsForImportType</a></span>
  </div>
  <p class="role-desc">Pobranie odbiorców e-mail dla typu importu.</p>
</div>

</div>
