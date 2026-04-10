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
  <p class="role-desc">Przekazuje dla IT mapowania w jaki sposób pliki importowe mają mapować się na odpowiednie pola w aplikacji DM — np. który adres z pliku importowego to adres zameldowania, jakie są reguły walidacji pliku itd.</p>
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
    <span class="role-name">Konfiguruje VPN/SFTP dostępy</span>
  </div>
  <p class="role-desc">IT konfiguruje z kontrahentem VPN oraz dostępy SFTP wykorzystywane do wymiany plikowej.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">Importy: Konfiguruje mechanizm pobierania danych</span>
  </div>
  <p class="role-desc">IT konfiguruje mechanizm, który pobierze pliki od kontrahenta i przeniesie je do Klienta.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">Importy: Implementuje mapowanie portfela</span>
  </div>
  <p class="role-desc">Na podstawie mapowań dostarczonych od biznesu, IT implementuje mechanizm transformujący dane z plików kontrahenta na komunikaty DM.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-it">IT</span>
    <span class="role-name">Eksporty: Konfiguruje mechanizm dostarczania danych</span>
  </div>
  <p class="role-desc">Na podstawie specyfikacji kontrahenta, IT implementuje mechanizm formatujący pliki eksportów wystawione przez DM do oczekiwanego formatu oraz konfiguruje transport plików na udziały sieciowe kontrahenta.</p>
</div>

</div>
