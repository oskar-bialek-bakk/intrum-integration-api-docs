---
title: "Kolejki techniczne"
---

# Kolejki techniczne

!!! warning "Czego dotyczy ten rozdział"
    Rozdział opisuje **techniczne kolejki** RabbitMQ oraz SQL Server używane wewnętrznie przez DEBT Manager i systemy współpracujące do orkiestracji walidacji, finalizacji i konsumpcji komunikatów importu.

    To **nie jest** lista wartości pola `queueName` używanego w endpointach API ([EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md), [GetObjectStatusByID](../funkcje-api/importy/get-object-status-by-id.md)). Wartość `queueName` to nazwa komunikatu (np. `Case`, `Customer`) — pełny wykaz: [Komunikaty → Wykaz komunikatów](../komunikaty/index.md#wykaz-komunikatow).

<div class="api-section" markdown>
<div class="api-section-title">Rola kolejek technicznych w API</div>

W ramach API integracyjnego wykorzystywane są kolejki techniczne, które pełnią następującą rolę:

- **Asynchroniczne przetwarzanie** — zapewniają asynchroniczne przetwarzanie danych.
- **Delegacja przetwarzania** — pozwalają na przeniesienie wybranych elementów przetwarzania danych do systemów zewnętrznych w stosunku do API, np. walidacja importu może zostać wykonana przez zewnętrzny system, który pobiera komunikat o konieczności walidacji z kolejki, wykonuje walidację i informuje API o wyniku.

</div>

---

<div class="api-section">
<div class="api-section-title">Wykaz kolejek technicznych</div>

<details class="collapsible-fields" open>
<summary>Kolejki RabbitMQ — walidacja</summary>
<ul class="param-list">
  <li>
    <span class="param-name">DebtImportQueue_ImportValidation</span>
    <span class="param-type">RabbitMQ</span>
    <span class="param-desc">Kolejka zawierająca importy dla których należy wykonać krok walidacji danych. Konsumowana przez <strong>system DEBT Manager</strong>.</span>
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
    <span class="param-desc">Dla każdego komunikatu istnieje w bazie <strong>DEBT Manager</strong>, w schemacie <code>dm_messages</code>, tabela pełniąca rolę kolejki danego komunikatu (np. <code>dm_messages.case_details</code> dla komunikatu <code>Case</code>). To właśnie do tych kolejek trafiają komunikaty wysłane przez <a href="../funkcje-api/importy/enqueue-import-message/">EnqueueImportMessage</a> — wartość pola <code>queueName</code> = nazwa komunikatu. Konsumowane przez <strong>system DEBT Manager</strong>. Pełna lista nazw komunikatów: <a href="../komunikaty/#wykaz-komunikatow">Wykaz komunikatów</a>. Szczegółowy opis zastosowanego rozwiązania: <a href="https://www.mssqltips.com/sqlservertip/1155/sql-server-database-specific-settings-service-broker/">Service Broker pattern</a>.</span>
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
