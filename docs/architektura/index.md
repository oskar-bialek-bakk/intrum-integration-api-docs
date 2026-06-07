---
title: "Architektura fizyczna"
---

# Architektura fizyczna

<div class="api-section" markdown>
<div class="api-section-title">Diagram architektury</div>

![](../diagrams/architektura-systemu.drawio)

</div>

---

<div class="api-section">
<div class="api-section-title">Opis komponentów</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-client">Client</span>
    <span class="role-name">External system</span>
  </div>
  <p class="role-desc">Zewnętrzny system lub usługa przekazująca żądanie importu do API.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-app">Application Server</span>
    <span class="role-name">Integrations API (ASP.NET WebAPI)</span>
  </div>
  <p class="role-desc">Usługa API Integracyjnego (opisana w rozdziale <a href="../funkcje-api/">Funkcje API</a>) odpowiedzialna za obsługę importu i jego statuowanie. Hostowana jako <strong>Azure App Service (Web App)</strong>.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-dm">DM Server</span>
    <span class="role-name">Schematy integracyjne w bazie DEBT Manager</span>
  </div>
  <p class="role-desc">Dane pomocnicze API integracyjnego (rekordy importów, komunikaty, reguły walidacji, konfiguracja typów importów) znajdują się w bazie <strong>DEBT Manager</strong> w wydzielonych schematach integracyjnych:</p>
  <ul class="param-list">
    <li>
      <span class="param-name"><code>dm_config</code></span>
      <span class="param-desc">Konfiguracja typów importów, słowniki walidacyjne, mapowania, ustawienia kontrahenta.</span>
    </li>
    <li>
      <span class="param-name"><code>dm_imports</code></span>
      <span class="param-desc">Rekordy importów — cykl życia, etapy, statusy, historia kroków.</span>
    </li>
    <li>
      <span class="param-name"><code>dm_messages</code></span>
      <span class="param-desc">Komunikaty oczekujące na konsumpcję oraz historia przetwarzania.</span>
    </li>
    <li>
      <span class="param-name"><code>dm_validations</code></span>
      <span class="param-desc">Reguły walidacyjne aktywne w ramach importów (powiązane z <a href="../funkcje-api/importy/set-import-validations/">SetImportValidations</a>).</span>
    </li>
  </ul>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-queue">Queue Server</span>
    <span class="role-name">RabbitMQ</span>
  </div>
  <p class="role-desc">Serwer kolejek RabbitMQ zapewniający asynchroniczną komunikację w API — orkiestracja walidacji i finalizacji importów (szczegóły: <a href="../kolejki/">Kolejki techniczne</a>).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-worker">Worker Servers</span>
    <span class="role-name">Azure WebJobs (1..n)</span>
  </div>
  <p class="role-desc">Procesy <strong>Azure WebJobs</strong> hostowane wraz z Web App, pełniące rolę konsumentów wiadomości z serwera kolejek RabbitMQ. Skalowalne horyzontalnie — zwiększenie liczby instancji Web App zwiększa moc przetwarzania.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-dm">DM Server</span>
    <span class="role-name">DEBT Manager database</span>
  </div>
  <p class="role-desc">Baza danych <code>dm_data_dmweb_intrum</code> systemu DEBT Manager, do której importowane są dane.</p>
</div>

</div>
