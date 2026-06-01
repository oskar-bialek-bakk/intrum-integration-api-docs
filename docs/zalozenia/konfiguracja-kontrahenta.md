---
title: "Konfiguracja nowego kontrahenta"
---

# Konfiguracja nowego kontrahenta

<div class="api-section" markdown>
<div class="api-section-title">Diagram procesu</div>

Zadanie skonfigurowania nowego kontrahenta wiąże się z krokami, które opisuje poniższy diagram:

![](../diagrams/konfiguracja-kontrahenta.drawio)

</div>

---

<div class="api-section">
<div class="api-section-title">Kroki konfiguracji</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-kontrahent">Kontrahent</span>
    <span class="role-name">Przekazuje opis portfela</span>
  </div>
  <p class="role-desc">Kontrahent przekazuje dokumentację nowego portfela: specyfikacje plików importowych oraz przykłady plików, specyfikacje plików eksportowych, wytyczne biznesowe do obsługi — np. czy można kapitalizować odsetki, sposób rozliczenia wpłat.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-biznes">Biznes</span>
    <span class="role-name">Rejestruje kontrahenta w DM</span>
  </div>
  <p class="role-desc">W ramach aplikacji DM użytkownik dodaje sprawę kontrahenta oraz sprawę handlową (umowę dla kontrahenta).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-biznes">Biznes</span>
    <span class="role-name">Konfiguruje warunki umowy</span>
  </div>
  <p class="role-desc">W ramach aplikacji DM użytkownik w sprawie handlowej przygotowuje warunki umowy — parametry umowy wykorzystywane do pracy operacyjnej na przekazanych sprawach, np. stawki prowizyjne, warunki windykacji (kapitalizacja odsetek, ugody itp.).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-biznes">Biznes</span>
    <span class="role-name">Konfiguruje atrybuty kontrahenta</span>
  </div>
  <p class="role-desc">W ramach aplikacji DM użytkownik konfiguruje dodatkowe pola opisujące kontrahenta lub umowę, które nie wpływają na operacyjną pracę systemu — np. dane wykorzystywane w raportowaniu, budżetowaniu itp.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-analityk">Biznes / Analityk</span>
    <span class="role-name">Przekazuje mapowania portfela</span>
  </div>
  <p class="role-desc">Przekazuje dla IT mapowania w jaki sposób dane źródłowe (otrzymane od Kontrahenta) mają mapować się na pola w aplikacji DM — np. który adres ze zbioru danych to adres zameldowania, jakie są reguły walidacji danych itd.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-analityk">Biznes / Analityk</span>
    <span class="role-name">Przygotowuje raporty zwrotne do kontrahenta</span>
  </div>
  <p class="role-desc">Przygotowuje w ramach modułu Raportów BI raporty, które wydobywają dane potrzebne do zasilenia plików eksportowych.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">Konfiguruje klienta API</span>
  </div>
  <p class="role-desc">IT konfiguruje credentials OAuth (Client ID i Client Secret), przekazane przez dostawcę API integracyjnego. Implementuje wywołania endpointów: <a href="../../funkcje-api/importy/create-import/">CreateImport</a>, <a href="../../funkcje-api/importy/set-import-step/">SetImportStep</a>, <a href="../../funkcje-api/importy/enqueue-import-message/">EnqueueImportMessage</a>, <a href="../../funkcje-api/importy/get-import-status/">GetImportStatus</a>.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">Importy: Implementuje mapowanie portfela</span>
  </div>
  <p class="role-desc">Na podstawie mapowań dostarczonych od biznesu, IT implementuje w systemie Klient API transformację danych źródłowych (otrzymanych od Kontrahenta dowolnym kanałem) na komunikaty API rozumiane przez DEBT Manager.</p>
</div>

</div>
