---
title: "Procedura importów danych od kontrahenta"
---

# Procedura importów danych od kontrahenta

<div class="api-section" markdown>
<div class="api-section-title">Diagram procesu</div>

Import danych od kontrahenta składa się z następujących kroków, które bardziej szczegółowo prezentuje poniższy diagram:

![](../diagrams/procedura-importow.drawio)

</div>

---

<div class="api-section">
<div class="api-section-title">Kroki importu</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-kontrahent">Kontrahent</span>
    <span class="role-name">1. Wystawienie danych</span>
  </div>
  <p class="role-desc">Kontrahent wystawia dane na udział sieciowy.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">2. Monitorowanie i pobranie pliku</span>
  </div>
  <p class="role-desc">IT monitoruje udział sieciowy i pobiera plik od kontrahenta.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">3. Import pliku przez API</span>
  </div>
  <p class="role-desc">IT importuje plik przy użyciu API — techniczna specyfikacja tego kroku znajduje się w rozdziale <a href="../funkcje-api/importy/">Funkcje API &rarr; Importy</a>.</p>
</div>

</div>
