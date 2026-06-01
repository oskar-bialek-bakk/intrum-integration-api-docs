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

Przed wywołaniem metody API wymagane jest uzyskanie tokenu autoryzacyjnego (OAuth 2.0 client credentials). Pełna procedura — wraz z przykładem `curl`, ustawieniem w Postman i logowaniem w Swagger UI — opisana jest w rozdziale [Autoryzacja](../zalozenia/autoryzacja.md).

W skrócie:

1. `POST` na `https://dmlogin-intrum-dev.groupad1.com/connect/token` z `client_id=integration`, `client_secret={CLIENT_SECRET}`, `scope=api1`, `grant_type=client_credentials`.
2. Z odpowiedzi pobierz `access_token` i przekazuj go w nagłówku `Authorization: Bearer <access_token>` przy każdym wywołaniu API integracyjnego.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Wywołanie EnqueueImportMessage</div>

Wywołaj funkcję [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md) z następującymi parametrami:

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
