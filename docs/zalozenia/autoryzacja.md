---
title: "Autoryzacja"
---

# Autoryzacja

<div class="api-section" markdown>
<div class="api-section-title">Model autoryzacji</div>

API integracyjne DEBT Manager używa standardu **OAuth 2.0 Client Credentials**. Każde wywołanie chronionego endpointu wymaga nagłówka `Authorization: Bearer <access_token>`, gdzie `access_token` jest jednorazowo pobierany z serwera autoryzacyjnego DM Login.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Endpoint serwera autoryzacji</div>

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://dmlogin-intrum-dev.groupad1.com/connect/token</div>
</div>

**Content-Type:** `application/x-www-form-urlencoded`

**Body — parametry formularza:**

<ul class="param-list">
  <li>
    <span class="param-name required">grant_type</span>
    <span class="param-type">string</span>
    <span class="param-desc">Stała wartość <code>client_credentials</code>.</span>
  </li>
  <li>
    <span class="param-name required">scope</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zakres uprawnień. Dla API integracyjnego: <code>api1</code>.</span>
  </li>
  <li>
    <span class="param-name required">client_id</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator klienta integracyjnego. Domyślnie: <code>integration</code>.</span>
  </li>
  <li>
    <span class="param-name required">client_secret</span>
    <span class="param-type">string</span>
    <span class="param-desc">Sekret klienta integracyjnego — przekazywany przez zespół wdrożeniowy.</span>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pobranie tokenu — curl</div>

```bash title="Pobranie access_token"
curl -X POST "https://dmlogin-intrum-dev.groupad1.com/connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "scope=api1" \
  -d "client_id=integration" \
  -d "client_secret={CLIENT_SECRET}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Odpowiedź serwera autoryzacji</div>

**Status:** `200 OK`

```json title="Przykładowa odpowiedź"
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjBFOD...",
  "expires_in": 3600,
  "token_type": "Bearer",
  "scope": "api1"
}
```

<ul class="param-list">
  <li>
    <span class="param-name">access_token</span>
    <span class="param-type">string (JWT)</span>
    <span class="param-desc">Token dostępu do dołączenia w nagłówku <code>Authorization</code>.</span>
  </li>
  <li>
    <span class="param-name">expires_in</span>
    <span class="param-type">int</span>
    <span class="param-desc">Czas życia tokenu w sekundach. Po upływie tego okresu należy pobrać nowy token.</span>
  </li>
  <li>
    <span class="param-name">token_type</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zawsze <code>Bearer</code>.</span>
  </li>
  <li>
    <span class="param-name">scope</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zakres uprawnień, jaki przyznano tokenowi (np. <code>api1</code>).</span>
  </li>
</ul>

!!! tip "Cache tokenu"
    Token można reużywać do upływu `expires_in`. Zalecane: cache w kliencie integracyjnym (np. w pamięci procesu) z odświeżeniem ~60 s przed wygaśnięciem, zamiast pobierania tokenu przed każdym wywołaniem API.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Użycie tokenu w wywołaniach API</div>

Token przekazujemy w nagłówku `Authorization` każdego wywołania API integracyjnego:

```bash title="Przykład wywołania endpointu z tokenem"
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage" \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjBFOD..." \
  -H "Content-Type: application/json" \
  -d '{ "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c", "queueName": "Customer", "message": "..." }'
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Autoryzacja w Postman</div>

1. Nowe żądanie typu **POST** na adres `https://dmlogin-intrum-dev.groupad1.com/connect/token`.
2. Zakładka **Body** → wybierz **x-www-form-urlencoded**.
3. Dodaj klucze: `scope=api1`, `client_id=integration`, `client_secret={CLIENT_SECRET}`, `grant_type=client_credentials`.
4. **Send** → w odpowiedzi otrzymasz `access_token` oraz `expires_in`.
5. W kolejnych requestach do API integracyjnego ustaw zakładkę **Authorization** → **Bearer Token** → wklej `access_token`.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Autoryzacja w Swagger UI</div>

Aplikacja API integracyjnego udostępnia interaktywny Swagger UI:

1. Otwórz: `https://dmapi-intrum-dev.groupad1.com/swagger` (lub odpowiedni adres środowiska).
2. Kliknij przycisk **Authorize** i wybierz schemat **api1**.
3. Uzupełnij `client_id` i `client_secret` — Swagger sam pobierze token i osadzi go w kolejnych wywołaniach.
4. Po poprawnym zalogowaniu ikona kłódki przy metodach zamknie się — możesz wywoływać endpointy z interfejsu.

!!! tip "Jednorazowa autoryzacja na sesję"
    Logowanie w Swaggerze wystarczy wykonać raz na sesję — token jest pamiętany dla kolejnych wywołań w tej karcie przeglądarki.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Środowiska</div>

| Środowisko | Serwer autoryzacji | API integracyjne |
|---|---|---|
| DEV | `https://dmlogin-intrum-dev.groupad1.com/connect/token` | `https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/` |

Adresy pozostałych środowisk (TEST/PROD) zostaną dopisane po wdrożeniu.

</div>
