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
  <li>Dostawca przygotowuje i utrzymuje API integracyjne, ale zasilanie danymi API integracyjnego może być wykonywane przez <strong>IT klienta</strong>. Dodatkowo IT klienta może przejąć na siebie wykonanie poszczególnych kroków importu: <strong>walidację</strong>, <strong>transformację</strong> i <strong>finalizację</strong>.</li>
</ol>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przegląd procesu importu</div>

<div class="pipeline">
  <div class="pipeline-step">
    <span class="step-num">1</span>
    <span class="step-title">Pobranie danych</span>
    <span class="step-desc">Pobranie danych z SFTP, e-mail lub zewnętrznego systemu.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">2</span>
    <span class="step-title">Wywołanie API</span>
    <span class="step-desc">Wywołanie funkcji API rozpoczynającej import.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">3</span>
    <span class="step-title">Walidacja danych</span>
    <span class="step-desc">Walidacja danych do importu, odrzucenie w przypadku błędów.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">4</span>
    <span class="step-title">Transformacja</span>
    <span class="step-desc">Transformacja danych na komunikaty zrozumiałe przez API.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">5</span>
    <span class="step-title">Import do bazy DM</span>
    <span class="step-desc">Zaimportowanie danych z komunikatów do bazy DEBT Manager.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">6</span>
    <span class="step-title">Finalizacja</span>
    <span class="step-desc">E-mail o sukcesie, zmiana statusu w sys. zewnętrznym.</span>
  </div>
</div>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Szczegóły konfiguracji</div>

Szczegóły procedury konfiguracji nowego importu oraz procedurę importowania danych od kontrahenta opisano w rozdziałach:

- [Konfiguracja nowego kontrahenta](konfiguracja-kontrahenta.md)
- [Procedura importów danych od kontrahenta](procedura-importow.md)

</div>
