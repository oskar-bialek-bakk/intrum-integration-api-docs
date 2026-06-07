---
title: "Procedura importów"
---

# Procedura importów

<div class="api-section" markdown>
<div class="api-section-title">Aktorzy</div>

W procedurze importu występuje jeden główny aktor techniczny: **Klient API** — system pośredniczący po stronie klienta wdrożeniowego, który wywołuje endpointy API integracyjnego. Klient API jest implementowany i utrzymywany przez IT klienta.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Diagram procesu</div>

![](../diagrams/procedura-importow.drawio)

</div>

---

<div class="api-section">
<div class="api-section-title">Kroki importu</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-kontrahent">Poza zakresem</span>
    <span class="role-name">Dostarczenie danych źródłowych</span>
  </div>
  <p class="role-desc">Klient API otrzymuje dane do wysłania od dowolnego źródła (kontrahent, system zewnętrzny, plik wewnętrzny). Sposób dostarczenia danych do Klienta API jest poza zakresem tej dokumentacji.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">1. Pobranie tokenu</span>
  </div>
  <p class="role-desc">Klient API pobiera token OAuth (client credentials) — szczegóły w rozdziale <a href="../autoryzacja/">Autoryzacja</a>.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">2. Utworzenie importu</span>
  </div>
  <p class="role-desc">Klient API woła <a href="../../funkcje-api/importy/create-import/">CreateImport</a> z parametrami typu importu, kontrahenta i portfela. W odpowiedzi otrzymuje <code>importId</code> — identyfikator wykorzystywany we wszystkich kolejnych krokach.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">3. Zgłoszenie startu fazy Adding</span>
  </div>
  <p class="role-desc">Klient API woła <a href="../../funkcje-api/importy/set-import-step/">SetImportStep</a> z <code>Stage=1 (Adding)</code> i <code>StageStatus=1 (InProgress)</code>.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">4. (Opcjonalnie) Słowniki i reguły walidacji</span>
  </div>
  <p class="role-desc">Klient API może pobrać słowniki walidacyjne (<a href="../../funkcje-api/importy/get-dictionaries/">GetDictionaries</a> / <a href="../../funkcje-api/importy/refresh-dictionaries/">RefreshDictionaries</a>) i ustawić reguły walidacji egzekwowane przez API (<a href="../../funkcje-api/importy/set-import-validations/">SetImportValidations</a>).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">5. Wysłanie komunikatów</span>
  </div>
  <p class="role-desc">Klient API wykonuje pętlę wywołań <a href="../../funkcje-api/importy/enqueue-import-message/">EnqueueImportMessage</a> — po jednym wywołaniu na typ komunikatu (Customer, Case, Contract, Payment, …), z możliwością batchowania wielu obiektów w jednym wywołaniu.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">6. Zamknięcie fazy</span>
  </div>
  <p class="role-desc"><strong>Walidacja po stronie API:</strong> SetImportStep <code>Stage=1 (Adding)</code>, <code>StageStatus=2 (Done)</code>. API podejmie Stage 3 Validation.<br>
  <strong>Walidacja po stronie Klienta API:</strong> SetImportStep <code>Stage=3 (Validation)</code>, <code>StageStatus=2 (Done)</code> — API pomija własną walidację i przechodzi do Consumption.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-app">API</span>
    <span class="role-name">7. Validation → Consumption → Finishing</span>
  </div>
  <p class="role-desc">API automatycznie przechodzi przez kolejne etapy: walidacja (jeśli nie przejęta), konsumpcja komunikatów z kolejek przez DM, finalizacja (raport, e-mail do odbiorców z <a href="../../funkcje-api/importy/get-mail-recipients/">GetMailRecipients</a>).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">Klient API</span>
    <span class="role-name">8. Monitorowanie statusu</span>
  </div>
  <p class="role-desc">Po wywołaniu kroku 6 (zamknięcie fazy) Klient API polluje <a href="../../funkcje-api/importy/get-import-status/">GetImportStatus</a> w trakcie automatycznego przetwarzania (etap 7) aż <code>IsFinished=true</code>. Pojedyncze komunikaty można sprawdzić przez <a href="../../funkcje-api/importy/get-object-status-by-id/">GetObjectStatusByID</a>.</p>
</div>

</div>
