---
title: "Zasilenie bez pliku"
---

# Zasilenie bez pliku

<div class="api-section" markdown>
<div class="api-section-title">Opis</div>

Przewodnik pokazuje jak zasilić system DEBT Manager pojedynczym komunikatem importowym **bez przygotowania pliku** — bezpośrednio przez wywołanie API.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 1 — Autoryzacja</div>

Przed wywołaniem metody API wymagane jest uzyskanie tokenu autoryzacyjnego. Metody wymagające autoryzacji są oznaczone w Swaggerze ikoną kłódki.

1. Otwórz Swagger UI na swoim serwerze: `http://[adres_serwera]:39451/swagger`
2. Kliknij przycisk **Authorize** i wybierz opcję **api1**.
3. Po poprawnym zalogowaniu ikona przy metodzie zmieni się na zamkniętą kłódkę.

!!! tip "Jednorazowa autoryzacja"
    Logowanie wystarczy wykonać raz na sesję — token jest zapamiętywany dla kolejnych wywołań.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Wywołanie EnqueImportMessage</div>

Wywołaj funkcję [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md) z następującymi parametrami:

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">GUID</span>
    <span class="param-desc">Pusty GUID (<code>00000000-0000-0000-0000-000000000000</code>) — import bez pliku.</span>
  </li>
  <li>
    <span class="param-name required">queueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki, np. <code>Case</code>, <code>Payment</code> — zgodna z wykazem z rozdziału <a href="../komunikaty/index.md">Komunikaty</a>.</span>
  </li>
  <li>
    <span class="param-name required">message</span>
    <span class="param-type">array</span>
    <span class="param-desc">Lista komunikatów JSON — przykłady składni w rozdziale <a href="../komunikaty/index.md">Komunikaty</a>.</span>
  </li>
</ul>

!!! example "Przykład wywołania"
    ```json
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Case",
      "message": [
        {
          "MatchKey": "CASE-001",
          "MatchKeyType": 2
        }
      ]
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 3 — Weryfikacja wyniku</div>

1. Po wysłaniu żądania w odpowiedzi otrzymasz wynik importu.
2. Poczekaj na zakończenie **asynchronicznego przetwarzania** — status importu możesz monitorować funkcją [GetImportStatus](../funkcje-api/importy/get-import-status.md).
3. Sprawdź zmienione dane w aplikacji DEBT Manager.

</div>
