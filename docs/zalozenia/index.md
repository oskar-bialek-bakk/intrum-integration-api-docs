---
title: "Założenia"
---

# Założenia

<div class="api-section" markdown>
<div class="api-section-title">Cel API integracyjnego</div>

System posiada API Integracyjne, którego celem jest umożliwienie przygotowywania importu danych kontrahentów — np. importowanie nowych spraw do obsługi.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Założenia działania importów danych</div>

<ol class="assumption-list">
  <li>System posiada API integracyjne, które udostępnia zestaw <strong>uniwersalnych i reużywalnych komunikatów</strong> pozwalających zasilić system — np. dodaj sprawę, dodaj wpłatę, dodaj adres dłużnika, dodaj akcję, dodaj sygnaturę.</li>
  <li>API integracyjne można zasilić <strong>bezpośrednio</strong> poprzez wywołanie usługi.</li>
  <li>API integracyjne <strong>kolejkuje</strong> przekazane komunikaty i przetwarza je wykorzystując tyle zasobów ile aktualnie posiada system (z możliwością zwiększenia mocy przetwarzania poprzez dodanie kolejnych maszyn konsumujących dane z kolejek).</li>
  <li>API integracyjne zasila system <strong>w trybie rzeczywistym</strong>, nie powodując zablokowania systemu DEBT Manager w trakcie zasilania.</li>
  <li>Dostawca przygotowuje i utrzymuje API integracyjne. Zasilanie API integracyjnego danymi może być wykonywane przez <strong>IT klienta</strong> z poziomu systemu pośredniczącego (Klient API). IT klienta może dodatkowo przejąć na siebie wykonanie etapu <strong>walidacji</strong> — zgłaszając zakończony etap przez <a href="../funkcje-api/importy/set-import-step/">SetImportStep</a>.</li>
</ol>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przegląd procesu importu</div>

!!! note "Granica zakresu dokumentacji"
    Niniejsza dokumentacja opisuje wyłącznie kanał komunikacji **Klient API → DEBT Manager**. To, w jaki sposób Klient API otrzymuje dane do wysłania (np. transformacja danych od kontrahenta, integracja z systemem zewnętrznym), jest poza zakresem tego dokumentu.

<div class="pipeline">
  <div class="pipeline-step">
    <span class="step-num">1</span>
    <span class="step-title">Pobranie tokenu</span>
    <span class="step-desc">OAuth 2.0 client credentials — patrz <a href="autoryzacja/">Autoryzacja</a>.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">2</span>
    <span class="step-title">Utworzenie importu</span>
    <span class="step-desc">Wywołanie <a href="../funkcje-api/importy/create-import/">CreateImport</a> i odebranie <code>importId</code>.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">3</span>
    <span class="step-title">Start fazy Adding</span>
    <span class="step-desc"><a href="../funkcje-api/importy/set-import-step/">SetImportStep</a> Stage=Adding, Status=InProgress.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">4</span>
    <span class="step-title">Wysłanie komunikatów</span>
    <span class="step-desc">Pętla <a href="../funkcje-api/importy/enqueue-import-message/">EnqueueImportMessage</a> per kolejka.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">5</span>
    <span class="step-title">Zamknięcie fazy</span>
    <span class="step-desc"><a href="../funkcje-api/importy/set-import-step/">SetImportStep</a> Stage=Adding, Status=Done (walidacja po stronie API) lub Stage=Validation, Status=Done (walidacja po stronie Klienta API).</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">6</span>
    <span class="step-title">Monitorowanie</span>
    <span class="step-desc"><a href="../funkcje-api/importy/get-import-status/">GetImportStatus</a> aż <code>IsFinished=true</code>.</span>
  </div>
</div>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Szczegóły konfiguracji</div>

Szczegóły procedury konfiguracji nowego importu oraz procedurę importowania danych od kontrahenta opisano w rozdziałach:

- [Konfiguracja nowego kontrahenta](konfiguracja-kontrahenta.md)
- [Procedura importów](procedura-importow.md)

</div>
