---
title: "Pierwszy import"
---

# Pierwszy import API-2-API

<div class="api-section" markdown>
<div class="api-section-title">Opis</div>

Przewodnik krok po kroku pokazuje pełny cykl pierwszego importu wykonywanego w trybie API-2-API. Przykład: import nowego klienta (Customer) wraz z jedną sprawą (Case) i kontraktem (Contract) — 3 komunikaty w trzech kolejkach.

W trybie API-2-API każdy import — niezależnie od wielkości — przebiega według tego samego schematu. Punktowa korekta jednej wpłaty i wsad 10 000 spraw używają tych samych endpointów; różnią się tylko liczbą wywołań [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 1 — Pobranie tokenu</div>

```bash
curl -X POST https://dmlogin-intrum-dev.groupad1.com/connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=integration&client_secret={CLIENT_SECRET}&scope=api1&grant_type=client_credentials"
```

Z odpowiedzi pobierz `access_token` i wykorzystaj w nagłówku `Authorization: Bearer <access_token>` we wszystkich kolejnych wywołaniach. Szczegóły — [Autoryzacja](../zalozenia/autoryzacja.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Utworzenie importu</div>

```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/CreateImport \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "ImportTypeId": 12,
    "CreditorId": 10013,
    "PortfolioId": 94,
    "ExternalReference": "tutorial-pierwszy-import"
  }'
```

Odpowiedź zawiera `importId`:
```json
{
  "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "ImportTypeName": "DM Integration API Sample - External Payments Import"
}
```

Zapisz wartość `ImportId` — będzie używana w każdym kolejnym kroku. Szczegóły — [CreateImport](../funkcje-api/importy/create-import.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 3 — Zgłoszenie startu fazy Adding</div>

```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Id": 0,
    "Added": "2026-06-01T10:00:00",
    "Stage": 1,
    "StageStatus": 1,
    "Message": null,
    "StepDetailsList": []
  }'
```

Szczegóły — [SetImportStep](../funkcje-api/importy/set-import-step.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 4 — (Opcjonalnie) Słowniki i reguły walidacji</div>

Klient API może pobrać słowniki walidacyjne aby zwalidować dane przed wysłaniem:

```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetDictionaries" \
  -H "Authorization: Bearer {TOKEN}"
```

Oraz zarejestrować reguły walidacji egzekwowane przez API (sprawdzane per komunikat w trakcie etapu Consumption; krytyczne reguły domyślne działają zawsze, także bez rejestracji):

```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportValidations \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "Rules": [
      { "Id": 3001, "Level": 3 }
    ]
  }'
```

Szczegóły — [GetDictionaries](../funkcje-api/importy/get-dictionaries.md), [SetImportValidations](../funkcje-api/importy/set-import-validations.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 5 — Wysłanie komunikatów</div>

Wysyłamy 3 komunikaty (Customer, Case, Contract) — każdy w osobnym wywołaniu `EnqueueImportMessage`. Pole `message` to **string JSON** (lista obiektów serializowana).

**Customer:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Customer",
    "message": "[{\"ObjectId\": \"cust-001\", \"VersionId\": 1, \"OrderInMessage\": 1, \"Name\": \"Jan Kowalski\", \"Pesel\": \"83041100112\"}]"
  }'
```

**Case:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Case",
    "message": "[{\"ObjectId\": \"case-001\", \"VersionId\": 1, \"OrderInMessage\": 1, \"CustomerObjectId\": \"cust-001\", \"ExternalCaseNumber\": \"774\"}]"
  }'
```

**Contract:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Contract",
    "message": "[{\"ObjectId\": \"contract-001\", \"VersionId\": 1, \"OrderInMessage\": 1, \"CaseObjectId\": \"case-001\", \"Amount\": 250.12, \"Currency\": \"PLN\"}]"
  }'
```

Każde wywołanie zwraca status (`Success` / `Warning` / `Error`). Szczegóły struktury komunikatów — [Komunikaty](../komunikaty/index.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 6 — Zamknięcie fazy</div>

**Wariant A — Walidacja po stronie API** (domyślnie):
```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Stage": 1,
    "StageStatus": 2
  }'
```

API podejmie Stage 2 Validation z regułami z Kroku 4.

**Wariant B — Walidacja po stronie Klienta API** (gdy Klient API zwalidował dane u siebie przed wysłaniem):
```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Stage": 2,
    "StageStatus": 2
  }'
```

API pomija własną walidację i przechodzi bezpośrednio do Stage 4 Consumption.

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 7 — Monitorowanie statusu</div>

Co kilkanaście sekund polluj status importu:
```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportStatus?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c&includeMessagesStatus=true" \
  -H "Authorization: Bearer {TOKEN}"
```

Z odpowiedzi sprawdź pole `IsFinished` — gdy `true`, import jest zakończony. W `CurrentStep` widzisz aktualny etap i jego status. W `ImportMessages` widzisz statusy poszczególnych komunikatów (`StatusId`: 2=SUCCESS, 3=ERROR, 5=INPROGRESS).

Jeśli `CurrentStep.Stage=2 (Validation)` i `StageStatus=3 (Error)` — import wstrzymany na walidacji. Szczegóły błędów w `CurrentStep.StepDetailsList`.

Szczegóły — [GetImportStatus](../funkcje-api/importy/get-import-status.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 8 — (Opcjonalnie) Status pojedynczego komunikatu</div>

Aby sprawdzić status pojedynczego obiektu (np. konkretnej wpłaty), użyj `ObjectId` z komunikatu:

```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetObjectStatusByID?objectId=case-001" \
  -H "Authorization: Bearer {TOKEN}"
```

Szczegóły — [GetObjectStatusByID](../funkcje-api/importy/get-object-status-by-id.md).

</div>
