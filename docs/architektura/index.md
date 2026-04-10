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
  <p class="role-desc">Usługa API Integracyjnego (opisana w rozdziale <a href="../funkcje-api/">Funkcje API</a>) odpowiedzialna za obsługę importu i jego statuowanie. Hostowana na IIS.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-app">Application Server</span>
    <span class="role-name">API staging database</span>
  </div>
  <p class="role-desc">Baza danych <code>dm_integration_[klient]</code> wykorzystywana do przetwarzania danych zanim finalnie zostaną zaimportowane do bazy DEBT Manager.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-queue">Queue Server</span>
    <span class="role-name">RabbitMQ</span>
  </div>
  <p class="role-desc">Serwer kolejek RabbitMQ zapewniający asynchroniczną komunikację w API (opisane w rozdziale <a href="../kolejki/">Kolejki</a>).</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-worker">Worker Servers</span>
    <span class="role-name">Windows Worker Services (1..n)</span>
  </div>
  <p class="role-desc">Usługi serwisów Windows pełniące role konsumentów wiadomości wpadających na serwer kolejek RabbitMQ. Skalowalne horyzontalnie — dodanie kolejnych maszyn zwiększa moc przetwarzania.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-owner role-arch-dm">DM Server</span>
    <span class="role-name">DEBT Manager database</span>
  </div>
  <p class="role-desc">Baza danych <code>dm_data_dmweb_[klient]</code> systemu DEBT Manager, do której importowane są dane.</p>
</div>

</div>
