---
title: "Kolejki"
---

# Kolejki

<div class="api-section" markdown>
<div class="api-section-title">Rola kolejek w API</div>

W ramach API integracyjnego wykorzystywane są kolejki danych, które pełnią następującą rolę:

- **Asynchroniczne przetwarzanie** — zapewniają asynchroniczne przetwarzanie danych.
- **Delegacja przetwarzania** — pozwalają na przeniesienie wybranych elementów przetwarzania danych do systemów zewnętrznych w stosunku do API, np. walidacja importu może zostać wykonana przez zewnętrzny system, który pobiera komunikat o konieczności walidacji z kolejki, wykonuje walidację i informuje API o wyniku.

</div>

---

<div class="api-section">
<div class="api-section-title">Wykaz kolejek</div>

<details class="collapsible-fields" open>
<summary>Kolejki RabbitMQ — walidacja</summary>
<ul class="param-list">
  <li>
    <span class="param-name">DebtImportQueue_ImportValidation</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Kolejka zawierająca importy dla których należy wykonać krok walidacji danych. Konsumowana przez <strong>system DEBT Manager</strong>.</span>
  </li>
  <li>
    <span class="param-name">ExternalImportValidation</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Jak wyżej — wersja konsumowana przez <strong>system zewnętrzny</strong>.<br><code>DebtManager.BL.DM.ExternalMessages.ExternalImportValidation</code></span>
  </li>
</ul>
</details>

<details class="collapsible-fields" open>
<summary>Kolejki RabbitMQ — transformacja</summary>
<ul class="param-list">
  <li>
    <span class="param-name">DebtImportQueue_ImportTransformation</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Kolejka zawierająca importy dla których należy wykonać krok transformacji danych z importu na komunikaty. Konsumowana przez <strong>system DEBT Manager</strong>.</span>
  </li>
  <li>
    <span class="param-name">ExternalImportTransformation</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Jak wyżej — wersja konsumowana przez <strong>system zewnętrzny</strong>.<br><code>DebtManager.BL.DM.ExternalMessages.ExternalImportTransformation</code></span>
  </li>
</ul>
</details>

<details class="collapsible-fields" open>
<summary>Kolejki RabbitMQ — finalizacja</summary>
<ul class="param-list">
  <li>
    <span class="param-name">DebtImportQueue_ImportFinalization</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Kolejka zawierająca importy które zostały zakończone, dająca możliwość wykonania kolejnych kroków po imporcie — np. uruchomienie workflowów na zaimportowanych sprawach (sukces) lub usunięcie danych (błąd). Konsumowana przez <strong>system DEBT Manager</strong>.</span>
  </li>
  <li>
    <span class="param-name">ExternalImportFinalizer</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Jak wyżej — wersja konsumowana przez <strong>system zewnętrzny</strong>.<br><code>DebtManager.BL.DM.ExternalMessages.ExternalImportFinalizer</code></span>
  </li>
</ul>
</details>

<details class="collapsible-fields" open>
<summary>Kolejki SQL Server — komunikaty</summary>
<ul class="param-list">
  <li>
    <span class="param-name">dm_messages.[nazwa_komunikatu]</span>
    <span class="param-type">SQL Server</span>
    <span class="param-desc">Dla każdego komunikatu istnieje w bazie API Integracyjnego (<code>dm_integration_[nazwa]</code>, schemat <code>dm_messages</code>) tabela pełniąca rolę kolejki danego komunikatu. Konsumowana przez <strong>system DEBT Manager</strong>. Szczegółowy opis zastosowanego rozwiązania: <a href="https://www.mssqltips.com/sqlservertip/1155/sql-server-database-specific-settings-service-broker/">Service Broker pattern</a>.</span>
  </li>
</ul>
</details>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przykład stanu kolejek</div>

Poniższy zrzut prezentuje status kolejek API integracyjnego w czasie gdy wykonujemy typ importu obsługiwany przez system zewnętrzny i jesteśmy w trakcie ostatniego kroku — finalizacji importu:

![Przykład stanu kolejek](../images/kolejki-status.png)

</div>
